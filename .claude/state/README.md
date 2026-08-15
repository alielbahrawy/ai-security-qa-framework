# Engagement State

## Purpose

The `state/` directory contains the persistent state used by the framework to resume security and QA engagements safely after interruption.

The state layer exists to prevent the framework from restarting an engagement from the beginning after:

- Internet interruption
- Terminal closure
- Claude Code restart
- Operating system restart
- Power loss
- Tool failure
- Agent interruption
- Unexpected process termination

The framework must be able to determine:

- What has already been completed
- What is currently running
- What was interrupted
- What failed
- What remains to be executed
- What findings already exist
- What evidence has already been collected
- What the next safe action is

---

## Architecture

The state layer is controlled by the Security Orchestrator.

```text
Claude Code
    |
    v
Security Orchestrator
    |
    +----------------------+
    |                      |
    v                      v
Load State            Determine Action
    |                      |
    +----------+-----------+
               |
               v
          Execute Task
               |
               v
          Collect Result
               |
               v
         Validate Result
               |
               v
        Write Checkpoint
               |
               v
           Save State
               |
               v
         Continue Workflow
```

The state layer does not replace the workflow.

It records the workflow's current execution state.

---

## Single Source of Truth

The engagement state is the authoritative source for the current execution state of an engagement.

Agents must not maintain independent conflicting copies of the engagement state.

The following components consume or update information through the Orchestrator:

- Security Agent
- Code Review Agent
- QA Agent
- Vulnerability Analyst
- Report Generator
- Security tools
- QA tools

---

## State Schema

The structure of the engagement state is defined by:

`../schemas/engagement-state.schema.json`

All persisted engagement state must conform to that schema.

The schema defines the expected structure for:

- Engagement identity
- Scope
- Status
- Current phase
- Current task
- Task history
- Checkpoints
- Findings
- Evidence
- Tool executions
- Next action
- Recovery information

---

## Engagement Isolation

Each engagement must maintain independent state.

State from one engagement must never be reused for another engagement unless explicitly requested and validated.

Conceptually:

```text
Engagement A
    |
    +-- State A

Engagement B
    |
    +-- State B

Engagement C
    |
    +-- State C
```

The Orchestrator must identify the active engagement before loading state.

---

## Lifecycle

An engagement follows this lifecycle:

```text
NEW
 |
 v
INITIALIZED
 |
 v
IN_PROGRESS
 |
 +----------------------+
 |                      |
 v                      v
COMPLETED            INTERRUPTED
 |                      |
 v                      v
CLOSED              RECOVERY
                        |
                        v
                    IN_PROGRESS
```

A task may independently be:

```text
PENDING
RUNNING
COMPLETED
FAILED
INTERRUPTED
SKIPPED
```

`COMPLETED` must only be used when the task result has been successfully recorded and persisted.

---

## Checkpoint Principle

A checkpoint represents a known valid recovery point.

The Orchestrator should create a checkpoint after every meaningful state transition.

Examples:

```text
Recon completed
Static analysis completed
QA testing completed
Dynamic testing completed
Finding validated
Report generated
```

A checkpoint should contain enough information for the Orchestrator to determine the next safe action.

---

## Recovery Principle

When Claude Code starts or an engagement is resumed, the Orchestrator must load the existing state before starting new work.

The recovery process is:

```text
Load State
    |
    v
Validate State
    |
    v
Identify Last Checkpoint
    |
    v
Inspect Current Task
    |
    +----------------------+
    |                      |
 COMPLETED             INTERRUPTED
    |                      |
    v                      v
Continue Next Task    Validate Previous Result
                           |
                    +------+------+
                    |             |
                 Valid          Invalid
                    |             |
                    v             v
                 Reuse          Rerun Safely
```

The framework must never assume that an interrupted task completed.

---

## Resume Rules

When resuming an engagement:

1. Load the persisted state.
2. Validate the state against the schema.
3. Confirm the engagement identity.
4. Confirm the authorized scope.
5. Inspect the current phase.
6. Inspect the current task.
7. Inspect the last checkpoint.
8. Determine whether the previous task completed successfully.
9. Reuse valid persisted results.
10. Re-run only incomplete or invalid tasks.
11. Continue from the safest valid point.

---

## Idempotency

Tasks should be designed to be safely repeatable whenever possible.

The Orchestrator must avoid blindly executing a task twice when a valid result already exists.

Before rerunning a task, determine:

```text
Does a valid result already exist?
        |
       YES
        |
        v
Can the result be safely reused?
        |
       YES
        |
        v
Reuse result

       NO
        |
        v
Execute task again
```

If a tool does not expose enough information to determine whether an interrupted execution completed, the Orchestrator must treat the execution as uncertain and perform a safe verification or rerun.

---

## Interrupted Tasks

An interrupted task must never automatically become `COMPLETED`.

Example:

```text
Dynamic Security Testing
        |
        v
      RUNNING
        |
        X
   System Shutdown
```

After recovery:

```text
RUNNING
   |
   v
INTERRUPTED
   |
   v
Verify Result
   |
   +----------+
   |          |
 Valid      Invalid
   |          |
   v          v
 Reuse      Rerun
```

The framework must preserve the interrupted state until recovery determines the correct outcome.

---

## Findings Persistence

Findings must persist independently from the current task.

A finding must not disappear because:

- An agent stops
- A tool fails
- The terminal closes
- Claude Code restarts
- The engagement is resumed

Validated findings must remain available to:

- Vulnerability Analyst
- Report Generator
- Regression workflows

---

## Evidence Persistence

Evidence associated with findings should remain available after recovery.

Evidence may include:

- Tool output
- Requests
- Responses
- File paths
- Line numbers
- Reproduction information
- Test results
- Runtime observations
- Correlation information

The state should reference evidence rather than unnecessarily duplicating large tool outputs.

---

## Tool Execution Tracking

Tool executions must be distinguishable from task state.

Example:

```text
Task:
    API Security Testing

Tool:
    pentest-ai

Execution:
    EXEC-001

Status:
    COMPLETED
```

This allows the Orchestrator to determine whether a previous tool execution already produced usable evidence.

---

## State Integrity

The Orchestrator must validate persisted state before using it.

Invalid or corrupted state must not be treated as authoritative.

If state validation fails:

```text
Invalid State
    |
    v
Do NOT blindly overwrite
    |
    v
Attempt Recovery
    |
    v
Use Last Valid Checkpoint
```

Previous valid state should be preserved whenever possible.

---

## Atomic Persistence

State updates should be written atomically.

The framework should avoid leaving partially written state as the only available state.

Conceptually:

```text
Current State
    |
    v
Create Temporary State
    |
    v
Validate
    |
    v
Commit
    |
    v
New Valid State
```

If a write operation is interrupted, the previous valid state should remain recoverable.

---

## State and Agents

Agents perform specialized work.

They do not independently control the global engagement lifecycle.

The responsibility boundary is:

```text
Security Orchestrator
    |
    +-- Owns engagement state
    +-- Owns task sequencing
    +-- Owns recovery
    +-- Owns checkpoints
    |
    +--> Security Agent
    +--> Code Review Agent
    +--> QA Agent
    +--> Vulnerability Analyst
    +--> Report Generator
```

The Orchestrator coordinates state transitions between these components.

---

## State and Rules

The state layer works together with:

```text
rules/workflow.md
rules/tool-selection.md
rules/severity-model.md
```

The relationship is:

```text
Workflow Rules
      |
      v
What should happen?

Tool Selection Rules
      |
      v
Which capability should execute?

Engagement State
      |
      v
What already happened?

Severity Model
      |
      v
How should validated findings be classified?
```

These components must remain consistent.

---

## State and Knowledge

The knowledge layer provides domain knowledge.

The state layer records execution state.

They must not be confused.

```text
knowledge/
    |
    +-- What we know

state/
    |
    +-- What happened
```

Knowledge is reusable across engagements.

Engagement state is specific to an engagement.

---

## State and Reporting

The Report Generator must consume validated findings and their associated engagement state.

The reporting flow is:

```text
Agents
   |
   v
Evidence
   |
   v
Vulnerability Analyst
   |
   v
Validated Findings
   |
   v
Engagement State
   |
   v
Report Generator
```

A report must never be generated from raw unvalidated tool output when the workflow requires validation.

---

## Recovery After Network Failure

Network failure must not automatically invalidate completed local work.

For example:

```text
Semgrep
   |
   v
COMPLETED
   |
   v
Checkpoint Saved
   |
   X
Internet Lost
```

After recovery:

```text
Load State
   |
   v
Semgrep Result = Valid
   |
   v
Do NOT rerun Semgrep unnecessarily
   |
   v
Continue with next required task
```

If a network-dependent tool was interrupted:

```text
Tool = INTERRUPTED
```

The Orchestrator must verify whether usable results exist before deciding whether to rerun it.

---

## Recovery After System Shutdown

Unexpected shutdown must be treated as an interruption, not as successful completion.

The Orchestrator should resume from the most recent valid checkpoint.

Example:

```text
Checkpoint 1
    |
    v
Checkpoint 2
    |
    v
Task Running
    |
    X
Power Loss
```

Recovery:

```text
Load Checkpoint 2
    |
    v
Inspect Interrupted Task
    |
    v
Resume Safely
```

---

## No False Completion

The framework must never mark work as completed merely because:

- A tool was started
- An agent was invoked
- A command was issued
- A task was planned
- A process exited unexpectedly

Completion requires a valid result and persisted state.

---

## State Transition Principle

Every important transition should follow:

```text
Before
  |
  v
Execute
  |
  v
Observe
  |
  v
Validate
  |
  v
Persist
  |
  v
Continue
```

Persistence happens after meaningful validated progress.

---

## Final Principle

The state system exists to make the framework resilient.

The framework should behave as:

```text
             Work
              |
              v
         Save Progress
              |
              v
             Work
              |
              v
         Save Progress
              |
              X
        INTERRUPTION
              |
              v
         Load State
              |
              v
       Recover Safely
              |
              v
        Continue Work
```

The goal is not merely to remember the last command.

The goal is to preserve the logical state of the engagement so the Security Orchestrator can make an informed decision about where and how to continue.

The authoritative schema for this state is:

`../schemas/engagement-state.schema.json`