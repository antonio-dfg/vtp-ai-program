# Runbook — Trial 3: The "Holocron" Synthesis (Prompt Version Walkthrough)

This runbook guides you through running each prompt version in sequence, from the naive v1 to the final v5. The goal is to observe how each iteration improves accuracy, eliminates hallucinations, and adds production-ready export capability.

---

## Prerequisites

- Access to any LLM chat interface (Claude, ChatGPT, Gemini, etc.)
- The sample transcript below (same for all versions)

## Sample Input (use for all versions)

Paste the full content of [`sample-transcript.md`](./sample-transcript.md) as your **user message** in every test.

---

## Version 1 — Single-Prompt Approach

**System prompt:**

```
Summarize this meeting transcript. List the risks, action items, and write a short summary.
```

**What to look for:**
- Incorrect owner assignments — a task attributed to the wrong person (e.g., Miguel's task attributed to Ana, or vice versa)
- An invented deadline that doesn't appear in the transcript ("by end of month" is a common fabrication)
- Missing action items — the staging dataset access grant and the calendar invite are frequently dropped
- No way to verify whether any extracted item is real

**Expected failure:** A single vague instruction produces unreliable output. The model conflates speakers, invents plausible-sounding deadlines, and misses implicit tasks because there is no mechanism forcing it to trace each claim back to the source.

---

## Version 2 — Explicit Step-by-Step Pipeline

**System prompt:**

```
Process this meeting transcript in 4 steps:

1. RISKS: Identify the top 3 risks mentioned or implied.
   For each: risk name, direct quote or close paraphrase from the source, impact level (Critical/High/Medium/Low).

2. ACTION ITEMS: Extract every commitment or next step.
   For each: task description, owner, deadline (as stated in transcript), priority (P0–P3).

3. EXECUTIVE SUMMARY: Write exactly 2 sentences.
   Sentence 1: the most important decision or status update.
   Sentence 2: the single biggest risk or blocker.
   Target audience: a C-level executive scanning their inbox.

4. AUDIENCE REWRITES: Rewrite the executive summary in two voices:
   A) For a senior technical engineer — use system names, metrics, and architectural detail.
   B) For a five-year-old — simple words, a relatable analogy, maximum 2 sentences.
```

**What to look for:**
- All action items captured (compare against the full list in the sample output)
- Risk identification is accurate and grounded
- One subtle owner attribution error may still appear — check each assigned owner against the transcript
- No mechanism exists to catch or surface this kind of error

**Expected partial fix:** Step-by-step processing dramatically improves completeness. However, without a verification step, plausible-but-wrong attributions still pass through looking identical to correct ones.

---

## Version 3 — Added Chain-of-Verification

**System prompt:**

```
Process this meeting transcript in 5 steps:

[Steps 1–4 as above]

5. VERIFICATION AUDIT: For each action item from Step 2:
   - Copy the exact source quote that generated this item
   - Challenge the owner: could this task belong to someone else? Did the speaker assign it or merely suggest it?
   - Challenge the deadline: does it clearly refer to THIS task, or could it apply to something else?
   - Challenge the task itself: is this a firm commitment, a hypothetical, or a passing thought?
   - Mark as Verified or Removed
   - Explain any removals with a brief note

   End with: "Verification complete: [X] items verified. [Y] items removed due to insufficient evidence."
```

**What to look for:**
- Any owner attribution error from v2 is now caught and corrected in the audit table
- Items that were invented (no source quote can be found) are removed
- The verification table makes it immediately visible which items have strong evidence and which are uncertain
- The adversarial framing ("actively attempt to disprove") catches items a simple confirmation step would miss

**Expected improvement:** The Chain-of-Verification turns the output from "probably correct" to "auditably correct." In real test runs, at least one hallucinated item will be removed — typically something that sounds reasonable but was never stated in the transcript.

---

## Version 4 — Added JSON Export

**System prompt:**

```
Process this meeting transcript in 6 steps:

[Steps 1–5 as above]

6. JSON EXPORT: Export only the verified action items (those that passed Step 5) as a JSON array.
   Use this schema for each item:
   {
     "id": "AI-001",
     "title": "Short task title",
     "description": "Full task description with context",
     "assignee": "Person Name or UNASSIGNED",
     "due_date": "YYYY-MM-DD or TBD",
     "priority": "P0|P1|P2|P3",
     "status": "To Do",
     "labels": ["meeting-action", "auto-extracted"],
     "dependencies": ["AI-002"],
     "confidence": "High|Medium|Low",
     "source_evidence": "Quoted text from transcript that supports this item"
   }
```

**What to look for:**
- JSON is syntactically valid (no missing brackets, no trailing commas)
- Only items that passed Step 5 verification appear in the JSON
- `source_evidence` field is populated for every single item — no empty strings
- `due_date` is in ISO 8601 format (YYYY-MM-DD) where a date was stated, "TBD" otherwise
- `dependencies` field cross-references related items by ID (e.g., AI-002 references AI-001)

**Expected improvement:** The export format turns the prompt from an exercise into a production tool. The `source_evidence` field means every task that lands in Linear or Jira carries its traceable origin — no task exists without a source.

---

## Version 5 (Final) — Edge Case Hardening

**System prompt:** Use the full content of [`system-prompt.md`](./system-prompt.md).

**What to look for:**

| Edge Case | Expected result |
|---|---|
| Task with no named owner | `assignee` = "UNASSIGNED" |
| Vague deadline ("soon", "when you can") | `due_date` = "TBD" |
| Off-topic side conversation | No action items extracted from it |
| Same task mentioned twice | Single deduplicated item with both source quotes |
| Sarcastic or hypothetical suggestion | Not extracted — flagged as non-actionable |
| Ambiguous ownership | Flagged as Medium or Low confidence |
| JSON validity | No syntax errors, only Step 5–verified items |

**Expected outcome:** Consistent, auditable output that handles the messy realities of real meetings — vague assignments, missing deadlines, off-topic tangents, and duplicates. The "prefer over-flagging uncertainty to under-flagging hallucination" rule means ambiguous items surface for human review rather than being silently included or silently dropped.

---

## Comparison Checklist

After running all five versions, use this table to record your observations:

| Version | All items captured | Owner errors | Hallucinated items | Verification audit | JSON export |
|---|---|---|---|---|---|
| v1 Single prompt | No | Yes | Yes | No | No |
| v2 Step-by-step | Yes | Possible | Possible | No | No |
| v3 + Verification | Yes | No | No | Yes | No |
| v4 + JSON export | Yes | No | No | Yes | Yes |
| v5 Final | Yes | No | No | Yes | Yes (hardened) |

---

## Key Takeaways to Reinforce

1. **v1 → v2:** Single-prompt extraction is unreliable for complex tasks. Step-by-step pipelines maintain accuracy by forcing the model to reason sequentially rather than summarize holistically.
2. **v2 → v3:** Steps alone are not enough — verification is non-negotiable. The adversarial framing ("attempt to disprove") is what catches attribution errors that polite confirmation misses.
3. **v3 → v4:** Source evidence fields create accountability. Every exported task must trace back to the transcript — no quote, no task. This is what separates a demo from a production tool.
4. **v4 → v5:** Edge case hardening matters for real-world use. Explicit UNASSIGNED and TBD rules prevent false precision on ambiguous data, and the deduplication rule handles the reality that people repeat themselves in meetings.
