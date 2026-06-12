# Skill 18 — delta-analyzer

## Purpose
Compare current program state against the previous run. Surface what is new, changed, worsened, improved, or resolved. Focus output on what matters — not what stayed the same.

## Inputs
- Previous PROGRAM_STATE.yaml snapshot
- Current PROGRAM_STATE.yaml (post-fusion, post-verification)

## Delta Dimensions
```
MILESTONE_CHANGE     — status changed (up or down)
RISK_FLAG_CHANGE     — flag changed (GREEN→YELLOW, YELLOW→RED, etc.)
DEPENDENCY_CHANGE    — DEPENDREQ status changed (Needs Triage→Committed, Committed→Stalled, etc.)
NEW_BLOCKER          — blocker not present in previous run
RESOLVED_BLOCKER     — blocker from previous run now cleared
NEW_DECISION         — decision added since last run
OWNERSHIP_CHANGE     — owner changed for a risk, milestone, or dependency
LAUNCH_DATE_CHANGE   — best estimate launch date changed
CONFIDENCE_CHANGE    — confidence score dropped or improved significantly
NEW_QUESTION         — unresolved question added
CLOSED_QUESTION      — previously open question now answered
```

## Process
1. For each dimension: diff current vs. previous state
2. Classify each difference by delta type
3. Sort by severity: worsening changes first, then new items, then improvements, then resolved
4. Generate delta report

## Delta Report Format
```
WORSENING:
  - [dimension]: [what changed] — [why it matters]

NEW:
  - [dimension]: [what appeared]

IMPROVING:
  - [dimension]: [what improved] — [evidence]

RESOLVED:
  - [dimension]: [what was resolved] — [evidence]

NO CHANGE:
  [not listed — silence is correct here]
```

## Outputs
- Delta report (worsening → new → improving → resolved)
- Escalation list (anything that worsened with no mitigation)

## Rules
- No-change items are not reported — delta-first means only what moved
- Worsening items always appear first — do not bury them
- An improvement without evidence is not confirmed — mark as unverified improvement
- If the delta is large (many changes), flag that the state has shifted significantly — may require full re-briefing of stakeholders
