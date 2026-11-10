# Core principles

Working draft for discussion. These principles are intended for SiMa engineers using Claude or Codex. They establish how development should be backed up, planned, documented, and coordinated. Agent skills and PC setup instructions will be designed later.

The stated principles below capture the requirements expressed so far. Proposed additions and unresolved questions are kept separate so that they do not silently become rules.

## Scope and repository selection

These principles define the workflow for new projects started under this approach. They continue to govern those projects as development proceeds. They are not applied retroactively to pre-existing projects.

When working in an existing project managed by others, use its repository and follow the owners' established rules and procedures. The new-project rules below do not replace that project's branching, documentation, review, or release workflow.

Use the repository that already exists for the work. Related experiments, exploratory scripts, and one-off tests stay in that project's repository, subject to its workflow. Create a new project repository only when no repository exists for the work.

## Stated principles

### Remote Git backup is the default

All development work in projects governed by these principles must be backed by a Git repository hosted on a remote server. This applies to initial setup and subsequent development, including related experiments and exploratory work.

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

### The user explicitly initiates releases

`develop` is published to `main` only for a release that the user explicitly requests or manually triggers.

Approval to merge a feature or bugfix into `develop` does not authorize a release. Completing such a merge must not automatically publish the project to `main`.

Each release published on `main` must have a release version number and a Git tag identifying the released commit. Release version numbers and release tags are reserved for releases on `main`; ordinary updates to `develop`, feature branches, bugfix branches, and subtask branches are identified by their commits without assigning release versions or release tags.

### Merge each feature as a whole

A feature's large subtasks and finer tasks are completed and integrated on its feature branch. The complete agreed feature must be ready before it is presented for a consolidated merge review and user approval.

The feature enters `develop` through one squash merge after all its agreed subtasks are complete. Individual subtasks must not be merged into `develop` separately. This reduces the number of merge approval interactions with the user.

Each large subtask still uses a new agent instance, and independent subtasks may run in parallel. Frequent commits and pushes continue throughout the work.

Agents may merge subtask branches into the feature branch without user approval as part of the approved plan. These internal merges do not authorize a merge into `develop`; the complete feature still requires its consolidated review and explicit user approval.

### Squash merges keep develop history concise

Approved feature and bugfix branches must be squash merged into `develop`. Each approved branch contributes one commit to `develop`, combining its implementation commits into a single completed change.

Agents must continue to commit and push meaningful steps on their working branches. The affected-area testing, Ponytail review, code explanation, and explicit user approval requirements still apply before the squash merge.

Successful and unsuccessful attempts must remain documented independently of the condensed commit history. Planning changes made directly on `develop` remain individual commits.

### Preserve decisions before deleting merged branches

After a successful squash merge into `develop`, the merged feature or bugfix branch must be deleted.

Before the branch is deleted, all decisions made during its work must be documented in the authoritative source of truth on `develop`. This includes how the feature is implemented and how its interactions are intended to work. The decision record must remain available after the working branch is gone.

Code and the documentation recording its decisions may arrive on `develop` together in the same squash merge.

### Documentation is essential

Every project governed by these principles must have an exhaustive description of its purpose and exactly one source of truth document.

The source of truth must have a table of contents and stable sections covering:

| Section | Contents |
| --- | --- |
| Purpose and scope | Why the project exists, its goals, boundaries, and exclusions. |
| Features and interactions | Required behavior, clearly identifying what is implemented and what is planned. |
| Architecture and contracts | Component responsibilities, inputs, outputs, and how components interact. |
| Working environment | Relevant hardware, software, dependencies, and known limitations. Recording exact versions is optional. |
| Roadmap | The implementation direction and development plan, with links to the issue hierarchy. |
| Implementation decisions | What was decided, why, and which components or interactions are affected. |
| Verification | How behavior is verified, with links to recorded results. |
| Changelog | A concise entry for each merged change, linked to its issue or pull request. |

The document on `develop` is the sole authoritative current source of truth. Agents must consult that version regardless of their working branch.

Git may retain historical revisions, and other branches may contain inherited snapshots. These snapshots are not authoritative and must never be maintained as independent versions. Changes to the source of truth must be coordinated on `develop`.

A feature or bugfix branch may prepare proposed documentation changes alongside its code. Those changes become authoritative when merged into `develop`; until then, the current document on `develop` remains authoritative.

Open tasks and work in progress must be tracked through GitHub Issues or equivalent features of the selected hosting provider. Detailed task status belongs in the issue tracker. The source of truth contains the roadmap and links to the issues, and must remain consistent with the work tracked there.

### Keep current documentation navigable

When behavior changes, update the relevant section of the authoritative document so that each feature or contract has one current description.

Implementation decisions must have stable identifiers. When a decision changes, mark the previous decision as superseded and preserve its rationale in a historical appendix within the same document.

A short project entry point links to the authoritative document on `develop`. Concrete filenames and storage layouts will be chosen later.

### Supporting records preserve evidence and reusable knowledge

Detailed experiment records, test results, reproduction details, and attempt histories may be kept separately and linked from the authoritative document. Each attempt record must describe the approach, result, and conclusion.

Recording software versions, source revisions, artifact checksums, and similar technical identifiers is optional. This does not change the requirement for release version numbers and Git tags when publishing a release on `main`.

The project document defines the current project and contains the resulting decisions. Supporting records explain what was observed and why those decisions were made; they do not establish competing current requirements or behavior.

Reusable techniques belong in the engineer's personal knowledge collection. Project-specific choices remain in the project's authoritative document.

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

### The user decides when to conclude exploration

When exploration or best effort optimization reaches an agreed stopping point, the agent must pause that work and report its attempts and findings, including when no improvement was achieved.

The user decides whether to continue, conclude the task, or move on to other work. Closing the task or its issue and moving on follow the user's decision.

### Verification covers the affected areas

Before declaring a task finished and merging its implementation into `develop`, the agent must provide evidence that the acceptance criteria agreed for that instance are satisfied.

Only affected rows, features, and modules must be tested. Testing the full functionality of the project on every change is not required. Verification of existing functionality is limited to the areas affected by the change.

The agent must document which affected areas were tested and the results. The checks appropriate to each task must reflect its goals, including whether it is implementation, exploration, or best effort optimization.

### Ponytail review is required before merging

Before any branch may be merged into `develop`, its changes must receive an extensive code review using the Ponytail plugin. This requirement applies to feature branches and bugfix branches.

The agent must fix review problems that can be resolved within the approved plan before presenting the complete feature or bugfix to the user. The version presented for approval must satisfy the affected-area verification and Ponytail review requirements.

Problems that require a change to the approved plan must be discussed with the user. Bugs already present in `develop` follow the bug-handling workflow below.

The user must approve the merge as described below.

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

After the squash merge, the bugfix branch must be deleted once its decisions have been recorded on `develop`.

#### A problem that invalidates the plan

If a major issue arises, the agent must pause and report it to the user. The user then takes over orchestration and replanning.

An example is discovering that the result of a previously finished task can never satisfy the needs of a downstream task. The agent must not treat such a problem as an ordinary local bugfix and independently continue with a changed project plan.

### Successful and unsuccessful attempts must be documented

Every attempt must be documented with enough detail to explain what was tried and what happened.

When a difficult situation requires previous knowledge or exploration, successful approaches must be preserved so that the engineer and their agents can find and reuse them in future projects. This knowledge must remain discoverable beyond the current agent instance and task.

Reusable knowledge is maintained individually for each engineer. Their successful and unsuccessful approaches must be discoverable and reusable across that engineer's projects.

Unsuccessful attempts must also be documented. Once an approach has been proven to fail, the documentation must prevent the same approach from being tried again on the same software basis. Recording only that an attempt failed is insufficient if another agent cannot recognize and avoid repeating it.

An update or a changed version of the software basis is an exception: the previously unsuccessful approach may be tried again under the changed software basis. The earlier attempt and its outcome must remain documented.

The location and discovery of these records, the evidence required to establish failure, and how past approaches are recognized still need to be agreed.

## Development scenario for new projects

The intended sequence for a project started under these principles is:

1. Establish remote backing for the project's Git repository, reusing the repository if one already exists. Create a new repository only when none exists for the work.
2. During initial setup, establish the project branches.
3. Define the project scope and create the source of truth on `develop`.
4. Agree on the core features, roadmap, implementation decisions, development plan, and input/output contracts.
5. Convert the plan into large issues and finer tasks using the hosting provider's features.
6. Create a descriptively named branch for the complete feature addition or extension. Use a new agent instance for each large subtask and define its concrete goals, acceptance criteria, and verification method when it starts. Goals may cover exploration or best effort optimization. For optimization work, agree with the user on the target, approach, and stopping conditions; the user shapes the methods unless they explicitly delegate those choices.
7. Follow the plan and document attempts as work proceeds. Agents may merge subtask work into the feature branch without asking for user approval. Preserve successful approaches for reuse in future projects and unsuccessful approaches to prevent repeated failures. For exploration and best effort optimization, report the findings at the agreed stopping point and let the user decide whether to continue, conclude, or move on.
8. Fix bugs in work that has not yet reached `develop` in the working branch, without creating separate bug issues or asking for a separate fixing worker.
9. For bugs already present in `develop`, pause and search the existing issues. Add context to a matching issue, or ask the user to create an issue and delegate a fixing worker when no match exists. The user orchestrates simultaneous discoveries and assigns a single fixing worker. The fix requires verification of the affected areas, Ponytail review, the code explanation, and explicit user approval before squash merging into `develop`. Bring the corrected `develop` into the feature branch before resuming. Other tasks blocked by the same bug must also be marked blocked and their agents paused.
10. Escalate problems that invalidate the plan to the user for orchestration and replanning.
11. Once all agreed subtasks are complete and integrated on the feature branch, verify the affected areas against the agreed acceptance criteria and obtain an extensive Ponytail code review of the complete feature. Fix review problems within the approved plan before presenting the result to the user, keeping verification and review applicable to the final version. Explain every changed or added line of code compared with `develop`, carry out any revisions requested by the user, and obtain explicit user approval before squash merging the complete feature into `develop` in one merge.
12. Ensure all decisions made during the branch's work, including implementation and intended interaction behavior, are documented in the authoritative source of truth on `develop`. Code and documentation may arrive together in the same squash merge. After the successful squash merge and once this record is in place, delete the merged working branch.
13. Preserve a working `develop` branch. Publish it to `main` only for a release explicitly requested or manually triggered by the user, assigning a release version number and Git tag to the released commit on `main`.

Explanations of deleted code, documentation filenames and storage locations, and the detailed release procedure are not yet fully defined.

## Proposed additions for discussion

### Additional detail for decision records

Decision records could also include rejected alternatives, the reasons for rejecting them, and links to supporting evidence and relevant issues.

### Information needed to understand an attempt

Beyond the required approach, result, and conclusion, an attempt record could also include:

- The task, intended outcome, and hypothesis being tested.
- The reason for choosing the approach.
- Environment details, software and tool versions, source revisions, artifact checksums, configuration, and inputs.
- The actions or commands needed to reproduce the attempt.
- Links to logs, tests, or other evidence supporting the observed result.
- An explicit distinction between proven failure and inconclusive results.
- Enough identifying context for another agent to recognize the approach and avoid repeating a proven failure.

### Information needed for a fresh agent instance

A large subtask could provide its new agent instance with:

- Its purpose, scope, acceptance criteria, and input/output contracts.
- Links to the source of truth and the relevant issue hierarchy.
- Its dependencies and the work already completed.
- Known successful and unsuccessful approaches relevant to the subtask, including any available context about their applicability to the current work.
- The current repository and branch state.

These are proposed requirements for task context, not a prescribed skill or file structure.

## Open questions

### How are releases identified and prepared

How should release version numbers be chosen? What documentation and deliverables should accompany a manually initiated release, and how should the chosen `develop` state be published to `main`?

### How are verification requirements selected

How should agents identify the affected rows, features, and modules? Within that scope, when are hardware execution, numerical accuracy, performance measurements, or environment reproduction required? Who agrees on the task's acceptance criteria and verification method when its agent instance starts?

### How is per engineer knowledge discovered across projects

Where should an engineer's successful and unsuccessful approaches be kept so that a new agent instance working on another of their projects can find them? How should project records link to that engineer's reusable knowledge while keeping each project's source of truth authoritative?

### How are direct planning commits grouped

How should direct planning updates on `develop` be grouped into coherent commits while keeping meaningful progress backed up remotely?

### How are backup failures and exceptions handled

How should agents handle network outages, a missing remote, or failed authentication? Does a confirmed opt-out waive remote hosting only, or also local Git use, and for how long does the exception apply?

### How are other affected agents informed and paused

Under human orchestration, should the user notify and pause other affected agents, or should agents also check the issue tracker at agreed checkpoints? Which tasks should pause when the impact of a bug is not yet known?

### How should deleted code be explained

Should the explanation also cover code that has been deleted, including why it was removed and how any affected behavior is handled?

### What counts as a repeated failed approach

How should agents recognize a previously failed approach and note a software change that permits a retry, without requiring exact version numbers or checksums?
