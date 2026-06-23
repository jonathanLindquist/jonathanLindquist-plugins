# JP-0002 Port Thermos as a Codex plugin

- Ticket: JP-0002
- Board: derived from `$PROJECT_WORKFLOW_OBSIDIAN_VAULT` and `docs/agents/project-workflow.json`
- Card: JP-0002 Port Thermos as a Codex plugin
- Created: 2026-06-23

## Summary

Implement the reviewed HANDOFF.md plan by creating a Codex-compatible Thermos plugin inside this repo-local marketplace. Keep the repo extensible for future plugins by using plugins/<plugin-name>/ as the plugin boundary and .agents/plugins/marketplace.json as the catalog.

## Context

Source plan: `HANDOFF.md`.

Triage decisions:

- First implementation scope is the MVP sequential Codex workflow: port the two rubric skills and rewrite `thermos/SKILL.md` so the main Codex agent runs both review passes sequentially and synthesizes the result.
- Native Codex multi-agent orchestration is out of scope for this ticket. Track it separately if still desired after the MVP port works.
- Pin the initial import to `cursor/plugins@e46364b8be46000b7df0f260550cd712afbb8d36`, which was `refs/heads/main` when triaged on 2026-06-23.
- Treat this as an update-capable vendored port, not a one-time dead fork. Record upstream provenance in the plugin README or metadata notes so future update tickets can diff old upstream against new upstream.
- A separate follow-up ticket should cover the repeatable upstream update workflow.
- The implementing agent is allowed to run local Codex install verification commands for this repo-local marketplace, including `codex plugin marketplace add .`, `codex plugin add thermos@jonathanlindquist-plugins`, and `codex plugin list`.
- The plugin README should be Codex-specific with upstream attribution, not a mostly verbatim copy of Cursor's README.

## Plan

- [x] Review HANDOFF.md and this plan before implementation.
- [x] Scaffold the thermos plugin with plugin-creator under plugins/thermos and register it in .agents/plugins/marketplace.json.
- [x] Import upstream Thermos skills, assets, license, and relevant README content from cursor/plugins@e46364b8be46000b7df0f260550cd712afbb8d36 without copying .cursor-plugin/plugin.json over the Codex manifest.
- [x] Translate upstream metadata into a valid .codex-plugin/plugin.json using Codex manifest shape.
- [x] Handle the Cursor-specific thermos orchestrator skill by implementing the agreed Codex MVP: sequential main-agent review plus synthesis.
- [x] Write a Codex-specific README that attributes Cursor's upstream Thermos plugin, records the pinned upstream commit, explains the sequential Codex workflow, references JP-0003 for future upstream refreshes, and preserves MIT license attribution.
- [x] Validate the plugin manifest, skill front matter, marketplace registration, plugin install, and fresh-thread skill visibility, using the approved local Codex install commands.

## Acceptance Criteria

- [x] The repo-level marketplace catalog points to ./plugins/thermos and remains suitable for additional plugins.
- [x] plugins/thermos contains a valid .codex-plugin/plugin.json, copied logo asset, expected Thermos skills, license, and README material.
- [x] Cursor's .cursor-plugin/plugin.json is not used as the Codex manifest.
- [x] The Thermos orchestrator uses the MVP sequential Codex workflow, and unsupported Cursor Task behavior is explicitly deferred.
- [x] Upstream provenance records cursor/plugins@e46364b8be46000b7df0f260550cd712afbb8d36 and explains that future upstream refreshes should happen through a separate update ticket.
- [x] README is written for Codex users, links to upstream Thermos, records the pinned source commit, explains the Codex-specific orchestration difference, references JP-0003, and preserves upstream MIT attribution.
- [x] Validation and install checks pass, including plugin validation and Codex marketplace/plugin listing after running the approved local install commands.

## Verification

- [x] skill-python "$HOME/.codex/skills/.system/plugin-creator/scripts/validate_plugin.py" plugins/thermos
- [x] for skill_dir in plugins/thermos/skills/*; do skill-python "$HOME/.codex/skills/.system/skill-creator/scripts/quick_validate.py" "$skill_dir"; done
- [x] codex plugin marketplace add .
- [x] codex plugin add thermos@jonathanlindquist-plugins
- [x] codex plugin list shows thermos@jonathanlindquist-plugins and a fresh Codex thread can see the Thermos skills.

## Outcome

Implemented a repo-local Codex Thermos plugin at `plugins/thermos` and registered it in `.agents/plugins/marketplace.json` as `thermos@jonathanlindquist-plugins`.

- Added `.codex-plugin/plugin.json` with Codex-compatible metadata translated from upstream Cursor Thermos.
- Vendored the pinned upstream Thermos rubric skills, logo, and MIT license from `cursor/plugins@e46364b8be46000b7df0f260550cd712afbb8d36`.
- Removed Cursor-only `disable-model-invocation` front matter so the skills validate in Codex.
- Rewrote `skills/thermos/SKILL.md` for the MVP Codex workflow: run both rubric passes sequentially in the main Codex agent and synthesize findings.
- Added a Codex-specific README with upstream attribution, pinned commit provenance, JP-0003 refresh guidance, install commands, and MIT attribution.

Commit:

- Scoped JP-0002 commit with message `JP-0002 port Thermos as Codex plugin`. The repository currently has no existing commits and still contains unrelated untracked setup files from JP-0001, so only JP-0002 files should be staged.

Verification:

- `skill-python "$HOME/.codex/skills/.system/plugin-creator/scripts/validate_plugin.py" plugins/thermos` passed.
- `for skill_dir in plugins/thermos/skills/*; do skill-python "$HOME/.codex/skills/.system/skill-creator/scripts/quick_validate.py" "$skill_dir"; done` passed for all three skills.
- `find plugins/thermos -maxdepth 3 -name '.cursor-plugin' -o -name 'plugin.json' -print` showed only `plugins/thermos/.codex-plugin/plugin.json`.
- `codex plugin marketplace add .` added `jonathanlindquist-plugins`.
- `codex plugin add thermos@jonathanlindquist-plugins` installed Thermos version `1.0.0`.
- `codex plugin list` showed `thermos@jonathanlindquist-plugins` as installed and enabled.
- `codex exec --ephemeral --sandbox read-only ...` fresh-session check saw `thermos:thermos`, `thermos:thermo-nuclear-review`, and `thermos:thermo-nuclear-code-quality-review`.

## Progress Notes

- 2026-06-23: Started Thermos Codex plugin implementation; scaffolded initial plugin directory.

- 2026-06-23: Triage complete: implementation scope is MVP sequential Codex workflow, upstream source is pinned, install verification is approved, and README requirements are defined.

## Completion Notes

- 2026-06-23: Completed Thermos Codex plugin port: marketplace registration, vendored pinned upstream skills/assets/license, Codex sequential orchestrator, README provenance, validation, install, and fresh-session visibility checks all passed.
