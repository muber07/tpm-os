# Skill 25 — weekly-highlights-generator

## Purpose
Generate the Weekly Highlights Slider — a concise leadership slide covering the most important cross-program topics of the week and key project updates by program or workstream.

## Inputs
- PROGRAM_STATE.yaml (current truth model, post-verification, post-risk)
- Delta report from delta-analyzer (what changed this week)
- Risk registry (new and worsening risks)
- Decisions log (new decisions this week)

## Output Format

```
WEEKLY HIGHLIGHTS — [Week of DATE]

TOP TOPICS THIS WEEK
• [Topic 1 — cross-program or leadership-level signal. Evidence-backed. One sentence.]
• [Topic 2]
• [Topic 3]
[3–5 bullets max]

KEY PROJECT UPDATES
[Program / Workstream]
• [What changed] — [evidence]
• [What slipped or is blocked] — [owner + evidence]

[Program / Workstream]
• [What moved] — [evidence]
• [What needs attention] — [owner]
```

## Top Topics Rules
- 3–5 bullets maximum — no exceptions
- Prioritize: new RED risks, major decisions made, leadership asks, cross-program dependencies
- Each bullet covers one topic — not a workstream summary
- Evidence-backed: each bullet references a specific event, ticket, or decision from the week
- Cross-program items take priority over single-workstream updates

## Top Topics Selection Criteria (in priority order)
```
1. New RED risk with no mitigation or unowned
2. Major decision made (with exec or partner impact)
3. Critical dependency committed or uncommitted
4. Milestone slip or launch date change
5. Leadership ask or escalation needed
```

## Key Project Updates Rules
- Grouped by Program or Workstream — not by individual
- Show what changed, what moved, what slipped, what is blocked
- One stream = 2–3 bullets max
- Include owner for blocked or slipped items
- GREEN workstreams with no change this week are omitted — silence is correct

## Slide Header
```
WEEKLY HIGHLIGHTS — [Week of YYYY-MM-DD]
Prepared by: TPM OS | Requires TPM review before distribution
```

## Outputs
- Weekly Highlights Slider (Slack-ready or slide-ready)
- Marked DRAFT until Governance Agent approves

## Rules
- Sharp, direct, no fluff — consulting-style
- No motivational language, no vague optimism
- Every bullet is one operational truth
- RED items always appear first in both sections
- This slide is for leadership — remove Jira ticket numbers unless the audience is technical
- Always cite the week date — this is a time-stamped artifact
