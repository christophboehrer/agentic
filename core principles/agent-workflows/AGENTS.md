# SiMa development workflow

Use this guidance for work the engineer brings under this workflow. Reuse the existing project repository for implementation and experiments. For externally managed projects, follow the owners' contribution procedures and use the agreed initiative tracking location.

## Repository overview and project skills

Give agents a short overview through the root startup instructions. Include it there or link directly to the root project entry point. Summarize the project purpose and core components with their paths. Link to the current project document, issue tracker, and main build and verification guidance. Choose a layout that fits the project and its tooling. Keep the overview current as the layout changes.

Make useful skills available to agents working on the project. Reuse shared skills where they fit. When a repeatable task benefits from project-specific guidance, package it as a skill and version it with its supporting files in the project's skill discovery paths. Point agents to the relevant skills from the overview.

For externally managed projects, use the owners' layout and procedures. Fill navigation gaps in the agreed initiative record. Keep useful initiative-specific skills in an authorized initiative Git repository when upstream additions lack approval.

## Standing rules

- Establish authorized remote Git backup before development. Commit and push meaningful checkpoints during implementation and exploration. Working without remote backup requires a request initiated by the user and a separate confirmation from them.
- If a required step is unavailable, report which step is blocked and why, then wait for the user's instructions.
- In the full workflow, implement on working branches. Initial repository setup is the exception. Planning and roadmap edits may go directly to `develop`. Consult the authoritative project document on current `develop` from any working branch.
- Work within the agreed plan. Keep the implementation handoff current in the existing task record.
- In the full workflow, before merging into `develop`, follow the integration skill and obtain explicit user approval. If the reviewed branch or its target changes after review, report the change and pause the merge. The user handles the changed branch state and directs the next step.
- In the full workflow, publish from `develop` to `main` only for a release explicitly requested or manually triggered by the user.

## Skills

| When | Skill |
| --- | --- |
| Selecting a workflow, planning work, starting an exploration, or deciding how to handle a bug or invalidated plan | `sima-plan-work` |
| Making a checkpoint, recording attempts, preparing a handoff, or resuming implementation | `sima-record-work` |
| Integrating subtasks, preparing or performing a merge, closing abandoned work, or handling a user-initiated release | `sima-integrate-change` |

Use available native tools and established skills for planning, Git and worktrees, debugging, verification, and hosting operations. Apply the repository's workflow and the approval boundaries above when using them.
