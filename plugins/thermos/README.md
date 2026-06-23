# Thermos for Codex

Thermos is a Codex plugin port of Cursor's Thermos plugin. It provides thermo-nuclear branch review skills for correctness, security, developer-experience regressions, feature-gate leaks, and strict maintainability review.

## What This Plugin Includes

- `thermo-nuclear-review`: deep diff-scoped audit for bugs, breakages, security issues, developer-experience regressions, and feature-gate leaks.
- `thermo-nuclear-code-quality-review`: strict maintainability audit for abstraction quality, file-size growth, spaghetti conditions, boundary drift, and codebase health.
- `thermos`: Codex orchestrator skill that runs both review rubrics sequentially in the main Codex agent and synthesizes the results.

## Codex Orchestration

The upstream Cursor plugin launches two Cursor `Task` subagents in parallel through an `agents/` manifest. This Codex port does not install or rely on Cursor's `.cursor-plugin/plugin.json`, `agents/` directory, or `run_in_background` behavior.

For the MVP Codex workflow, invoke `thermos` to run both review passes sequentially in the main Codex agent:

1. Gather the review scope, diff, and relevant file context.
2. Run `thermo-nuclear-review`.
3. Run `thermo-nuclear-code-quality-review`.
4. Synthesize prioritized, deduplicated findings.

Native Codex multi-agent orchestration is intentionally deferred to JP-0003.

## Upstream Provenance

- Upstream repository: https://github.com/cursor/plugins
- Upstream Thermos URL: https://github.com/cursor/plugins/tree/main/thermos
- Upstream plugin path: `thermos`
- Pinned source commit: `cursor/plugins@e46364b8be46000b7df0f260550cd712afbb8d36`
- Pinned date in ticket triage: 2026-06-23

Future upstream refreshes should be handled through JP-0003 so the Codex manifest and sequential orchestrator decisions are preserved intentionally.

## Local Marketplace

This repo is a local Codex plugin marketplace. The repo-level catalog lives at `.agents/plugins/marketplace.json` and points to `./plugins/thermos`.

Install from this repo-local marketplace with:

```bash
codex plugin marketplace add .
codex plugin add thermos@jonathanlindquist-plugins
```

Start a fresh Codex thread after installation so the Thermos skills are visible.

## License And Attribution

Thermos is ported from Cursor's MIT-licensed `cursor/plugins` repository. The upstream MIT license is preserved in `LICENSE`.
