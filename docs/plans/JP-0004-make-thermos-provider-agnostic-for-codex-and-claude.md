# JP-0004 Make Thermos provider-agnostic for Codex and Claude

- Ticket: JP-0004
- Board: derived from `$PROJECT_WORKFLOW_OBSIDIAN_VAULT` and `docs/agents/project-workflow.json`
- Card: JP-0004 Make Thermos provider-agnostic for Codex and Claude
- Created: 2026-06-23

## Summary

Make the Thermos plugin package provider-agnostic by keeping one shared plugin payload and adding provider-specific Codex and Claude manifests, marketplace metadata, docs, and validation paths. Codex and Claude are the required initial destinations; the structure should leave room for Cursor or other destinations later without duplicating shared skills.

## Context

Source plan: `HANDOFF.md`.

The current Thermos package is a Codex plugin at `plugins/thermos` registered through `.agents/plugins/marketplace.json`. Most of the useful payload is already provider-neutral: shared skills, README, license, and assets. The provider-specific pieces should stay around that payload as manifests, marketplace catalogs, install docs, and validation commands.

Core design decision: keep one shared plugin payload under `plugins/<plugin-name>/`, then add provider-specific wrappers for Codex and Claude. Do not sync or commit provider-owned install caches such as `~/.codex/plugins/cache` or provider-managed Claude plugin internals.

Codex is already the baseline target through `plugins/thermos/.codex-plugin/plugin.json` and `.agents/plugins/marketplace.json`. Claude Code should become the second first-class target through validated Claude manifest and marketplace metadata, with the final file locations confirmed against Claude's current plugin tooling before completion.

The shared Thermos skill text needs cleanup before the package can be called provider-neutral. In particular, remove Codex-only wording such as "Codex port" and orchestration claims that belong in provider-specific docs instead of shared skill prompts.

Future Cursor or other provider support is out of scope for implementation in this ticket, but the repo convention created here should make adding another destination a provider-wrapper task instead of a shared-payload fork.

## Plan

- [x] Review HANDOFF.md, JP-0002, JP-0003, and the current plugins/thermos package before implementation.
- [x] Add Claude-compatible plugin packaging around the shared Thermos payload, including plugins/thermos/.claude-plugin/plugin.json and a root .claude-plugin/marketplace.json or the validated Claude marketplace location.
- [x] Update shared Thermos skill text and README content so provider-neutral skills do not describe themselves as Codex-only, while keeping provider-specific install and metadata details in provider-specific sections or files.
- [x] Document the repo convention that future plugins should keep shared payloads under plugins/<plugin-name>/ with Codex and Claude as first-class provider targets and room for future destinations such as Cursor.
- [x] Validate Codex packaging still works and validate Claude packaging/marketplace behavior using the provider CLIs where available.

## Acceptance Criteria

- [x] Thermos remains installable in Codex from .agents/plugins/marketplace.json.
- [x] Thermos has validated Claude-compatible plugin packaging and marketplace metadata without moving or duplicating the shared skills payload.
- [x] Shared Thermos skill files and README language are provider-neutral, with Codex-specific and Claude-specific details isolated to provider-specific manifests, catalogs, or documentation sections.
- [x] The repo documents the provider-agnostic plugin package convention for future plugins, starting with Codex and Claude and leaving a clear extension point for Cursor or other destinations.
- [x] No provider-owned install cache directories are committed or treated as source of truth.

## Verification

- [x] skill-python "$HOME/.codex/skills/.system/plugin-creator/scripts/validate_plugin.py" plugins/thermos
- [x] for skill_dir in plugins/thermos/skills/*; do skill-python "$HOME/.codex/skills/.system/skill-creator/scripts/quick_validate.py" "$skill_dir"; done
- [x] codex plugin marketplace add .
- [x] codex plugin add thermos@jonathanlindquist-plugins
- [x] codex plugin list
- [x] claude plugin validate .
- [x] claude --plugin-dir ./plugins/thermos
- [x] claude plugin marketplace add ./ --scope user

## Outcome

Implemented provider-agnostic Thermos packaging for Codex and Claude Code.

- Added root Claude marketplace metadata at `.claude-plugin/marketplace.json`.
- Added Claude plugin metadata at `plugins/thermos/.claude-plugin/plugin.json`.
- Reworded the shared `thermos` skill to run in the current agent instead of describing itself as a Codex-only port.
- Reworked `plugins/thermos/README.md` around a provider-neutral shared payload with separate Codex and Claude install sections.
- Updated `AGENTS.md` project conventions so future plugins use shared provider-neutral payloads with Codex and Claude wrappers first, and future destinations such as Cursor as additional wrappers.
- Confirmed no provider-owned cache directories were added to the repo.

Commit:

- Scoped JP-0004 commit with message `JP-0004 add Claude Thermos packaging`.

Verification:

- `skill-python "$HOME/.codex/skills/.system/plugin-creator/scripts/validate_plugin.py" plugins/thermos` passed.
- `for skill_dir in plugins/thermos/skills/*; do skill-python "$HOME/.codex/skills/.system/skill-creator/scripts/quick_validate.py" "$skill_dir"; done` passed for all three Thermos skills.
- `claude plugin validate plugins/thermos` and `claude plugin validate .` passed.
- `claude plugin validate --strict plugins/thermos` and `claude plugin validate --strict .` passed.
- `codex plugin marketplace add .` reported the local marketplace already added from this repo.
- `codex plugin add thermos@jonathanlindquist-plugins` installed Thermos version `1.0.0`.
- `codex plugin list` showed `thermos@jonathanlindquist-plugins` installed and enabled.
- `claude plugin marketplace add . --scope user` failed because Claude Code `2.1.186` rejects bare `.` as an invalid marketplace source. Updated docs and ticket verification to use `./`.
- `claude plugin marketplace add ./ --scope user` added `jonathanlindquist-plugins`.
- `claude plugin install thermos@jonathanlindquist-plugins --scope user` installed Thermos version `1.0.0`.
- `claude plugin list` showed `thermos@jonathanlindquist-plugins` enabled.
- `claude plugin details thermos@jonathanlindquist-plugins` showed three skills: `thermo-nuclear-code-quality-review`, `thermo-nuclear-review`, and `thermos`.
- `claude --plugin-dir ./plugins/thermos -p "/help" --output-format json --no-session-persistence --tools ""` exited successfully without an API call; `/help` itself is unavailable in Claude print mode.
- `git diff --check` passed.
- Provider cache scan found no `.codex/plugins/cache`, `.claude/plugins/cache`, or `.cursor-plugin` directories in the repo.

## Progress Notes

- 2026-06-23: Started provider-agnostic Thermos packaging implementation by adding Claude manifest and marketplace metadata.

## Completion Notes

- 2026-06-23: Completed provider-agnostic Thermos packaging for Codex and Claude Code, including Claude manifests, provider-neutral shared docs, provider convention updates, and validation/install checks.
