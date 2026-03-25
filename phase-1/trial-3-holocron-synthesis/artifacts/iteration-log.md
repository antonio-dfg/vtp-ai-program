# Iteration Log — Trial 3: The "Holocron" Synthesis

This log documents how the Chain-of-Thought pipeline was refined to achieve
zero-hallucination action item extraction.

---

## Version 1 — Single-Prompt Approach

**What I tried:**

Summarize this meeting transcript. List the risks, action items, and write a short summary.

**Result:** The AI produced a reasonable summary but:
- Mixed up who was assigned to what (gave Pedro a task that was Miguel's)
- Invented a deadline that wasn't in the transcript ("by end of month")
- Missed 2 action items entirely (staging dataset access, calendar invite)
- No way to verify whether extracted items were real

**Lesson:** A single vague instruction produces unreliable output. The model needs
a step-by-step pipeline to maintain accuracy across complex extraction tasks.

---

## Version 2 — Explicit Step-by-Step Pipeline

**What I changed:** Broke the task into 4 numbered steps:
1. Identify risks
2. Extract action items
3. Write executive summary
4. Rewrite for different audiences

**Result:** Dramatically improved. All action items were captured, and the risk
identification was accurate. However:
- One action item had the wrong owner (said "Ana" should update the timeline doc, but it was Miguel)
- No mechanism to catch this kind of error

**Lesson:** Step-by-step helps, but without verification the model can still make
attribution errors that look plausible.

---

## Version 3 — Added Chain-of-Verification

**What I changed:** Added Step 5 (Chain-of-Verification) requiring the AI to:
- Quote the source text supporting each action item
- Verify owner and deadline match the source
- Mark items as Verified or Removed

**Result:** The owner attribution error from V2 was caught and corrected.
The verification table made it immediately obvious which items had strong
evidence and which were uncertain.

**Key catch:** In one test run, the AI generated an action item "Set up monitoring
dashboards for the new cluster" — which sounds reasonable but was NEVER
mentioned in the transcript. The verification step caught it: no source quote
could be provided, so it was removed.

**Lesson:** Chain-of-Verification is the most important addition. It turns the
prompt from "probably accurate" to "auditably accurate."

---

## Version 4 — Added JSON Export + Source Evidence Field

**What I changed:**
- Added Step 6 (JSON export) with a strict schema matching Jira/Linear import format
- Added `source_evidence` field in the JSON so every exported task carries its provenance
- Added cross-references between related items (e.g., AI-001 references AI-004)

**Result:** The JSON output was directly importable into Linear. Each task arrived
with its source quote attached, making it easy for the team to verify.

**Lesson:** The export format is what turns this from a "prompt exercise" into an
actual production tool. The source evidence field is key — it means no task exists
in the project management tool without a traceable origin.

---

## Version 5 (Final) — Hardened Against Edge Cases

**What I changed:**
- Added explicit "UNASSIGNED" rule for tasks where no owner was named
- Added "TBD" rule for tasks where no deadline was stated
- Added the instruction "prefer over-flagging uncertainty to under-flagging hallucination"
- Tested with a second, messier transcript (rambling, interruptions, off-topic tangents)

**Edge cases caught:**
| Edge Case | How It Was Handled |
|---|---|
| Task implied but never explicitly assigned | Marked as UNASSIGNED |
| Vague deadline ("soon", "when you can") | Marked as TBD |
| Side conversation unrelated to project | Correctly ignored — no action items extracted |
| Same task mentioned twice by different people | Deduplicated into single item with both source quotes |
| Sarcastic suggestion ("maybe we should just delete prod") | Correctly identified as non-actionable |

---

## Key Takeaways

1. **Step-by-step pipelines beat single-prompt approaches** for complex extraction tasks
2. **Chain-of-Verification is non-negotiable** — it's the difference between "useful" and "trustworthy"
3. **Source evidence fields create accountability** — every extracted task must trace back to the transcript
4. **Edge case hardening matters** — real meetings are messy, with vague assignments and missing deadlines
5. **Export format = production readiness** — JSON that imports into Jira/Linear turns this from an exercise into a tool