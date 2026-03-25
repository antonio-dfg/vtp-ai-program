# Trial 1: The "Kessel Run" — Few-Shot Prompting & Sarcasm Detection

## Objective

Categorize 10 disorganized customer feedback comments using few-shot prompting.

The AI must correctly detect sarcasm and classify based on **intended** sentiment,
not literal word choice.

## Files

| File | Description |
|------|-------------|
| `system-prompt.md` | Reusable System Prompt Template (the main deliverable) |
| `sample-input.md` | 10 customer feedback comments used for testing |
| `iteration-log.md` | How the prompt was refined across versions |

## Try it yourself

1. Copy the contents of `system-prompt.md` into your LLM's system prompt field
2. Paste the comments from `sample-input.md` as the user message
3. Verify the output matches the expected results in the system prompt
4. The template works with any LLM (Claude, GPT-4, Gemini, etc.)

## Key Techniques Used

- **Few-shot examples**: 2 pre-classified examples to set the pattern
- **Explicit sarcasm detection rules**: Forces the model to check for tone/content contradictions
- **Reasoning column**: Makes logic transparent and auditable
- **Sarcasm Audit section**: Separate verification block with confidence scores
- **Structured output**: Markdown table for readability + easy copy-paste
