# Local development station

This guide defines the intended layout for Christoph’s Linux workstation. Follow the [core principles](../core%20principles/README.md) for repository selection, Git backup, planning, branches, verification, and project records. Apply the project owner's workflow when working in an externally managed repository.

## Directory layout

| Path | Purpose |
| --- | --- |
| `/home/christoph/Documents/projects/` | The only root for local development. Keep Git checkouts, development worktrees, source, project-specific skills, and project scratch in the project locations Christoph chooses. |
| `/home/christoph/workspace/` | The Modalix workspace. Use it exclusively for DevKit interaction, preparing and building pipelines for the DevKit, and staging and running them. |
| `/home/christoph/.agents/` | The shared home for workstation instructions, agent setup notes, and general-purpose personal skills. |
| `/home/christoph/Documents/zzz_general_resources/` | The task index and reusable knowledge collection. Keep task-specific skills, procedures, recommended practices, pitfalls, and lessons here or link to their current source. |

Christoph creates and organizes project roots, selects worktree locations, and designates each project's Modalix workspace folder. Agents may create ordinary subdirectories inside the selected project or its designated development worktree as part of the task. The project build or run command may create disposable run subdirectories inside its designated Modalix workspace folder.

Before a development write, check the resolved checkout or worktree path. If it is outside `~/Documents/projects/`, or if the task has no selected checkout, ask Christoph which project to use. Reuse its existing repository and scratch convention, including `do_not_track/` where configured. Keep project scratch, worktrees, and development outputs out of projectless chat folders such as `~/Documents/Codex/`. Leave app-managed state and caches to the tools that own them. For any other persistent directory the task requires, ask Christoph to designate it.

## Agent startup

Use the tool's own installer and settings. The table lists the default user locations for local sessions. The shared workstation instructions belong in `~/.agents/workstation.md`. Keep credentials in approved secret storage.

| Tool | Startup instructions | Personal skills | User settings |
| --- | --- | --- | --- |
| Codex | `~/.codex/AGENTS.md` | `~/.agents/skills/` | `~/.codex/config.toml` |
| Claude Code | `~/.claude/CLAUDE.md` | `~/.claude/skills/` | `~/.claude/settings.json` |

Use the current vendor instructions for installation and updates. Use `command -v codex` or `command -v claude` to locate the executable used by the shell. [Codex CLI installation](https://learn.chatgpt.com/docs/codex/cli) · [Claude Code setup](https://code.claude.com/docs/en/setup)

### Shared workstation instructions

Use this text in `~/.agents/workstation.md`.

```markdown
# Local development station

Do local development only in a user-selected Git checkout or designated
worktree whose resolved path is under /home/christoph/Documents/projects/.
If the current checkout is elsewhere or no project is selected, ask Christoph
which checkout to use before creating files or running development commands.
Christoph chooses new project roots and worktree locations. Keep development
scratch and outputs inside the project, using its existing scratch convention.

Use /home/christoph/workspace/ exclusively for Modalix DevKit work and the
associated pipeline builds and runs. Use the project's designated workspace
folder. The build or run command may create disposable run directories there.
Make source, configuration, and script changes in the project checkout.

Before planning or running task-specific commands for each development or
DevKit task, open /home/christoph/Documents/zzz_general_resources/README.md.
Follow and read the current entry for the task, including the linked skill
or procedure where applicable. Identify the guidance used in your plan or
first work update. If there is no matching entry, say so. If a required
resource is missing, report the missing path and ask how to proceed.
For SiMa/Neat tasks, follow the current Neat guidance linked from the index
and check installed SDK or CLI behavior where version details matter.

Follow the project's startup instructions and Git workflow. Keep project
decisions and evidence links in the agreed project record. Preserve lessons
that apply across projects under the relevant task in the resource index.
Ask Christoph to designate any other persistent directory the task requires.
```

Add this instruction to the active Codex user instructions alongside existing guidance. Check for `~/.codex/AGENTS.override.md`, which takes precedence over `AGENTS.md` when it is non-empty. [Codex instruction discovery](https://learn.chatgpt.com/docs/agent-configuration/agents-md)

```text
Before planning or running commands for local development or Modalix DevKit
work, open /home/christoph/.agents/workstation.md and follow its instructions.
```

Add this import alongside existing guidance in `~/.claude/CLAUDE.md`. [Claude Code imports](https://code.claude.com/docs/en/memory#import-additional-files)

```text
@/home/christoph/.agents/workstation.md
```

Start a local Codex Desktop chat with the actual checkout as its primary folder. If using managed worktrees, set **Settings → Worktrees → Worktree root** to a location Christoph designates under `~/Documents/projects/`. Start Claude Code from the selected checkout or an approved worktree. [Codex project folders](https://learn.chatgpt.com/docs/projects) · [Codex worktree location](https://learn.chatgpt.com/docs/environments/git-worktrees#frequently-asked-questions)

### Skill and knowledge ownership

- Keep general-purpose personal skills in `~/.agents/skills/`. To make one available to Claude Code, link its individual skill directory from `~/.claude/skills/`. [Codex skill discovery](https://learn.chatgpt.com/docs/build-skills) · [Claude Code skill discovery](https://code.claude.com/docs/en/skills)
- Use `zzz_general_resources/README.md` as the task index. Give each topic a clear entry that links to its current skill or procedure and to relevant successes, failures, and pitfalls. Keep the working instructions in their canonical location. If a task-specific skill needs automatic discovery, link its skill directory from the tool's discovery path.
- Keep project-specific skills, decisions, and experiment records in the project or agreed initiative tracking location. For a reusable finding in the task index, record what was tried, what happened, and what the next agent should do. For a proven failure, include the conditions under which it failed so the next agent can tell whether the lesson still applies.

## From project checkout to Modalix

```text
Host checkout   ~/Documents/projects/<project>/       source, configs, scripts, project record
                         │ prepare a run using the project's build and run procedure
                         ▼
Host workspace  ~/workspace/<project>/<run>/          staged inputs, runnable files, outputs
                         │ Neat DevKit Sync and dk
                         ▼
Modalix DevKit                                        execute and verify
```

1. Start in the project checkout. Develop and review source, configuration, build and deployment scripts, and verification code there. Use the project's Git workflow and commit and push meaningful checkpoints as work progresses.
2. Use the project’s existing build and run procedure. If it can build from the checkout, build there and stage the files the board needs in the designated run directory. For C++, that may be the ARM64 executable, configuration, and model package. For Python, stage the entry point, modules, configuration, and other runtime inputs.
3. If the SDK only sees the shared workspace and needs source to cross-compile, stage a disposable snapshot of the required build inputs under the run directory and compile inside the SDK. For a source or config change, edit the checkout and regenerate the staged files. Keep the run directory replaceable and stage actual files so the board can resolve their paths.
4. Follow the resource index into the current Neat checkout and use `core/docs/getting-started/dev-environment/devkit-sync.md` for pairing and synchronization. With the standard workspace mapping, host `~/workspace/` appears as `/workspace` inside the SDK. With NFS, the DevKit sees the shared files. With rsync fallback, `dk` syncs the entry point's top-level workspace folder and maps paths to the DevKit's synchronized copy. Keep files needed by the run under the same top-level project folder. Check the active mapping with `dk status` inside the paired SDK before using it.
5. Verify the expected behavior on the DevKit. Record meaningful results in the agreed project or initiative record. Move large retained outputs such as traces or raw measurements to approved storage and link to them from that record. Preserve cross-project lessons under the relevant task in the resource index. Clean up disposable run files within the agreed scope once any needed evidence is preserved.

A pipeline change follows all five steps. A rerun can use the existing staged files when they match the intended inputs and current DevKit environment. When they differ, regenerate the run from the selected checkout or the project's published artifact.

## Check the setup after adoption

1. Start a fresh local session in each agent from an approved project. Give it a SiMa task without naming the resource index. Check that it opens the index, follows the relevant entry, and identifies the guidance it used before planning or running task commands.
2. In a projectless session, ask where a development change would go. Check that the agent asks for the approved project before making any writes. If using worktrees, verify that their resolved paths are under `~/Documents/projects/`.
3. In an approved project with a DevKit run procedure, stage a run and check `dk status`. Confirm that the staged files resolve on the board, the run produces the expected result, and the result is recorded in the agreed project location. Repeat with the same inputs to check the rerun path.
