# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Repository Is

This is **Invicta-One**, a structured AI literacy program for Visma Tech Portugal. It is not a traditional software project — there is no build system, test runner, or deployable application. The repository contains prompt engineering artifacts organized by phase and trial.

## Repository Structure

Each trial follows a consistent layout under `phase-<N>/trial-<N>-<name>/`:
- `README.md` — Trial overview, objective, and success criteria
- `artifacts/system-prompt.md` — The reusable, versioned system prompt (primary deliverable)
- `artifacts/iteration-log.md` — Refinement history showing what changed between prompt versions and why
- `artifacts/sample-input.md` — Representative inputs used during development
- `artifacts/sample-output.md` (or `expected-output.md`) — Benchmark output demonstrating success
- `artifacts/export.json` — Structured output for downstream tools (e.g., Jira/Linear), where applicable

## How to Evaluate a Trial

There is no `npm test` or `pytest`. Validation is done by:
1. Running the `system-prompt.md` against `sample-input.md` in an AI chat interface
2. Comparing the output to `sample-output.md` / `expected-output.md`
3. Checking adversarial cases described in `iteration-log.md`

Each trial documents its own success criteria in its `README.md`.

## Current Program State

- **Phase 1 — The Augmentation Era** is active with three completed trials:
  - `trial-1-kessel-run`: Few-shot prompting and sarcasm detection in feedback classification
  - `trial-2-architect-constraint`: Negative constraint lists and anti-cliché enforcement
  - `trial-3-holocron-synthesis`: Chain-of-Thought extraction pipeline with zero-hallucination verification
- **Phase 2** and **Phase 3** are planned but not yet started

## Key Prompt Patterns in Use

| Trial | Core Techniques |
|-------|----------------|
| 1 | Few-shot examples, explicit sarcasm rules, confidence scoring, CSV export |
| 2 | Negative constraint lists with replacement guidance, self-check protocol, expandable ban list |
| 3 | Sequential CoT pipeline, Chain-of-Verification (self-audit), source evidence tracing, multi-audience rewriting, JSON export |

## When Adding New Trials

New trials should follow the same `artifacts/` layout. The `iteration-log.md` is important — it explains *why* the prompt evolved, not just *what* changed. Always document the failure mode that prompted each iteration.
