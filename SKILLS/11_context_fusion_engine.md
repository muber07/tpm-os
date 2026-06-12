# Skill 11 — context-fusion-engine

## Purpose
Merge all ingested signals into a single unified operational truth. Reconcile contradictions, assign confidence, and update PROGRAM_STATE.yaml.

## Inputs
- Raw signal set from Ingestion Agent
- Current PROGRAM_STATE.yaml

## Process
1. Group signals by dimension: milestones, risks, dependencies, decisions, questions.
2. Apply source authority hierarchy (see `source-reliability-ranker`) to weight conflicting signals.
3. Log contradictions with both sides and source attribution — do not resolve unilaterally.
4. Build best-available truth per dimension from ranked signals.
5. Flag claims not refreshed in 7+ days as stale.
6. Add confirmed decisions to decision log with date, owner, source.
7. Write updated truth model to PROGRAM_STATE.yaml.

## Confidence Rules
```
All sources agree               = HIGH
Minor conflict, clear winner    = MEDIUM
Unresolved contradiction        = LOW
Missing evidence                = LOW or VERY LOW
```

## Outputs
- Updated PROGRAM_STATE.yaml
- Contradiction log (unresolved and resolved, with attribution)
- Staleness flags
- Confidence scores per dimension

## Rules
- Never drop one side of a contradiction — log both
- Stale claims must be marked — do not present as current
- Every claim must have a source tag
