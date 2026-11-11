---
name: sima-plan-work
description: Use when a SiMa workflow task needs repository selection or setup, an agent-facing repository overview, project or subtask planning, exploration or optimization, or a decision about a bug or invalidated plan.
---

# Plan development work

Use the existing planning and worktree capabilities of the active tool or an available skill. Apply these project decisions before implementation.

## Select the repository and tracking location

Reuse the project repository, including for experiments. Create a project repository only when none exists for the work.

- **Full workflow.** Use this for projects started under this approach where the user controls the repository and its layout. Set up remote backing and the project branches. Keep `main` for published releases and `develop` in a working state. Define the full scope and authoritative project document on `develop`. See [Project record](references/project-record.md).
- **Externally managed project.** Follow the owners' branching, documentation, review, and release procedures. Adapt planning, backup, tracking, and verification to their workflow and permissions. Use the existing tracking location when it meets the initiative's needs. If a layout change would help, describe the specific change and confirm the user's authority and agreement. If separate tracking is needed and the layout stays as it is, use a separate Git repository authorized for initiative tracking. Record plans, decisions, progress, and verification there and link the upstream issues, pull requests, and documentation. The initiative record governs its plan and decisions. The project and its maintainers govern project behavior and contributions.

When setting up or reorganizing a project, provide the startup overview and make useful project skills discoverable as required by the standing instructions.

## Prepare and assign implementation

Define core features, purpose, input and output contracts, roadmap, and implementation plan. Break planned work into large subtasks and finer tasks in the hosting provider's issue hierarchy or equivalent. Agree on the plan before implementation. For each feature addition or extension in the full workflow, use a new descriptive feature branch.

Assign each large subtask to a fresh agent instance with concrete goals, acceptance criteria, and a verification method. Agents work within the agreed plan. Run large subtasks in parallel only when independent. In the full workflow, ordinary changes to `develop` should leave active branches able to continue independently.

Read [Exploration and optimization](references/exploration.md) for exploratory goals or optimization. Read [Bug triage](references/bug-triage.md) when a defect is found or a finding invalidates the plan.
