---
name: sima-record-work
description: Use when a SiMa workflow task reaches a meaningful checkpoint, produces an experiment result, needs a handoff, or is being resumed from another agent's work.
---

# Record development work

## Back up checkpoints

Commit and push meaningful checkpoints on the working branch during implementation and exploration using the project's existing Git tools. Follow the standing instructions for an unavailable remote or required push.

Track source, tests, build and verification scripts, dependency manifests, shared configuration, and small fixtures or expected outputs. Track exploratory scripts and tests needed to resume work or reproduce a meaningful result. Retain evidence that supports a decision, serves as a baseline, or is costly to reproduce. Put large retained datasets, binaries, and raw evidence in approved storage and link them from the task or project record.

Keep disposable outputs, caches, virtual environments, temporary logs, and machine-specific configuration ignored. Keep credentials in approved secret storage and track sample configuration when useful. Use `.gitignore` for shared exclusions and Git's local exclude file for personal ones. Follow the owners' rules for shared exclusions.

## Record meaningful attempts

For each meaningful attempt, record the approach, result, and conclusion in the agreed tracking location. Preserve failed approaches with enough context to recognize the same method and software basis and avoid repeating it. A changed software basis can justify another attempt. Keep the original result. Routine implementation mistakes are corrected in the working branch under the bug-triage rules. Keep reusable successes and failures in the engineer's agreed personal knowledge collection.

If the project uses `do_not_track/`, keep temporary probes there under the relevant Git exclusion. Before discarding them, put findings and evidence needed for future work in tracked or approved storage. Move scripts needed to resume or reproduce important results into a tracked location while work is in progress.

## Maintain the handoff during implementation

Update the existing task record at meaningful checkpoints. Keep these fields current:

- Branch and last pushed commit.
- Local edits and unpushed commits, with their checkout location when needed.
- Completed checks, affected areas, and results.
- Current blockers.
- Next action.
- Links to the approved scope and evidence needed to continue.

Before declaring the task complete, verify the agreed acceptance criteria using checks for the affected areas and the task's goals. Record what was checked and the outcome. When resuming, read the handoff and compare it with the current repository and task state before choosing the next action. Preserve edits already in progress and route changes to the agreed plan through the user.
