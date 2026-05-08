# Eval: Communication Modes

## Applies To
Voice skill — Thread 3 (Communication Modes)

## Criteria
- Modes emerge from the user's actual communication surfaces, not from a template
- Coach maps communication surfaces before defining modes
- Each mode has sufficient depth (reader posture, rhythm, idea development, patterns, tone calibration)
- Modes are distinct from each other — each serves a different reader posture and communication job
- Common patterns are offered as prompts when the user is stuck, not as prescriptions
- Each mode gets a stable snake_case ID

## Pass Indicators
- Coach asks about where the studio actually communicates before suggesting modes
- Mode count matches the user's actual needs (could be 2, could be 7)
- Each mode is grounded in a specific communication surface the user identified
- When referencing common patterns (outreach, teaching, etc.), coach presents them as "does this match what you're describing?" not "you should have an outreach mode"
- Modes that the user doesn't need are not included just for completeness
- Vocabulary exceptions are captured when a mode overrides global rules
- Stable IDs are assigned and noted

## Fail Indicators
- Coach prescribes 5 modes matching outreach/conversational/teaching/research/proposal without grounding in user needs
- Modes are defined without first mapping communication surfaces
- Modes are shallow — only name and description, missing rhythm, patterns, tone calibration
- All modes sound the same — no meaningful differentiation between them
- Common patterns are presented as requirements rather than reference points
- No stable IDs assigned

## Weight
High — modes are the practical layer of the voice system. They determine how the studio actually sounds in specific contexts. Generic or prescriptive modes produce a voice system the user won't trust or use.
