# Skill 21 — narrative-compressor

## Purpose
Compress complex, multi-source program truth into concise, high-signal executive narrative.

## Inputs
- PROGRAM_STATE.yaml (full program truth model)
- Target output type and audience
- Bullet/word count constraint

## Compression Rules
```
One bullet = one operational truth
No bullet without evidence
Lead with status → evidence → implication
Numbers over adjectives: "3 of 8 milestones at risk" not "several milestones at risk"
Negative news first — do not bury blockers
Cut filler: "It's worth noting that", "Moving forward", "As we can see"
Cut adjectives that don't add information: show why something matters instead
If two sentences say the same thing, cut one
```

## Process
1. Identify the 3–5 most operationally significant facts in current state.
2. Strip each to: subject + verb + evidence + implication.
3. Order: worst news first, then context, then ask.
4. Validate: does every sentence carry a unique operational truth?

## Audience Calibration
```
Exec / VP     → business impact framing, no Jira ticket numbers, lead with risk and ask
XFN team      → workstream detail, named owners, specific blockers
Eng team      → technical context, Jira references OK, implementation evidence
Partner       → compliance and integration framing, no internal org detail
```

## Outputs
- Compressed narrative block ready for any output type
- Sentence-level evidence tags (internal — not shown in final output)

## Rules
- Never exceed the target bullet count — compression is the job
- If a fact can't be expressed in one bullet without losing essential meaning, flag for human editing
- Tone: sharp, skeptical, direct — never optimistic without evidence
