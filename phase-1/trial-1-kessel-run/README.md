#  Trial 1: The "Kessel Run" (Few-Shot Prompting & Sarcasm Detection)

## Objective

Categorize 10 disorganized customer feedback comments using few-shot prompting.

The AI must correctly detect sarcasm and classify based on **intended** sentiment,
not literal word choice.

## Files

| File | Description |
|------|-------------|
| `system-prompt.md` | Reusable System Prompt Template (the main deliverable) |
| `sample-input.md` | 10 customer feedback comments used for testing |
| `expected-output.md` | Expected AI output for verification |
| `iteration-log.md` | How the prompt was refined across versions |
| `runbook.md` | Step-by-step guide to test each prompt version |

## Try it yourself 🚀

1. Copy the contents of [system-prompt.md](https://github.com/antonio-dfg/vtp-ai-program/blob/main/phase-1/trial-1-kessel-run/artifacts/system-prompt.md) into your LLM's system prompt field or add it to your message
2. Paste the comments from [sample-input.md](https://github.com/antonio-dfg/vtp-ai-program/blob/main/phase-1/trial-1-kessel-run/artifacts/sample-input.md) as the user message or use them as inspiration
3. Verify the output against the expected results ([expected-output.md](https://github.com/antonio-dfg/vtp-ai-program/blob/main/phase-1/trial-1-kessel-run/artifacts/expected-output.md))

To understand the full prompt evolution, follow the [runbook](https://github.com/antonio-dfg/vtp-ai-program/blob/main/phase-1/trial-1-kessel-run/artifacts/runbook.md).

## Key Techniques Used

- **Few-shot examples**: Pre-classified examples to set the pattern
- **Explicit sarcasm detection rules**: Forces the model to check for tone/content contradictions
- **Reasoning column**: Makes logic transparent and auditable
- **Sarcasm Audit section**: Separate verification block with confidence scores
- **Structured output**: Markdown table for readability + easy copy-paste
