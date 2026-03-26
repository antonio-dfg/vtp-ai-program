# Runbook — Trial 1: The "Kessel Run" (Prompt Version Walkthrough)

This runbook guides you through running each prompt version in sequence, from the naive v1 to the final v4. The goal is to observe how each iteration fixes a specific failure mode.

---

## Prerequisites

- Access to any LLM chat interface (Claude, ChatGPT, Gemini, etc.)
- The sample input below (same for all versions)

## Sample Input (use for all versions)

Paste the following as your **user message** in every test:

```
Categorize the following feedback comments:

1. "The new coffee machine on the shared area is brilliant, best upgrade this year."
2. "Oh great, another rainy day in Porto, I loved waiting 45 minutes for the bus!"
3. "Could we get more recycling bins near the kitchen area?"
4. "The mentorship program helped me close my first enterprise deal in 3 weeks."
5. "IT took 5 days to replace my broken monitor. Five. Days."
6. "Team lunch last Friday was a nice touch, thanks to HR for organizing."
7. "Absolutely wonderful that the heating broke again in January. Really love shivering at my desk."
8. "The onboarding documentation is clear but could use more examples for the API section."
9. "Parking situation is impossible. I have been late to 3 meetings this month because of it."
10. "The Porto office has a great vibe, honestly one of the best places I have worked!"
```

---

## Version 1 — Naive Approach

**System prompt:**

```
Categorize these comments by sentiment, department, and urgency.
```

**What to look for:**
- Comment 2 ("I loved waiting 45 minutes") — expect it to be classified as **Positive** (wrong)
- Comment 7 ("Absolutely wonderful that the heating broke") — expect **Positive** (wrong)

**Expected failure:** The model latches onto literal positive words ("loved", "wonderful") and misses the sarcasm.

---

## Version 2 — Added Sarcasm Instruction

**System prompt:**

```
Categorize these comments by sentiment, department, and urgency.

Watch out for sarcasm — classify based on intended meaning, not literal words.
```

**What to look for:**
- Comment 2 — should now be **Negative** (fixed)
- Comment 7 — likely still **Positive** (still broken)

**Expected partial fix:** A generic sarcasm warning catches obvious cases but misses subtler ones. No indicators = no reliability.

---

## Version 3 — Explicit Sarcasm Indicators + Few-Shot Examples

**System prompt:**

```
You are a Customer Feedback Analyst for a Portuguese tech company's Porto office.
Categorize each comment into a table: # | Comment | Sentiment | Department | Urgency | Reasoning

Sentiment: Positive, Negative, Neutral, or Mixed
Department: Facilities, IT Support, HR, Management, Product, Transport/Logistics, General
Urgency: Low, Medium, High, Critical

SARCASM DETECTION — before classifying, check for:
- Exaggerated positive language paired with a negative situation
- Exclamation marks combined with complaints
- Phrases like "Oh great", "love it when", "so wonderful that"
- Contradiction between literal words and described experience

If sarcasm is detected, classify based on INTENDED sentiment. Explain sarcasm in the Reasoning column.

Examples:
Comment: "Oh, I just love when the elevator is out of service on a Monday morning."
→ Sentiment: Negative | Reasoning: SARCASM — "just love" is ironic; broken elevator is objectively negative.

Comment: "The new standing desks are amazing, my back pain is completely gone!"
→ Sentiment: Positive | Reasoning: Genuine praise, no sarcasm indicators.
```

**What to look for:**
- Comment 2 — **Negative** (correct)
- Comment 7 — **Negative** (now also correct)
- Reasoning column populated with justification for each classification

**Expected improvement:** Both sarcastic comments detected. The Reasoning column forces the model to show its logic.

---

## Version 4 (Final) — Sarcasm Audit + Confidence Scores + Self-Verification

**System prompt:** Use the full content of [`system-prompt.md`](./system-prompt.md).

**What to look for:**

| Check | Expected result |
|---|---|
| Comment 2 | Negative, sarcasm flagged, confidence score |
| Comment 7 | Negative, sarcasm flagged, confidence score |
| Sarcasm Audit section | Present after the main table |
| Self-verification line | "✅ Verification: 2 sarcastic comments detected!" |
| CSV block | Present, correctly formatted |

**Expected outcome:** Consistent, auditable output with full reasoning. Sarcasm Audit makes the logic reviewable by any team member. CSV block ready for export.

---

## Comparison Checklist

After running all four versions, use this table to record your observations:

| Version | Comment 2 | Comment 7 | Reasoning provided | Sarcasm Audit | Confidence Scores |
|---|---|---|---|---|---|
| v1 Naive | ❌ Positive | ❌ Positive | No | No | No |
| v2 Generic warning | ✅ Negative | ❌ Positive | No | No | No |
| v3 Indicators + few-shot | ✅ Negative | ✅ Negative | Yes | No | No |
| v4 Final | ✅ Negative | ✅ Negative | Yes | Yes | Yes |

---

## Key Takeaways to Reinforce

1. **v1 → v2:** Sarcasm detection must be explicitly requested — LLMs default to literal reading.
2. **v2 → v3:** Generic warnings are insufficient; explicit indicators + few-shot examples are the fix.
3. **v3 → v4:** Forcing explanations (Reasoning column) and adding an audit step improves consistency and transparency across runs and models.
