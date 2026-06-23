# Thermos

Thermos provides thermo-nuclear branch review skills for correctness, security, developer-experience regressions, feature-gate leaks, and strict maintainability review.

This package keeps the review payload provider-neutral. The shared skills, assets, README, and license live in this plugin directory; provider-specific manifests and marketplace catalogs wrap that payload for each destination.

## What This Plugin Includes

- `thermo-nuclear-review`: deep diff-scoped audit for bugs, breakages, security issues, developer-experience regressions, and feature-gate leaks.
- `thermo-nuclear-code-quality-review`: strict maintainability audit for abstraction quality, file-size growth, spaghetti conditions, boundary drift, and codebase health.
- `thermos`: orchestrator skill that runs both review rubrics sequentially in the current agent and synthesizes the results.

## Provider Model

The upstream Cursor plugin launches two Cursor `Task` subagents in parallel through an `agents/` manifest. This provider-neutral package does not install or rely on Cursor's `.cursor-plugin/plugin.json`, `agents/` directory, or `run_in_background` behavior.

Instead, invoke `thermos` to run both review passes sequentially in the current agent:

1. Gather the review scope, diff, and relevant file context.
2. Run `thermo-nuclear-review`.
3. Run `thermo-nuclear-code-quality-review`.
4. Synthesize prioritized, deduplicated findings.

Provider-specific files should only describe installation, display metadata, marketplace metadata, and provider-specific capabilities. Shared review behavior belongs in `skills/`.

## Provider Packaging

- Codex manifest: `.codex-plugin/plugin.json`
- Claude Code manifest: `.claude-plugin/plugin.json`
- Shared skills: `skills/*/SKILL.md`
- Shared assets and attribution: `assets/`, `LICENSE`, and this `README.md`

## Codex Install

This repo's Codex marketplace catalog lives at `.agents/plugins/marketplace.json` and points to `./plugins/thermos`.

Install from this repo-local marketplace with:

```bash
codex plugin marketplace add .
codex plugin add thermos@jonathanlindquist-plugins
```

Start a fresh Codex thread after installation so the Thermos skills are visible.

## Claude Code Install

This repo's Claude Code marketplace catalog lives at `.claude-plugin/marketplace.json` and points to `./plugins/thermos`.

Test the plugin directly during development with:

```bash
claude --plugin-dir ./plugins/thermos
```

Register the local marketplace from this repo with:

```bash
claude plugin marketplace add ./ --scope user
```

Then install `thermos@jonathanlindquist-plugins` from Claude Code's `/plugin` interface or equivalent plugin install flow.

## Upstream Provenance

- Upstream repository: https://github.com/cursor/plugins
- Upstream Thermos URL: https://github.com/cursor/plugins/tree/main/thermos
- Upstream plugin path: `thermos`
- Pinned source commit: `cursor/plugins@e46364b8be46000b7df0f260550cd712afbb8d36`
- Pinned date in ticket triage: 2026-06-23

Future upstream refreshes should be handled through JP-0003 so provider-specific manifests and the shared sequential orchestrator decision are preserved intentionally.

## License And Attribution

Thermos is ported from Cursor's MIT-licensed `cursor/plugins` repository. The upstream MIT license is preserved in `LICENSE`.

Additional provenance, source-commit, and local-modification details are recorded in `NOTICE.md`.
