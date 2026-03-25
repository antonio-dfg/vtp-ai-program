# Trial 2: The "Architect" Constraint — Precision & Negative Prompting

## Objective

Draft a 300-word proposal for a new project in Porto using a persona (Senior Urban Planner)
while applying **negative prompting** — the output cannot contain the words:
`smart`, `sustainable`, `innovative`, `synergy`, or `future`.

## Files

| File | Description |
|------|-------------|
| `system-prompt.md` | The "Architect's Filter" — reusable system instruction with negative constraints |
| `negative-constraint-list.md` | Living document of banned buzzwords (expandable by the team) |
| `sample-output.md` | Example AI output that passes all constraints |
| `iteration-log.md` | How the prompt was refined to reliably enforce constraints |

## Try it yourself
1. Copy the system prompt from `system-prompt.md` into your LLM
2. Provide any drafting task as the user message (proposal, report, email, etc.)
3. The filter will automatically strip corporate jargon while maintaining a professional tone
4. To ban new words, add them to `negative-constraint-list.md` and update the system prompt

## Key Techniques Used

- **Persona prompting**: "Act as a Senior Urban Planner" to anchor tone and expertise
- **Negative prompting**: Explicit list of banned words with replacement guidance
- **Self-check instruction**: AI scans its own output before delivering
- **Concrete replacement patterns**: Instead of banning words blindly, the prompt teaches *what to use instead*
- **Stress-tested**: Validated against topics that naturally attract banned words (green tech, IoT, urban planning)
