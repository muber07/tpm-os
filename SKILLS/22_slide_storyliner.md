# Skill 22 — slide-storyliner

## Purpose
Structure program truth into a logical slide narrative. Determine the right storyline before generating content. One slide, one argument.

## Inputs
- Compressed narrative from narrative-compressor
- Output type (one-slider, stream one-pager, weekly highlights)
- Audience

## Storyline Patterns

### Status Update Storyline (most common)
```
1. Where we are [current state, honest]
2. What changed [delta from last period]
3. Risks and impact [what threatens the plan]
4. What we need [asks / decisions / unblocks]
5. Bottom line [one sentence — what this means for launch]
```

### Escalation Storyline
```
1. The problem [specific, evidence-backed]
2. Why it matters [impact on launch / business]
3. What's been tried [mitigation attempted]
4. What's needed [specific ask from this audience]
5. Consequence of inaction [if ask is not met, what happens]
```

### Decision Briefing Storyline
```
1. The question [what needs to be decided]
2. Context [relevant facts, not background]
3. Options [2–3 max, with tradeoffs]
4. Recommendation [with evidence]
5. Ask [specific decision needed]
```

## Process
1. Identify the primary purpose of this output (status, escalation, or decision)
2. Select the appropriate storyline pattern
3. Map compressed narrative blocks to storyline sections
4. Validate: does the slide tell one coherent story? If not, split or cut.

## Outputs
- Slide structure with section labels and content mapped to each section
- Ready for uber-executive-renderer to format

## Rules
- One slide = one argument — if it takes more than one argument, it's two slides
- The bottom line must be the sharpest, most action-oriented sentence on the slide
- Never start with background — start with the current state or the problem
- Every section must earn its place — if a section has no new information, cut it
