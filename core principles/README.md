# Core principles

Working draft for discussion. These principles are intended for SiMa engineers using Claude or Codex. They establish how development should be backed up, planned, documented, and coordinated. Agent skills and PC setup instructions will be designed later.

The stated principles below capture the requirements expressed so far. Proposed additions and unresolved questions are kept separate so that they do not silently become rules.

## Stated principles

### Remote Git backup is the default

All development work must be backed by a Git repository hosted on a remote server. This applies both to creating a new project without an existing repository and to continuing work in an existing repository.

Agents must commit and push their work after meaningful steps, including while implementation is still in progress. Remote backup must not wait until the task is complete or ready for review.

The user must explicitly initiate any decision to work without remote backup. The agent must never ask whether the user wants to opt out or suggest opting out as a routine alternative.

If the user explicitly requests an exception, the agent must follow up to confirm that the user is really sure they want to proceed without backing up their work. An initial opt-out request alone is insufficient.

### Branches have distinct purposes

| Branch | Purpose |
| --- | --- |
| `main` | Publish the project and share it with others. |
| `develop` | Hold the latest working state of the project and its authoritative plan. |
| Feature branch | Implement a specific feature addition or extension. |

Implementation work must never take place directly on `main` or `develop`. The only stated exception for implementation work is the setup of a new repository.

Roadmap changes and planning may be performed directly on `develop`, including after initial repository setup.

Every feature addition or extension requires a new branch with a descriptive name. `develop` must represent a working state, rather than merely the most recent changes.

### Squash merges keep develop history concise

Approved feature and bugfix branches must be squash merged into `develop`. Each approved branch contributes one commit to `develop`, combining its implementation commits into a single completed change.

Agents must continue to commit and push meaningful steps on their working branches. The affected-area testing, Ponytail review, code explanation, and explicit user approval requirements still apply before the squash merge.

Successful and unsuccessful attempts must remain documented independently of the condensed commit history. Planning changes made directly on `develop` remain individual commits.

### Documentation is essential

Every project must have an exhaustive description of its purpose and exactly one source of truth document.

The source of truth must contain:

- The purpose and full scope of the project.
- The features that must be supported.
- The current implementation roadmap and development plan.
- Implementation decisions.
- A changelog.

The document on `develop` is the sole authoritative current source of truth. Agents must consult that version regardless of their working branch.

Git may retain historical revisions, and other branches may contain inherited snapshots. These snapshots are not authoritative and must never be maintained as independent versions. Changes to the source of truth must be coordinated on `develop`.

Open tasks and work in progress must be tracked through GitHub Issues or equivalent features of the selected hosting provider. The source of truth must remain consistent with the work tracked there.

### Planning precedes implementation

The project begins by using `develop` to define its full scope and create the source of truth document.

The core features must be decided during project planning. Once the scope and development plan are established, the work must be broken into large subtasks. Each large subtask must then be broken into finer tasks using the hosting provider's task hierarchy or equivalent features.

The purpose and input/output contracts of the planned work must be well defined before implementation begins.

Each large subtask must be assigned to a new agent instance. The concrete goals, acceptance criteria, and relevant verification method for that instance must be defined when it starts. The instance must follow the agreed plan.

### Parallel execution requires independent work

Independent large subtasks may be executed in parallel by separate agent instances.

When a bug already present in `develop` blocks tasks, those tasks must be marked as blocked and their agents must pause the blocked work. The user orchestrates affected agents and designates the worker responsible for the fix. Exactly one agent at a time may work on a given fix, even when multiple agents discover the same bug.

Bug handling uses existing GitHub issues and human orchestration. An automated coordinator is not part of this workflow. How other affected running agents are informed and paused remains an open question.

### Exploration and best effort optimization are valid tasks

A task may explore options or attempt an optimization without a predefined technical outcome or numerical improvement target.

When its agent instance starts, the concrete goal may be to investigate options, compare approaches, or make a best effort to improve the current result and document the findings. The goal must reflect the exploratory nature of the work rather than require an outcome that is not yet known to be achievable.

### The user shapes optimization targets and methods

Before pursuing an optimization, the agent must discuss and agree with the user on the target, the approach, and when the optimization can stop. The target may be exploratory or best effort; it need not promise a fixed numerical improvement.

The user must shape how the optimization is achieved unless they explicitly delegate those choices to the agent. The agent must follow the agreed direction. Agreement on an approach authorizes its routine steps; material changes to that direction must be discussed with the user unless covered by explicit delegation.

The agreed target, approach, stopping conditions, and any delegation must be documented.

### Verification covers the affected areas

Before declaring a task finished and merging its implementation into `develop`, the agent must provide evidence that the acceptance criteria agreed for that instance are satisfied.

Only affected rows, features, and modules must be tested. Testing the full functionality of the project on every change is not required. Verification of existing functionality is limited to the areas affected by the change.

The agent must document which affected areas were tested and the results. The checks appropriate to each task must reflect its goals, including whether it is implementation, exploration, or best effort optimization.

### Ponytail review is required before merging

Before any branch may be merged into `develop`, its changes must receive an extensive code review using the Ponytail plugin. This requirement applies to feature branches and bugfix branches.

How review findings are resolved remains an open question. The user must approve the merge as described below.

### The user approves merges after a full code explanation

Every merge into `develop` requires explicit user approval. Passing the affected-area tests and completing the Ponytail review do not by themselves authorize a merge.

Before requesting approval, the agent must explain the implementation compared with `develop`. The explanation must account for every line of code that has been changed or added. For every such line, it must describe in simple terms what the code does, its purpose, and a good reason why it should exist.

Related lines may be explained together. An explanation may cover a whole block, method, or class, provided every changed or added line's purpose and reason for existing are covered. A separate explanation for each individual line is not required.

The user has full authority to decide whether the implementation meets their standards. They may request changes or a complete overhaul. The agent must carry out the requested revisions and present the revised implementation for approval before merging. The revised implementation remains subject to the same verification and review requirements.

### Bug handling depends on whether the bug is in develop

Whether the bug is present in `develop` determines the workflow, regardless of which branch the agent is using when it discovers the bug.

#### A bug that has not reached develop

If a bug exists only in work that has not been merged into `develop`, the agent must fix it in its working branch without creating a separate bug issue or asking the user to delegate a separate fixing worker.

The correction remains part of the branch's implementation work. The general requirement to document attempts still applies.

#### A bug already in develop

If an agent discovers a bug already present in `develop`, it must:

1. Pause execution of the current subtask.
2. Search the existing GitHub issues for the same bug, or use the equivalent issue tracker for the hosting provider.
3. If a matching issue exists, use that issue. The discovering agent may add further context to the same issue.
4. If no matching issue exists, ask the user to create an issue and delegate a worker to fix it. Issue creation and worker delegation are performed by the user under this workflow.
5. Let the user orchestrate the fixing effort. If multiple agents discover the same bug at the same time, the user resolves the overlap and delegates a single fixing worker.
6. Have the fixing worker present the required explanation of the fix and obtain explicit user approval for the merge.
7. Squash merge the approved fix into `develop`.
8. Merge the updated `develop` into the paused feature branch.
9. Continue the original subtask.

The bugfix must meet the verification requirements for its affected areas and receive the required Ponytail review before the agent requests approval to merge.

#### A problem that invalidates the plan

If a major issue arises, the agent must pause and report it to the user. The user then takes over orchestration and replanning.

An example is discovering that the result of a previously finished task can never satisfy the needs of a downstream task. The agent must not treat such a problem as an ordinary local bugfix and independently continue with a changed project plan.

### Successful and unsuccessful attempts must be documented

Every attempt must be documented with enough detail to explain what was tried and what happened.

When a difficult situation requires previous knowledge or exploration, successful approaches must be preserved so that the engineer and their agents can find and reuse them in future projects. This knowledge must remain discoverable beyond the current agent instance and task.

Reusable knowledge is maintained individually for each engineer. Their successful and unsuccessful approaches must be discoverable and reusable across that engineer's projects.

Unsuccessful attempts must also be documented. Once an approach has been proven to fail, the documentation must prevent the same approach from being tried again on the same software basis. Recording only that an attempt failed is insufficient if another agent cannot recognize and avoid repeating it.

An update or a changed version of the software basis is an exception: the previously unsuccessful approach may be tried again under the changed software basis. The earlier attempt and its outcome must remain documented.

The location and discovery of these records, the evidence required to establish failure, and how approaches and software versions are identified still need to be agreed.

## Development scenario

The intended sequence is:

1. Establish a remotely hosted Git repository, or identify the existing repository and its remote.
2. During initial setup, establish the project branches.
3. Define the project scope and create the source of truth on `develop`.
4. Agree on the core features, roadmap, implementation decisions, development plan, and input/output contracts.
5. Convert the plan into large issues and finer tasks using the hosting provider's features.
6. Start a new agent instance for a large subtask, define its concrete goals, acceptance criteria, and verification method, and create a descriptively named feature branch for the feature addition or extension. Goals may cover exploration or best effort optimization. For optimization work, agree with the user on the target, approach, and stopping conditions; the user shapes the methods unless they explicitly delegate those choices.
7. Follow the plan and document attempts as work proceeds. Preserve successful approaches for reuse in future projects and unsuccessful approaches to prevent repeated failures.
8. Fix bugs in work that has not yet reached `develop` in the working branch, without creating separate bug issues or asking for a separate fixing worker.
9. For bugs already present in `develop`, pause and search the existing issues. Add context to a matching issue, or ask the user to create an issue and delegate a fixing worker when no match exists. The user orchestrates simultaneous discoveries and assigns a single fixing worker. The fix requires verification of the affected areas, Ponytail review, the code explanation, and explicit user approval before squash merging into `develop`. Bring the corrected `develop` into the feature branch before resuming. Other tasks blocked by the same bug must also be marked blocked and their agents paused.
10. Escalate problems that invalidate the plan to the user for orchestration and replanning.
11. Verify the affected areas against the instance's agreed acceptance criteria and obtain an extensive Ponytail code review. Explain every changed or added line of code compared with `develop`, carry out any revisions requested by the user, and obtain explicit user approval before squash merging the branch into `develop`.
12. Preserve a working `develop` branch and use `main` exclusively for publication.

The handling of exploratory outcomes without implementation improvements, review findings, explanations of deleted code, source of truth updates, and release process are not yet fully defined.

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
- Known successful and unsuccessful approaches relevant to the subtask, including their software versions and applicability to the current work.
- The current repository and branch state.

These are proposed requirements for task context, not a prescribed skill or file structure.

## Open questions

### How are other documentation updates handled

Roadmap changes and planning may be performed directly on `develop`. Should other documentation updates, such as changelog entries and descriptions of completed implementation work, also be made directly on `develop`, or should they accompany the relevant feature or bugfix branch?

### How are verification requirements selected

How should agents identify the affected rows, features, and modules? Within that scope, when are hardware execution, numerical accuracy, performance measurements, or environment reproduction required? Who agrees on the task's acceptance criteria and verification method when its agent instance starts?

### How is exploration without implementation improvement concluded

If the agreed stopping conditions are reached and exploration produces useful knowledge but no implementation improvement, what deliverable closes the task, and is human sign-off required?

### How is per engineer knowledge discovered across projects

Where should an engineer's successful and unsuccessful approaches be kept so that a new agent instance working on another of their projects can find them? How should project records link to that engineer's reusable knowledge while keeping each project's source of truth authoritative?

### How are documentation and issue records divided

Which information is authoritative in the source of truth, and which information is authoritative in the hosting provider's issues? Where should detailed attempt records live so they can be found without creating competing versions of project state?

### How are planning commits and completed branches managed

How should direct planning updates on `develop` be grouped, and how long should the detailed history on completed working branches be retained after a squash merge?

### How are backup failures and exceptions handled

How should agents handle network outages, a missing remote, or failed authentication? Does a confirmed opt-out waive remote hosting only, or also local Git use, and for how long does the exception apply?

### How do large subtasks map to branches and agents

Is each large subtask exactly one feature branch, or can a feature contain several large subtasks?

### How are other affected agents informed and paused

Under human orchestration, should the user notify and pause other affected agents, or should agents also check the issue tracker at agreed checkpoints? Which tasks should pause when the impact of a bug is not yet known?

### How are review findings handled

Which Ponytail findings must be resolved before requesting user approval, and how should unresolved findings be presented to the user? What happens when the agent discovers that a bugfix changes a contract, conflicts with other work, or requires replanning?

### How should deleted code be explained

Should the explanation also cover code that has been deleted, including why it was removed and how any affected behavior is handled?

### What counts as a repeated failed approach

How should agents identify matching approaches and the software basis on which they were tried? How should an update or software version change that permits a retry be recorded so the retry can be distinguished from repeating an unchanged failed approach?

### What is the boundary of a development project

Do temporary experiments, exploratory notebooks, one-off scripts, and hardware investigations each need a separate remote repository, or may they live in an existing development repository?
