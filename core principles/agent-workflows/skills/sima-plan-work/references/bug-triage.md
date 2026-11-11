# Bug triage

For the full workflow, establish whether the bug is present on current `develop`, regardless of where it was discovered. Use the repository's debugging tools. In externally managed projects, follow the owners' triage and contribution procedure.

- **Bug confined to unmerged work.** Fix it in the working branch as part of the current implementation. Use the existing task record and document meaningful attempts.
- **Bug already on `develop`.** Pause the discovering agent's current subtask, mark work the bug blocks as blocked, and search existing issues. Use a matching issue and add relevant context when useful. If none exists, ask the user to create it. In either case, report the affected work and ask the user to coordinate the affected agents and assign one fixing worker. The user handles duplicate discoveries and informs the other agents.
- **Finding invalidates the agreed plan.** Pause and report the finding and its impact. The user leads coordination and replanning. An upstream result that cannot satisfy a downstream task's needs belongs here.

The assigned worker uses `sima-integrate-change` for verification, Ponytail review, the code explanation, explicit approval, squash merge, and closeout. The user integrates the relevant updated `develop` into the paused feature branch. Resume the original subtask after that integration.
