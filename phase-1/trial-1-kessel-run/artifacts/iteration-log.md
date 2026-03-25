# Iteration Log — Trial 1: The "Kessel Run"

This log documents how the prompt was refined to achieve reliable sarcasm detection.

---

## Version 1 — Naive Approach

**What I tried:** Simple instruction: "Categorize these comments by sentiment, department, and urgency."

**Result:** The AI classified Comment 2 ("I loved waiting 45 minutes") as **Positive** because
it latched onto the word "loved" without considering context.

**Lesson:** LLMs default to literal interpretation. Sarcasm detection needs to be explicitly requested.

---

## Version 2 — Added Sarcasm Instruction

**What I changed:** Added "Watch out for sarcasm — classify based on intended meaning, not literal words."

**Result:** Comment 2 was now correctly flagged as Negative. However, Comment 7 was still
classified as Positive — the model caught one sarcasm pattern but missed the other.

**Lesson:** A general sarcasm warning isn't enough. The model needs **specific indicators** to look for.

---

## Version 3 — Explicit Sarcasm Indicators + Few-Shot Examples

**What I changed:**
- Added a bulleted list of sarcasm indicators (exaggerated praise + negative situation, etc.)
- Added 2 few-shot examples to set the classification pattern
- Added a Reasoning column so the model must explain its logic

**Result:** Both Comments 2 and 7 were correctly identified as sarcastic and classified as Negative.
The Reasoning column provided clear justification for each detection.

**Lesson:** Combining explicit detection rules with structured output (the Reasoning column)
forces the model to "show its work," which dramatically improves accuracy.

---

## Version 4 (Final) — Added Sarcasm Audit + Confidence Scores

**What I changed:**
- Added a separate "Sarcasm Audit" section after the main table
- Added confidence scores for each sarcasm detection
- Added a self-verification step in the prompt

**Result:** Consistent, high-accuracy output across multiple runs and different LLMs.
The Sarcasm Audit makes the logic transparent for any team member reviewing the output.

---

## Key Takeaways

1. **Few-shot examples are the foundation** — they set the pattern more reliably than instructions alone
2. **Sarcasm detection requires explicit rules** — don't assume the model will "get it"
3. **Forcing explanations improves accuracy** — the Reasoning column acts as a built-in verification layer
4. **Iterate in small steps** — each version fixed one specific failure mode
