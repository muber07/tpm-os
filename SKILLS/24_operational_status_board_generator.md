# Skill 24 — operational-status-board-generator

## Purpose
Generate a per-workstream RAG status board. One line per workstream. RED first. Every status backed by evidence.

## Inputs
- PROGRAM_STATE.yaml (milestone statuses, risks, dependencies per workstream)
- Execution verification results
- Risk registry

## RAG Rules
```
RED   — delivery-threatening blocker, pre-implementation with launch pressure, unowned critical risk
AMBER — at-risk but mitigation exists, stalled but not blocking launch yet, unresolved question with ETA
GREEN — on track, evidence of active progress, no unowned blockers
```

## Status Board Row Format
```
[Stream] | [🔴/🟡/🟢 STATUS] | [Owner] | [Top Blocker or Evidence] | [ETA]
```

## Process
1. For each workstream in the program: pull milestone status, active risks, DEPENDREQ status
2. Apply RAG logic — do not self-report GREEN without execution evidence
3. Sort: RED first, AMBER second, GREEN last
4. For RED: the top blocker must be specific (not "multiple issues")
5. For GREEN: the evidence must be cited (not assumed)
6. Add program-level summary row at top

## Board Header Format
```
[PROGRAM] — OPERATIONAL STATUS
As of: [date]
Overall: 🔴 RED | 🟡 AMBER | 🟢 GREEN

[rows sorted RED → AMBER → GREEN]

LAUNCH DATE: [best estimate] (Confidence: LOW/MEDIUM/HIGH)
NEXT ACTION: [most critical unblock needed]
```

## Outputs
- Formatted status board ready for Slack or slide
- Workstream-level evidence log (internal, not shown in output)

## Rules
- Never show GREEN for a workstream with an unowned RED risk
- Never show GREEN for work with no implementation started during active execution phase
- AMBER is not "probably fine" — it means "real risk, active mitigation"
- Every status must have a source — if evidence is missing, default to AMBER not GREEN
- When a milestone is AT_RISK or BLOCKED: state the specific sourced facts (Jira ticket, Slack message, date) — do not use editorial language or invented characterizations
- Do not assign AT_RISK or BLOCKED without citing the evidence that supports it
