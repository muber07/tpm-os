# Skill 12 — contradiction-detector

## Purpose
Identify and log conflicts between signals from different sources or within the same source.

## Inputs
- Full signal set with source tags
- Current PROGRAM_STATE.yaml

## Contradiction Types
```
LAUNCH_DATE       — different dates across sources for the same milestone
STATUS_CONFLICT   — one source says In Progress, another says Not Started
COMMITMENT_CONFLICT — DEPENDREQ committed in one source, Needs Triage in another
SCOPE_CONFLICT    — PRD says X is in scope, ERD or Slack says it's out of scope
OWNERSHIP_CONFLICT — different owners named for the same item across sources
INTERNAL          — two conflicting statements within the same document
TEMPORAL          — older source says X, newer source says Y (not necessarily a contradiction — check dates)
```

## Process
1. For each program dimension: compare all signals and flag where values differ across sources
2. Apply source hierarchy — if newer/higher-authority source contradicts older/lower-authority source, flag as temporal contradiction (may be a legitimate update, not an error)
3. For same-authority, same-date conflicts: flag as unresolved contradiction
4. Generate contradiction record: dimension, source A, source B, value A, value B, resolution status

## Contradiction Record Format
```
type: [contradiction type]
dimension: [e.g. launch_date, milestone_status]
source_a: [source name + date]
value_a: [what source A says]
source_b: [source name + date]
value_b: [what source B says]
resolution: UNRESOLVED | RESOLVED_BY_[source] | REQUIRES_HUMAN
evidence: [resolution rationale if resolved]
```

## Outputs
- Contradiction log entries
- Unresolved contradiction list for human review
- Resolved contradiction log with evidence

## Rules
- An unresolved contradiction must surface in executive output — never bury it
- Do not auto-resolve by picking the more recent source if both are authoritative at similar dates
- Internal document contradictions (e.g. two dates in the same PRD) are high-priority — they indicate the document itself is unreliable
