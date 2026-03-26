# GitHub Copilot Instructions — Invicta-One

## What This Repository Is

**Invicta-One** is a structured AI literacy program for Visma Tech Portugal. This is **not a software project** — there is no build system, test runner, linter, or deployable application. All content is prompt engineering artifacts (Markdown, JSON).

## Repository Structure

Trials are organized under `phase-<N>/trial-<N>-<name>/`. Each trial contains:

```
artifacts/
  system-prompt.md       ← Primary deliverable — the reusable, versioned prompt
  iteration-log.md       ← Refinement history (what changed and WHY it changed)
  sample-input.md        ← Representative inputs used during development
  sample-output.md       ← (or expected-output.md) Benchmark output
  negative-constraint-list.md  ← Trial 2 only: living document of banned words
  export.json            ← Structured output for Jira/Linear (where applicable)
README.md                ← Trial objective, challenge, and success criteria
```

## How to Validate a Trial

There are no automated tests. Validation is manual:
1. Copy `artifacts/system-prompt.md` into an AI chat interface as the system prompt
2. Paste `artifacts/sample-input.md` (or `sample-transcript.md`) as the user message
3. Compare the output against `artifacts/sample-output.md` / `expected-output.md`
4. Check adversarial cases documented in `artifacts/iteration-log.md`

## Current State

- **Phase 1 — The Augmentation Era** (active, 3 trials complete)
- **Phase 2 — The Agentic Shift** (planned)
- **Phase 3 — The Autonomous Frontier** (planned)

## Prompt Engineering Patterns in Use

| Trial | Techniques |
|-------|-----------|
| `trial-1-kessel-run` | Few-shot examples, sarcasm detection rules, confidence scoring, CSV export block |
| `trial-2-architect-constraint` | Negative constraint list (banned buzzwords + replacement guidance), self-check protocol |
| `trial-3-holocron-synthesis` | Sequential CoT pipeline (6 ordered steps), Chain-of-Verification (adversarial self-audit), source evidence tracing, multi-audience tone rewrites, JSON export |

## Key Conventions

**system-prompt.md structure** — All system prompts use parameterized placeholders in `[BRACKETED]` form with a configuration table at the top listing defaults. This makes prompts reusable across contexts without editing the core logic.

**iteration-log.md purpose** — This file documents *failure modes* that triggered each prompt revision, not just a changelog. When editing a system prompt, always add an entry explaining what broke and why the fix addresses the root cause.

**negative-constraint-list.md sync rule** — In Trial 2, the banned word table in `negative-constraint-list.md` and the table inside `artifacts/system-prompt.md` (Part 1 — The Architect's Filter) must stay in sync. If you add a banned word, update both files and bump the version number in both.

**export.json schema** — Trial 3's JSON export uses Jira/Linear-compatible fields: `id` (AI-001 format), `due_date` (ISO 8601 or "TBD"), `status` always "To Do", `labels` always includes `["meeting-action", "auto-extracted"]`, `confidence` matches the verification audit from Step 5.

**Adding new trials** — Follow the same `artifacts/` layout. The trial `README.md` must include the objective, challenge, and success criteria. The `iteration-log.md` is required even for a first version (document v1.0 design decisions as if explaining the initial failure mode you were designing against).

## CI / Automation

A GitHub Actions workflow (`.github/workflows/claude.yml`) enables `@claude` mentions in issues and PRs to trigger Claude Code responses. It requires an `ANTHROPIC_API_KEY` secret.
