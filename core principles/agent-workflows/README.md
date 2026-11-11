# SiMa agent workflows

These templates apply the [Core principles](../README.md) to projects using Codex or Claude Code. The skills use one shared source and the standing instructions are in `AGENTS.md`.

## Adopt in a project

1. Place `AGENTS.md` at the project root, or incorporate its guidance into the project's existing instructions with the owner's approval. Provide the [repository overview and project skills](AGENTS.md#repository-overview-and-project-skills) required by the standing instructions.
2. For Claude Code, use `CLAUDE.md` at the same root. If the project already has a `CLAUDE.md`, import the root `AGENTS.md` with a path relative to that file.
3. Copy each directory under `skills/` with its supporting files to `.agents/skills/` for Codex or `.claude/skills/` for Claude Code. Keep project-specific skills and their supporting files under version control in the same discovery paths. When updating an installation, use the same shared source directories.
4. Start a new session at the project root and check that the standing instructions, repository overview, and relevant skills are available. Claude Code's `/memory` shows loaded instructions and `/skills` shows its skill list. In Codex, check the skills picker or invoke the skill by name.

See the official documentation for project discovery and skill placement. [Codex customization](https://learn.chatgpt.com/docs/customization/overview) · [Claude Code imports](https://code.claude.com/docs/en/memory#share-one-file-with-other-coding-tools) · [Claude Code skills](https://code.claude.com/docs/en/skills)

## Use the tools already available

Use the host's planning and worktree features, Git and the existing hosting tools, and the project's test and verification tools. When Superpowers is available, its planning, worktree, debugging, and verification skills can handle those mechanics under the project's approval rules. For the full workflow, check that the active tool can discover Ponytail's `ponytail-review` skill. Use it for the required merge review. [Ponytail installation guide](https://github.com/DietrichGebert/ponytail/blob/main/INSTALL.md)

Use agreed project conventions for choices such as verification methods, release preparation and numbering, and the engineer's knowledge storage. If a choice is needed to proceed and remains open, ask the engineer.
