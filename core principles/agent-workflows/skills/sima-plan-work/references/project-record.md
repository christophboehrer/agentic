# Project record

For the full workflow, keep one authoritative document on `develop`. Include a table of contents and stable sections.

| Section | Contents |
| --- | --- |
| Purpose and scope | A complete description of the project's purpose, goals, boundaries, and exclusions. |
| Features and interactions | Required behavior, with implemented and planned features clearly identified. |
| Architecture and contracts | Component responsibilities, inputs, outputs, and interactions. |
| Working environment | Relevant hardware, software, dependencies, and known limitations. Exact versions are optional. |
| Roadmap | Implementation direction and plan, with links to the issue hierarchy. |
| Implementation decisions | What was decided, why, and the components or interactions affected. |
| Verification | How behavior is verified and links to results. |
| Changelog | A concise entry for each merged change, linked to its issue or pull request. |

Link to the current document on `develop` from a short project entry point. Feature and bugfix branches can propose changes alongside their code. Treat those edits as proposals until merged into `develop` and use historical branch copies as snapshots.

Update the current description of each affected feature or contract when behavior changes. Give decisions stable identifiers. Mark superseded decisions and keep their rationale in a historical appendix in the same document.

Keep open tasks and detailed progress in GitHub Issues or the hosting provider's equivalent. Keep the roadmap and its issue links consistent with that tracker.
