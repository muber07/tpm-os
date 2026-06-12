# Skill 23 — uber-executive-renderer

## Purpose
Render the final executive artifact in Uber-native format. Sharp, direct, no fluff, evidence-backed, consulting-style compression.

## Inputs
- Storyline structure from slide-storyliner
- Compressed narrative from narrative-compressor
- Output type: Slack update | one-slider | status board | one-pager | Gantt | review packet | weekly highlights

## Format Specs

### Slack Executive Update
```
*[Program Name] — [Date]*

🔴 / 🟡 / 🟢 [Overall status — one word]

• [Top signal #1 — what changed, why it matters]
• [Top signal #2]
• [Top signal #3]
• [Risk or blocker — specific, named owner]
• [Ask — what you need from this audience]
```

### Uber-Style One-Slider
```
TITLE: [Action-oriented — verb phrase, not a noun]

WHERE WE ARE
• [current state, evidence-backed]

WHAT CHANGED
• [delta bullets — specific and dated]

RISKS & IMPACT
• [RED risks first — probability + impact + owner]

WHAT WE NEED
• [specific ask — owner + due date]

BOTTOM LINE
[One sentence. The most important thing. What this means for launch.]
```

### Operational Status Board
```
[PROGRAM] STATUS BOARD — [Date]

STREAM          | STATUS | OWNER           | TOP BLOCKER              | ETA
----------------|--------|-----------------|--------------------------|----------
Disbursements   | 🔴 RED | Alcides         | Pre-implementation       | Unknown
Compliance      | 🔴 RED | Balaji/Charu    | BRD not approved         | June start
Applications    | 🟡 AMB | Daniele Boscolo | ERDs not finalized       | TBD
Entrypoint      | 🟡 AMB | Daniele Boscolo | UUE Gateway in progress  | ~Jun
Identity/IDVP   | 🟡 AMB | Shahan Din      | DEPENDREQ-62186 open     | Jul 10
Payments/ACH    | 🔴 RED | Rajdeep/Alcides | ERD incomplete, Aug ETA  | Aug
```

### Weekly Highlights Slider
See `25_weekly_highlights_generator.md`

## Rules
- Tone: never optimistic without evidence, never vague, never motivational
- RAG status must be earned — GREEN requires evidence, not absence of visible problems
- Bottom line is always the sharpest sentence — write it last, place it last
- All artifacts marked DRAFT until Governance Agent approves
