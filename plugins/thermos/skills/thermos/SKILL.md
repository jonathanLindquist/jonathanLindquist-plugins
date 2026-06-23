---
name: thermos
description: "Run both Thermos review passes sequentially in the main Codex agent, then synthesize their findings. Use for thermos, double thermo review, or combined bug/security and code-quality branch audits."
---

# Thermos

Run the two Thermos review passes in the main Codex agent, then synthesize their results.

This Codex port intentionally does not use Cursor `Task` subagents or the upstream `agents/` manifest field. Native Codex multi-agent orchestration is deferred to JP-0003.

## Workflow

1. Determine the review scope from the user request, PR, current branch, or relevant changed files.
2. Gather the diff and any file/context excerpts needed for reviewers to evaluate the change without guessing.
3. Run the `thermo-nuclear-review` rubric first against the scoped diff and context. Capture prioritized findings with file references and evidence.
4. Run the `thermo-nuclear-code-quality-review` rubric second against the same scoped diff and context. Capture prioritized findings with file references and evidence.
5. Synthesize the two passes with findings first, deduplicated across rubrics. Weight overlapping findings more heavily, resolve disagreements with your own judgment, and keep summaries brief.

Do not restate either pass wholesale. Surface the unified verdict, the highest-signal findings, and any remaining uncertainty.
