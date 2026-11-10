# Core principles

Working draft for discussion. These principles are intended for SiMa engineers using Claude or Codex. They establish how development should be backed up, planned, documented, and coordinated. Agent skills and PC setup instructions will be designed later.

The stated principles below capture the requirements expressed so far. Proposed additions and unresolved questions are kept separate so that they do not silently become rules.

## Stated principles

### Remote Git backup is the default

All development work must be backed by a Git repository hosted on a remote server. This applies both to creating a new project without an existing repository and to continuing work in an existing repository.

The user must explicitly initiate any decision to work without remote backup. The agent must never ask whether the user wants to opt out or suggest opting out as a routine alternative.

If the user explicitly requests an exception, the agent must follow up to confirm that the user is really sure they want to proceed without backing up their work. An initial opt-out request alone is insufficient.

### Branches have distinct purposes

| Branch | Purpose |
| --- | --- |
| `main` | Publish the project and share it with others. |
| `develop` | Hold the latest working state of the project. |
| Feature branch | Implement a specific feature addition or extension. |

Development work must never take place directly on `main` or `develop`. The only stated exception is the setup of a new repository.

Every feature addition or extension requires a new branch with a descriptive name. `develop` must represent a working state, rather than merely the most recent changes.

The planning scenario below also requires initial project definition on `develop`. How documentation changes interact with the direct-development restriction remains an explicit open question.

### Documentation is essential

Every project must have an exhaustive description of its purpose and exactly one source of truth document.

The source of truth must contain:

- The purpose and full scope of the project.
- The features that must be supported.
- The current implementation roadmap and development plan.
- Implementation decisions.
- A changelog.

The source of truth must always be on `develop`. There must not be different versions of this document, including different versions on different branches.

This is the intended requirement. Its precise handling in Git remains unresolved; it must not be silently weakened to permit independent branch copies.

Open tasks and work in progress must be tracked through GitHub Issues or equivalent features of the selected hosting provider. The source of truth must remain consistent with the work tracked there.

### Planning precedes implementation

The project begins by using `develop` to define its full scope and create the source of truth document.

Once the scope and development plan are established, the work must be broken into large subtasks. Each large subtask must then be broken into finer tasks using the hosting provider's task hierarchy or equivalent features.

The purpose and input/output contracts of the planned work must be well defined before implementation begins.

Each large subtask must be assigned to a new agent instance. That instance must follow the agreed plan.

### Bug handling depends on where the bug originated

#### A bug introduced earlier in the same subtask

If an agent discovers a bug that it introduced in an earlier step of its current subtask, it must fix the bug in place. It must not create a new issue for that bug.

The correction remains part of the current subtask. The general requirement to document attempts still applies.

#### A bug in a previously finished task

If an agent discovers a bug in a previously finished task, it must:

1. Pause execution of the current subtask.
2. Raise a new bugfix issue.
3. Spawn a subagent to fix that bug first.
4. Merge the completed fix into `develop`.
5. Merge the updated `develop` into the paused feature branch.
6. Continue the original subtask.

This recovery must preserve the requirement that `develop` contains a working state. The checks that establish this state and the authority to approve merges still need to be defined.

#### A problem that invalidates the plan

If a major issue arises, the agent must pause and report it to the user. The user then takes over orchestration and replanning.

An example is discovering that the result of a previously finished task can never satisfy the needs of a downstream task. The agent must not treat such a problem as an ordinary local bugfix and independently continue with a changed project plan.

### Attempts and failed approaches must be documented

Every attempt must be documented with enough detail to explain what was tried and what happened.

Once an approach has been proven to fail, the documentation must prevent the same approach from being tried again. Recording only that an attempt failed is insufficient if another agent cannot recognize and avoid repeating it.

The location of these records, the evidence required to establish failure, and what counts as the same approach still need to be agreed.

## Development scenario

The intended sequence is:

1. Establish a remotely hosted Git repository, or identify the existing repository and its remote.
2. During initial setup, establish the project branches.
3. Define the project scope and create the source of truth on `develop`.
4. Agree on the roadmap, implementation decisions, development plan, and input/output contracts.
5. Convert the plan into large issues and finer tasks using the hosting provider's features.
6. Start a new agent instance for a large subtask and create a descriptively named feature branch for the feature addition or extension.
7. Follow the plan and document attempts as work proceeds.
8. Fix bugs introduced within the current subtask in place, without creating separate issues.
9. For bugs in finished tasks, pause, raise a bugfix issue, have a subagent fix it, merge into `develop`, bring `develop` into the feature branch, and resume.
10. Escalate problems that invalidate the plan to the user for orchestration and replanning.
11. Complete the work while preserving a working `develop` branch and using `main` exclusively for publication.

The completion criteria, merge process, source of truth updates, and release process are not yet defined.

## Proposed additions for discussion

### Additional source of truth content

The following would help an engineer or a fresh agent understand the project without reconstructing its history:

- Goals, success criteria, exclusions, and project constraints.
- Architecture, component responsibilities, and the boundaries between them.
- Input/output contracts, including data formats, units, shapes, types, and failure behavior where relevant.
- The supported SiMa hardware, SDK and tool versions, and relevant environment requirements.
- Verification and acceptance criteria, with links to the evidence supporting the current working state.
- Decision rationale, including alternatives rejected and the reasons for rejecting them.
- Known limitations, unresolved questions, and dependencies on other work.
- Links to issues, task dependencies, and the records of attempts.

The boundaries between this document, issue records, and supporting evidence need to be agreed before these additions become requirements.

### Information needed to understand an attempt

A useful attempt record could include:

- The task, intended outcome, and hypothesis being tested.
- The approach and the reason for choosing it.
- The relevant source revision, environment, tool versions, configuration, and inputs.
- The actions or commands needed to reproduce the attempt.
- The observed result and links to logs, tests, or other evidence.
- The conclusion, including whether the approach was proven to fail or remains inconclusive.
- Enough identifying context for another agent to recognize the approach and avoid repeating a proven failure.

### Information needed for a fresh agent instance

A large subtask could provide its new agent instance with:

- Its purpose, scope, acceptance criteria, and input/output contracts.
- Links to the source of truth and the relevant issue hierarchy.
- Its dependencies and the work already completed.
- Known failed approaches relevant to the subtask.
- The current repository and branch state.

These are proposed requirements for task context, not a prescribed skill or file structure.

## Open questions

### How should the single source of truth work across branches

Git preserves earlier revisions, and a branch created from `develop` normally inherits the files committed there. Should the requirement prohibit only competing current documents, with `develop` always authoritative, or should the source of truth be absent from feature and publication branches entirely? How should agents obtain its current content while working on another branch?

### Can planning and documentation be changed directly on develop

Does the prohibition on direct development apply to implementation only, allowing planning and source of truth updates directly on `develop`? Or must documentation changes also use separate branches after repository setup?

### What establishes that develop is working

Which checks are mandatory before a merge? For SiMa work, when are hardware execution, numerical accuracy, performance measurements, or environment reproduction required? Can a project be working while planned features remain unimplemented, provided its accepted current behavior passes the required checks?

### How are documentation and issue records divided

Which information is authoritative in the source of truth, and which information is authoritative in the hosting provider's issues? Where should detailed attempt records live so they can be found without creating competing versions of project state?

### When must work be committed and pushed

What events require a remote backup? How should agents handle network outages, a missing remote, or failed authentication? Does a confirmed opt-out waive remote hosting only, or also local Git use, and for how long does the exception apply?

### How do large subtasks map to branches and agents

Is each large subtask exactly one feature branch, or can a feature contain several large subtasks? May several agents work in parallel? What marks a task as finished for the bug-handling rule?

### Who approves fixes and merges

May an agent merge a verified feature or bugfix into `develop` autonomously, or is human approval required? What happens when the agent discovers that a bugfix changes a contract, conflicts with other work, or requires replanning?

### What counts as a repeated failed approach

How should agents distinguish repeating a proven failure from a materially different attempt after a code change, corrected configuration, new SDK version, different hardware, or new evidence? Who may authorize reconsidering an approach?

### What is the boundary of a development project

Do temporary experiments, exploratory notebooks, one-off scripts, and hardware investigations each need a separate remote repository, or may they live in an existing development repository?
