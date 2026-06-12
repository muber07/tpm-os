# Skill 15 — decision-memory-engine

## Purpose
Maintain a persistent, evidence-backed log of confirmed decisions. Prevent decisions from being relitigated. Surface decisions that may be stale or superseded.

## Inputs
- Signal set (DECISION-type signals)
- Current PROGRAM_STATE.yaml `decisions.confirmed`

## Process
1. For each new DECISION signal: check against existing decisions log
2. If new: add to decisions log with source, date, and actor
3. If updates an existing decision: mark previous decision as superseded, log new decision with reference to old
4. If contradicts an existing decision: flag as decision conflict — requires human review
5. Periodically scan decisions for staleness (>60 days old in fast-moving program = review flag)

## Decision Record Format
```
decision: [what was decided — one sentence, imperative]
date: YYYY-MM-DD
owner: [person who made or confirmed the decision]
source: [Slack ref / email / Jira ticket / PRD section]
status: CONFIRMED | SUPERSEDED | UNDER_REVIEW
superseded_by: [reference if applicable]
```

## Outputs
- Updated decisions log in PROGRAM_STATE.yaml
- Decision conflict flags for human review
- Stale decision flags (decisions not re-confirmed in >60 days on fast-moving items)

## Rules
- A decision with no source is not a confirmed decision — flag for evidence
- Superseded decisions are kept in the log — they are historical evidence
- "We agreed to X" in Slack must be cross-referenced against at least one other source (Jira, email, PRD) before being logged as CONFIRMED
- Decisions made by lower-authority stakeholders that contradict decisions by higher-authority stakeholders must be flagged, not silently overwritten
