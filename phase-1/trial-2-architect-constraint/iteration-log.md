# Iteration Log — Trial 2: The "Architect" Constraint

This log documents how the negative prompting system was refined to reliably enforce constraints.

---

## Version 1 — Simple Ban List

**What I tried:**

Do not use these words in your output: smart, sustainable, innovative, synergy, future.
Write a 300-word proposal for a green tech hub in Campanhã, Porto.

**Result:** The AI avoided "smart" and "synergy" but still used "sustainable" twice and
"future" once. It seemed to "forget" the constraint mid-generation.

**Lesson:** A bare list of banned words isn't enough. The model needs the constraint
reinforced throughout the prompt, and it needs alternatives to reach for instead.

---

## Version 2 — Added Replacement Guidance

**What I changed:** Instead of just banning words, I provided a table with
"Use Instead" alternatives for each banned word.

**Result:** "Sustainable" was replaced by "energy-efficient" and "low-emission."
However, "future" still slipped through once ("in the future, the hub will...").

**Lesson:** Replacement guidance works — it gives the model a concrete path to follow.
But the model still needs a self-check mechanism for words that slip through.

---

## Version 3 — Added Self-Check Protocol

**What I changed:** Added an explicit instruction: "Before delivering your response,
scan every word against the banned list. If any are found, rewrite that sentence."
Also added the confirmation line: "Constraint check passed: 0 banned words detected."

**Result:** All 5 originally-banned words were consistently absent. The confirmation
line appeared at the end. Tested 5 times — passed every time.

**Lesson:** The self-check protocol is critical. It acts as a "compile step" that catches
errors the model made during generation.

---

## Version 4 (Final) — Expanded List + Persona + Adversarial Testing

**What I changed:**
- Added a full persona (Senior Urban Planner with 20 years experience)
- Expanded the banned list from 5 to 15 words
- Added structural requirements (Context → Problem → Intervention → Outcome)
- Tested with adversarial topics: smart city initiative, sustainable energy project, innovation pitch

**Result:** The filter held across all adversarial tests. The persona grounded the tone
so the output reads like a real planning document, not a sanitized corporate pitch.

**Adversarial test results:**

| Topic | Banned Words in Output | Pass? |
|---|---|---|
| Green tech hub in Campanhã | 0 | ✅ |
| Smart city initiative for Lisbon | 0 | ✅ |
| Sustainable energy park proposal | 0 | ✅ |
| Innovation accelerator pitch | 0 | ✅ |
| IoT infrastructure report | 0 | ✅ |

---

## Key Takeaways

1. **Banning words alone doesn't work** — you need replacement guidance so the model has an alternative path
2. **Self-check instructions are essential** — they act as a post-generation verification layer
3. **Personas anchor tone** — without one, the model defaults to generic professional writing that loves buzzwords
4. **Adversarial testing proves robustness** — testing with topics that naturally attract banned words is the real validation
5. **The expandable list makes this a team tool** — anyone can add new buzzwords without redesigning the prompt