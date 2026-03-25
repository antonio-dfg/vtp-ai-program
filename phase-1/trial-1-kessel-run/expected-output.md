# Expected Output — Feedback Categorization with Sarcasm Detection

> This is the expected AI output when processing the sample input with the system prompt.
> Use this file to verify your results — do NOT include this in the system prompt itself.

---

## Expected Table

| # | Comment (excerpt) | Sentiment | Department | Urgency | Reasoning |
|---|---|---|---|---|---|
| 1 | New coffee machine brilliant | Positive | Facilities | Low | Genuine praise, no sarcasm indicators |
| 2 | Loved waiting 30 min for bus | **Negative** | Transport/Logistics | Medium | **SARCASM** — "Oh great" + "loved" contradict the negative wait experience |
| 3 | More recycling bins | Neutral | Facilities | Low | Polite request, no strong sentiment |
| 4 | Mentorship program, first deal | Positive | HR | Low | Genuine positive outcome with specific metric |
| 5 | 5 days to replace monitor | Negative | IT Support | High | Frustrated tone, emphasis on delay |
| 6 | Team lunch nice touch | Positive | HR | Low | Genuine appreciation |
| 7 | Wonderful heating broke again | **Negative** | Facilities | Critical | **SARCASM** — "Absolutely wonderful" + "Really love" contradict freezing conditions in January |
| 8 | Onboarding docs clear but... | Mixed | Product | Medium | Positive base with constructive criticism |
| 9 | Parking impossible, late 3x | Negative | Facilities | High | Direct complaint with measurable business impact |
| 10 | Great vibe, best workplace | Positive | General | Low | Genuine praise, consistent tone throughout |

## Expected Sarcasm Audit

- **Comment 2**: "loved" is used sarcastically. The experience described (30-min wait in rain) is objectively negative. Confidence: **High (95%)** — Multiple indicators: exaggerated positive ("Oh great", "loved") + negative situation (waiting in rain) + exclamation mark emphasis.
- **Comment 7**: "Absolutely wonderful" and "Really love" are ironic. Heating failure in January is clearly negative. Confidence: **High (98%)** — Multiple indicators: exaggerated praise ("Absolutely wonderful", "Really love") + negative situation (heating broke, shivering) + repetition of sarcastic pattern.