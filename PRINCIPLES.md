# Prompt Engineering Principles

> Distilled from the Phase 1 Prompting Trials. These principles apply to any LLM interaction,
> not just the specific tasks in this program.

---

## 1. Always Require Reasoning, Not Just Conclusions

If you only ask for a classification, you get a guess. If you require the AI to explain *why*,
you get a decision you can audit. The Reasoning column (Trial 1), the replacement guidance
(Trial 2), and the Source Quote field (Trial 3) all serve the same purpose: making the AI's
logic visible and verifiable.

**Pattern:** Add a "Reasoning" or "Evidence" field to every structured output.

---

## 2. Ban Behaviors With Alternatives, Not Just Prohibitions

Telling an AI "don't use this word" is unreliable — the model may forget mid-generation.
Telling it "don't use X, use Y instead" gives it a concrete path to follow. Replacement
guidance outperforms bare bans consistently.

**Pattern:** For every constraint, provide the alternative the AI should reach for instead.

---

## 3. Self-Verification Must Be Explicit and Structured

A vague "double-check your work" instruction does nothing. A specific protocol — "scan
every word against this list, rewrite violations, then confirm with a checkmark" — produces
consistent enforcement. The self-check must be a defined process, not a suggestion.

**Pattern:** End every prompt with a mandatory verification step that has concrete actions and a confirmation output.

---

## 4. Chain-of-Verification Should Be Adversarial, Not Confirmatory

When the AI verifies its own work, it tends to confirm what it already generated. Effective
verification requires adversarial framing: "Try to disprove this. Try to find a reason it's
wrong. Only keep it if you can't." This transforms a rubber-stamp into a genuine audit.

**Pattern:** Frame verification as "attempt to DISPROVE" rather than "confirm."

---

## 5. Few-Shot Examples Anchor Behavior More Reliably Than Instructions

Instructions describe what you want. Examples *show* it. When instructions and examples
conflict, the model follows the examples. Use this to your advantage: craft 2-3 examples
that demonstrate the exact pattern, including edge cases, before writing any rules.

**Pattern:** Include at least 2-3 few-shot examples covering the common case, the edge case, and the ambiguous case.

---

## 6. Edge Case Hardening Separates Exercises From Production Tools

A prompt that works on clean input is an exercise. A prompt that handles UNASSIGNED owners,
TBD deadlines, sarcastic non-suggestions, duplicate mentions, and malformed input is a
production tool. Explicitly handle the messy cases in your prompt design.

**Pattern:** For every field, define what happens when the data is missing, ambiguous, or contradictory.

---

## 7. Structured Output Enables Downstream Integration

Free-form text is useful for humans but useless for systems. Markdown tables, JSON exports,
and standardized schemas turn AI output into pipeline-ready artifacts that can feed into
Jira, Linear, Slack, or any other tool in your workflow.

**Pattern:** Always include a machine-readable export format alongside human-readable output.

---

## 8. Iterate in Small Steps — Fix One Failure Mode Per Version

Resist the urge to redesign the entire prompt when something fails. Identify the specific
failure, add the minimal fix, and test again. The iteration logs in this project show that
each version addresses exactly one weakness — this discipline prevents prompt bloat and
makes it easy to diagnose regressions.

**Pattern:** When a prompt fails, ask "what is the ONE thing that caused this?" and fix only that.
