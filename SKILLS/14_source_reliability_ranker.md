# Skill 14 — source-reliability-ranker

## Purpose
Assign authority weights to sources when resolving conflicting signals. This hierarchy is the single source of truth — referenced by Agents 1, 3, and Skill 11.

## Source Authority Hierarchy
```
1. PRD           — primary product truth
2. ERD           — technical design truth
3. uPlan         — program plan truth
4. Jira          — ticket-level execution state
5. GitHub        — implementation evidence (PRs, commits, deployments)
6. Slack         — operational signals and real-time decisions
7. Gmail         — email decisions and commitments
8. Meeting notes — verbal commitments and context (lowest authority)
```

## Tiebreaker Rules (when same tier)
```
Newer date > older date
Named owner statement > anonymous or channel-level
Written artifact > verbal/Slack claim
Specific reference (ticket, PR number) > general claim
```

## When to Escalate to Human
- Two sources at the same tier directly contradict each other
- A lower-tier source contains specific evidence (e.g. screenshot, ticket) that contradicts a higher-tier source
- The contradiction is launch-date or compliance-critical

## Outputs
- Source ranking applied to each contested signal
- Escalation flags for unresolvable conflicts

## Rules
- Never use a lower-tier source to override a higher-tier one without flagging it
- Recency alone does not override source tier — a recent Slack message does not supersede the PRD
- If the PRD itself is internally contradictory, flag it — do not pick a side
