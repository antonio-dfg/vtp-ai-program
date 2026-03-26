# Runbook — Trial 2: The "Architect's Constraint" (Prompt Version Walkthrough)

This runbook guides you through running each prompt version in sequence, from the naive v1 to the final v4. The goal is to observe how each iteration closes a specific failure mode in word-ban enforcement.

---

## Prerequisites

- Access to any LLM chat interface (Claude, ChatGPT, Gemini, etc.)
- The sample task below (same for all versions unless noted)

## Sample Task (use for all versions)

Paste the following as your **user message** in every test:

```
Write a 300-word proposal for a green tech hub in Campanhã, Porto.
```

For Version 4 adversarial testing, also try each of these separately:

```
Write a 300-word proposal for a smart city initiative for Lisbon.
```
```
Write a 300-word proposal for a sustainable energy park.
```
```
Write a short innovation accelerator pitch.
```
```
Write a 300-word IoT infrastructure report.
```

---

## Version 1 — Simple Ban List

**System prompt:**

```
Do not use these words in your output: smart, sustainable, innovative, synergy, future.
Write a 300-word proposal for a green tech hub in Campanhã, Porto.
```

**What to look for:**
- The word "sustainable" — expect it to appear at least twice despite the ban
- The word "future" — expect it to appear at least once
- No confirmation that the model checked its own output

**Expected failure:** The model acknowledges the constraint but forgets it mid-generation. A bare list with no alternatives offers no path forward, so the model defaults to its most familiar vocabulary when reaching for the right word.

---

## Version 2 — Added Replacement Guidance

**System prompt:**

```
Write a 300-word proposal for a green tech hub in Campanhã, Porto.

Do NOT use the following words. Use the replacements shown instead:
- smart → "sensor-equipped", "data-informed", or describe the specific technology
- sustainable → "low-emission", "energy-efficient", "net-zero carbon by [year]"
- innovative → describe what is new and why: "the first in Portugal to use..."
- synergy → "coordination between X and Y", "shared infrastructure"
- future → use timeframes: "by 2030", "within the next decade"
```

**What to look for:**
- "sustainable" now replaced by something concrete ("energy-efficient", "low-emission") — should work
- "synergy" avoided or replaced — should work
- "future" — may still slip through once despite the guidance

**Expected partial fix:** Replacement guidance works for most words because it gives the model a concrete alternative path. But without a post-generation self-check, edge cases still slip through toward the end of the response.

---

## Version 3 — Added Self-Check Protocol

**System prompt:**

```
Write a 300-word proposal for a green tech hub in Campanhã, Porto.

BANNED WORDS — do not use any of the following:
smart, sustainable, innovative, synergy, future

For each banned word, use more specific language instead:
- sustainable → "energy-efficient", "low-emission", "net-zero carbon by [year]"
- future → "by 2030", "within the next decade", "over a 15-year horizon"
- smart → describe the actual technology (e.g., "sensor-equipped")
- innovative → describe what is new and why
- synergy → "coordination between X and Y"

SELF-CHECK (mandatory before responding):
Scan every sentence in your output for banned words. If any are found, rewrite that sentence.
Confirm at the very end: "Constraint check passed: 0 banned words detected."
```

**What to look for:**
- All 5 originally-banned words absent
- Confirmation line present: "Constraint check passed: 0 banned words detected."
- Output still coherent and well-structured (not degraded by the constraints)

**Expected improvement:** The self-check protocol acts as a post-generation verification layer. The confirmation line forces the model to commit to a claim it cannot make while knowingly leaving banned words in place. This is the step that closes the remaining gap.

---

## Version 4 (Final) — Expanded List + Persona + Adversarial Testing

**System prompt:** Use the full content of [`system-prompt.md`](./system-prompt.md).

**User message (primary):**
```
Write a 300-word proposal for a green tech hub in Campanhã, Porto.
```

**Run each adversarial message separately with the same system prompt to verify the filter holds.**

**What to look for:**

| Check | Expected result |
|---|---|
| All 15 banned words absent | None present across all test runs |
| Confirmation line | "✅ Constraint check passed: 0 banned words detected." |
| Rewrite counter | "🔄 [N] sentences rewritten" if the model caught any mid-generation |
| Persona applied | Reads like an urban planning document, not a generic pitch |
| Structure followed | Context → Problem → Intervention → Outcome |
| Adversarial topics | Filter holds even for "smart city", "sustainable energy", "innovation" topics |

**Expected outcome:** The filter holds even when the topic naturally attracts banned vocabulary. The persona grounds the tone in professional planning language, which naturally displaces the jargon the filter bans.

---

## Comparison Checklist

After running all four versions, use this table to record your observations:

| Version | Banned words in output | Replacement guidance | Self-check present | Persona present |
|---|---|---|---|---|
| v1 Simple ban list | Multiple | No | No | No |
| v2 Replacement guidance | Fewer | Yes | No | No |
| v3 Self-check protocol | 0 | Partial | Yes | No |
| v4 Final | 0 | Full (15 words) | Yes | Yes |

---

## Key Takeaways to Reinforce

1. **v1 → v2:** A ban list alone doesn't work — the model needs an alternative path. Without replacement guidance, it falls back on the banned words because they are the most natural fit.
2. **v2 → v3:** Replacement guidance reduces failures but doesn't eliminate them. The self-check step is the "compile step" that catches what slips through during generation.
3. **v3 → v4:** Expanding the list to 15 words and adding a persona makes the filter robust against adversarial topics — the ones most likely to attract exactly the words you're trying to ban.
