# Skill 21 — narrative-compressor

## Purpose
Compress complex, multi-source program truth into concise, high-signal executive narrative. Remove noise. Preserve operational truth.

## Inputs
- PROGRAM_STATE.yaml (full program truth model)
- Target output type and audience
- Word/bullet count constraint

## Compression Rules
```
One bullet = one operational truth
No bullet without evidence
No sentence longer than needed
Lead with the status, follow with evidence, end with implication
Cut adjectives that don't add information ("significant", "important", "critical" — show why instead)
Cut filler phrases ("It's worth noting that", "As we can see", "Moving forward")
Negative news first — do not bury blockers in the middle
Numbers over adjectives ("3 of 8 milestones at risk" over "several milestones at risk")
```

## Compression Process
1. Identify the 3–5 most operationally significant facts in the current state
2. For each fact: strip to subject + verb + evidence + implication
3. Order: worst news first, then context, then asks
4. Validate: does each sentence convey a unique operational truth? If two sentences say the same thing, cut one.

## Audience Calibration
```
Executive / VP    → business impact framing, no Jira ticket numbers, lead with risk and ask
XFN team          → workstream-level detail, named owners, specific blockers
Eng team          → technical context, Jira references acceptable, implementation evidence
Partner (Bancorp) → compliance and integration framing, no internal org detail
```

## Outputs
- Compressed narrative block ready for use in any output type
- Sentence-level evidence tags (internal — not shown in final output)

## Rules
- Never exceed the target bullet count — compression is the job
- If a fact cannot be expressed in one bullet without losing essential meaning, flag it for human editing rather than expanding
- Tone: sharp, skeptical, direct. Never optimistic without evidence.
