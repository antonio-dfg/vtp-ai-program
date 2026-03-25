# Trial 3: The "Holocron" Synthesis — Chain-of-Thought & Data Extraction

## Objective

Provide the AI with a long, messy piece of text (meeting transcript) and extract:
1. The 3 biggest risks
2. A structured table of action items
3. A 2-sentence executive summary
4. Two tone rewrites: one for a technical engineer, one for a five-year-old

The "Go Further" artifact adds a **Chain-of-Verification** layer (zero hallucination guarantee)
and exports action items as **JSON importable into Jira/Linear**.

## Files

| File | Description |
|------|-------------|
| `system-prompt.md` | Production-ready prompt system with 6-step pipeline |
| `sample-transcript.md` | Messy 30-minute meeting transcript used for testing |
| `sample-output.md` | Full AI output across all 6 steps with verification audit |
| `export.json` | Jira/Linear-compatible JSON export of action items |
| `iteration-log.md` | How the prompt was refined, including hallucination catches |

## Try it yourself 🚀

1. Copy the system prompt from `system-prompt.md` into your LLM
2. Paste ANY meeting transcript or messy report as the user message
3. The AI will process it through all 6 steps automatically
4. Copy the JSON from Step 6 and import into Jira, Linear, or Trello

## Key Techniques Used

- **Chain-of-Thought**: Strict 6-step sequential pipeline
- **Chain-of-Verification**: AI cross-references every action item against source text
- **Multi-audience adaptation**: Same content rewritten for executive, engineer, and child
- **Structured export**: JSON format directly importable into project management tools
- **Zero-hallucination design**: Items without source evidence are flagged and removed
