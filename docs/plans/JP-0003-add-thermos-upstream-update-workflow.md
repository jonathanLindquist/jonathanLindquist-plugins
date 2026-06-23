# JP-0003 Add Thermos upstream update workflow

- Ticket: JP-0003
- Board: derived from `$PROJECT_WORKFLOW_OBSIDIAN_VAULT` and `docs/agents/project-workflow.json`
- Card: JP-0003 Add Thermos upstream update workflow
- Created: 2026-06-23

## Summary

Define and implement the future workflow for refreshing the local Thermos Codex plugin from newer cursor/plugins upstream revisions after JP-0002 lands. This should preserve Codex-specific manifest and orchestrator decisions while making upstream provenance and local reinstall steps repeatable.

## Context

Relevant background, links, constraints, prior decisions, and source references.

## Plan

- [ ] Wait until JP-0002 has landed so plugins/thermos exists and records its initial upstream source commit.
- [ ] Design the update workflow around comparing the recorded upstream commit with a selected newer cursor/plugins commit.
- [ ] Refresh vendored Thermos skills, assets, license, and README material while preserving Codex-specific .codex-plugin/plugin.json and sequential orchestrator decisions unless intentionally changed.
- [ ] Document the update procedure for future Thermos refreshes, including source commit recording and local Codex cachebuster/reinstall steps.
- [ ] Run plugin validation, skill validation, cachebuster update, and install verification after a refresh.

## Acceptance Criteria

- [ ] The update workflow has a documented source-of-truth for the previous and new upstream cursor/plugins commits.
- [ ] Refreshing upstream Thermos content does not overwrite the Codex manifest or silently reintroduce Cursor-only agents/Task behavior.
- [ ] The update procedure explains when to use update_plugin_cachebuster.py and when to reinstall thermos from the repo-local marketplace.
- [ ] Validation commands for plugin manifest, copied skill front matter, and Codex install visibility are explicit.

## Verification

- [ ] git ls-remote https://github.com/cursor/plugins.git refs/heads/main
- [ ] skill-python "$HOME/.codex/skills/.system/plugin-creator/scripts/validate_plugin.py" plugins/thermos
- [ ] skill-python "$HOME/.codex/skills/.system/skill-creator/scripts/quick_validate.py" plugins/thermos/skills/<skill-name>
- [ ] skill-python "$HOME/.codex/skills/.system/plugin-creator/scripts/update_plugin_cachebuster.py" plugins/thermos
- [ ] codex plugin add thermos@jonathanlindquist-plugins

## Outcome

Fill in when completed with implementation summary, commits, verification commands, and results. Move the Kanban card to Completed only after this is filled in and the applicable card checkboxes are checked.
