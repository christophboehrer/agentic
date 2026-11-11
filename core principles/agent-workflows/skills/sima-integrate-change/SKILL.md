---
name: sima-integrate-change
description: Use when a SiMa workflow feature or bugfix is being integrated or presented for merge, an abandoned branch is being closed out, or the user initiates a release.
---

# Integrate development changes

Apply these branch rules in the full workflow. In externally managed projects, use the owners' integration, review, and release procedures and preserve the agreed initiative records.

## Integrate into the feature branch

Agents may merge subtask branches into the feature branch within the approved plan. Integrate all agreed subtasks there before presenting the complete feature for one review and one squash merge into `develop`.

## Prepare and merge into `develop`

1. Include cleanup made necessary by the change. Make optional readability edits only when the user requests them. Prepare the project document updates for current behavior and all branch decisions, including implementation and intended interactions. Include these updates with the code when appropriate.
2. Record the branch head and target revisions for this review. Verify the complete change against the agreed acceptance criteria. Check the affected rows, features, and modules using the project's verification tools. Record the scope and results.
3. Use the Ponytail plugin's `ponytail-review` skill. Specify the complete feature or bugfix diff against the recorded `develop` revision for an extensive review. Fix findings within the agreed plan. If those fixes change the branch, update the recorded head and repeat the affected-area checks and review before presenting it. Raise findings that would change the plan with the user. Route bugs already on `develop` through the bug-triage procedure. Apply the unavailable-step rule if a required check or Ponytail review is unavailable.
4. Compare the current branch head and target with the reviewed revisions. If either has changed after review, report the change and pause. The user handles the changed branch state and directs the next step.
5. Present the reviewed change against `develop` with both revisions, the verification results, and the review findings. Explain every changed or added line of code in simple terms, covering what it does, its purpose, and why it belongs. Group related lines where the explanation covers each one. Ask the user explicitly to approve the merge. If they request revisions, make them and present the revised change with the required verification, review, and explanation for approval.
6. Check that the project's merge path leaves release initiation to the user. If merging would trigger publication to `main` or assign a release version or tag, report the blocker and wait. Confirm the meaningful attempt records will remain accessible after branch deletion. Recheck both revisions immediately before merging and apply the same pause rule if either changed. With explicit approval for the verified, reviewed, and explained change, use the existing Git or hosting workflow to squash merge the complete branch into `develop` as one commit.
7. Confirm the successful merge and that `develop` contains the branch decisions. Delete the merged working branch if it still exists.

## Close abandoned work

When the user abandons work, preserve project findings, decisions, and evidence links in the authoritative document on `develop` or the agreed initiative tracker. Preserve reusable lessons in the engineer's personal knowledge collection. Then delete the abandoned branch under the project's workflow.

## Release to `main`

For an explicitly requested or manually triggered release, use the agreed procedure to publish the chosen `develop` state to `main`. Each published release has a release version number and a Git tag identifying the released commit on `main`. Use commit identifiers for ordinary development. If details such as the version or release procedure are still open, ask the user. Merge approval into `develop` covers the merge only.
