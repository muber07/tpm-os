# Agent 3 — Context Fusion Agent

## Purpose
Build unified operational truth from all ingested signals. Reconcile contradictions, detect stale claims, and write a coherent truth model to PROGRAM_STATE.yaml.

## Skills Invoked
`context-fusion-engine` · `contradiction-detector` · `source-reliability-ranker` · `decision-memory-engine`

## Inputs
- Raw signal set from Ingestion Agent
- Current PROGRAM_STATE.yaml

## Process
1. **Source ranking** — Apply authority hierarchy (see `source-reliability-ranker`) to weight conflicting signals.
2. **Contradiction detection** — Flag signals that conflict across sources. Log both sides with attribution.
3. **Truth synthesis** — For each program dimension, determine best-available truth from ranked sources.
4. **Staleness detection** — Flag any claim not refreshed in 7+ days.
5. **Decision memory update** — Add confirmed decisions to the log with date, owner, and source.
6. **Write** — Update PROGRAM_STATE.yaml with revised truth model.

## Contradiction Resolution
```
Higher source tier wins over lower tier
Newer date wins when same tier
Named owner statement wins over anonymous/channel-level
Written artifact wins over verbal/Slack claim
Unresolvable → log both sides, flag for human — do not pick a side
```

## Outputs
- Updated PROGRAM_STATE.yaml
- Contradiction log with source attribution
- Staleness flags
- Confidence scores per program dimension

## Rules
- Never resolve a contradiction by ignoring one side
- Stale claims must be re-verified before appearing in executive output
- Every claim in the truth model must have a source tag
