# Agent Instructions

This file is the source of truth for repo-local agent guidance. Keep shared instructions here; `CLAUDE.md` is only a thin wrapper that points Claude Code back to this file.

## Agent skills

### Issue tracker

Issues and implementation tickets live in the Obsidian Kanban board derived from `$PROJECT_WORKFLOW_OBSIDIAN_VAULT` and this repository's path relative to `$HOME`. External PRs are not a triage surface. See `docs/agents/issue-tracker.md`.

### Ticket workflow

Create tickets with `new_project_ticket.mjs`; it allocates stable IDs, appends a Kanban card, creates a linked plan in `docs/plans/`, and advances `docs/agents/ticket-sequence.json`. Use repeatable `--todo`, `--acceptance`, and `--verification` fields when a ticket is ready for agent implementation. Update ticket status with `update_project_ticket.mjs` after code changes and during closeout. See `docs/agents/ticket-workflow.md`.

When working from a ticket, read the Kanban card and linked plan before implementation. After making implementation changes, move the card to `In Progress` unless it is already there. Before calling the ticket complete, verify the acceptance criteria and verification items; add completion notes to the linked plan; move the Kanban card to `Completed`; check applicable TODO, Acceptance Criteria, and Verification boxes; and re-read the board to confirm the lane.

### Project setup verification

After setup or any rerun of setup, the setup command must finish by running:

```bash
node "$HOME/.agents/skills/setup-project-workflow/scripts/verify_project_workflow.mjs" --project-root "$PWD"
```

Run the same command manually after resolving any protected-file merge.

### Execution plans

Execution plan Markdown files live under stable paths in `docs/plans/`, for example `docs/plans/JP-0001-initialize-project-workflow.md`. Do not use lane-named status folders for new plans; old `docs/plans/Backlog/`, `docs/plans/In Progress/`, and `docs/plans/Completed/` folders are legacy.

### Triage labels

Use the default five-role triage vocabulary as Obsidian tags configured with Kanban plugin colors: `#needs-triage`, `#needs-info`, `#ready-for-agent`, `#ready-for-human`, and `#wontfix`. Add, remove, or replace those tags in the card's `Description` section. See `docs/agents/triage-labels.md`.

### Domain docs

This is a single-context repo: read root `CONTEXT.md` and `docs/adr/` if they exist. See `docs/agents/domain.md`.

## Project conventions

This repository is a local provider-agnostic plugin marketplace, not a one-plugin scratch directory.

- Keep the repo-level Codex marketplace catalog at `.agents/plugins/marketplace.json`.
- Keep the repo-level Claude Code marketplace catalog at `.claude-plugin/marketplace.json`.
- Keep plugin implementations under `plugins/<plugin-name>/`.
- Treat each `plugins/<plugin-name>/` directory as the ownership boundary for that plugin's manifests, skills, assets, license, README, and plugin-specific source.
- Keep shared plugin payloads provider-neutral inside `plugins/<plugin-name>/`; put provider-specific metadata in provider-specific wrappers such as `.codex-plugin/` and `.claude-plugin/`.
- Codex and Claude Code are first-class destinations for new plugins. Structure future provider support, such as Cursor, as an additional provider wrapper around the shared payload rather than a fork of shared skills or assets.
- Parameterize future scaffold/import scripts by plugin name and source path or URL; do not hard-code the Thermos plugin path into shared automation.
- Do not implement `HANDOFF.md` directly. Turn the reviewed plan into a ticket first, then work from the Kanban card and linked plan.

### Verify, don't Trust

When producing an analysis or summarization of something gleaned from a resource (web page, MCP call, user-provided document), do not trust a memory or retained summary of that resource. Always retrieve the resource afresh and compare it to the summary or analysis you are preparing. When comparing, do so in an adversarial way: you are fact-checking work that you suspect at the start contains errors and hallucinations.
