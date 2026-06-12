# Skill 17 — risk-registry-manager

## Purpose
Maintain the authoritative risk registry. Track probability, impact, trend, owner, mitigation, and evidence for every active risk.

## Inputs
- New risk signals from ingestion
- Current PROGRAM_STATE.yaml `risks`

## Risk Record Format
```
id: R[N]
description: [one sentence — what could go wrong]
dimension: [Compliance | Technical | Schedule | Dependency | Ownership | Governance]
probability: HIGH | MEDIUM | LOW
impact: CRITICAL | HIGH | MEDIUM | LOW
trend: WORSENING | STABLE | IMPROVING | UNKNOWN
owner: [named person — not a team]
mitigation: [specific action being taken]
evidence: [source reference with date]
flag: RED | YELLOW | GREEN
```

## Flag Rules
```
RED    — HIGH probability + CRITICAL or HIGH impact + no effective mitigation
YELLOW — MEDIUM probability OR mitigation exists and is active
GREEN  — LOW probability OR impact contained
```

## Process
1. For each new RISK signal: check if it matches an existing risk (update) or is new (add)
2. Update trend based on direction: worse since last run = WORSENING, better = IMPROVING
3. Flag risks with no owner — every RED risk must have a named owner
4. Flag risks with no mitigation and flag = RED — these require immediate escalation
5. Mark resolved risks as CLOSED with evidence — do not delete from registry
6. Sort output: RED first, then YELLOW, then GREEN

## Outputs
- Updated risk registry in PROGRAM_STATE.yaml
- New risks list
- Worsening risks list
- Unowned RED risks (escalation required)
- Resolved risks log

## Rules
- Never auto-close a risk — only mark CLOSED with human-confirmed evidence
- UNASSIGNED owner on a RED risk = immediate escalation flag
- Trend UNKNOWN means the risk has not been re-evaluated recently — not the same as STABLE
- A risk mitigated in the last run that has not been re-confirmed is still ACTIVE, not RESOLVED
