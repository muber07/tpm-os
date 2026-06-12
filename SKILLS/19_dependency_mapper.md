# Skill 19 — dependency-mapper

## Purpose
Map and track the full dependency graph for a program. Identify which workstreams block which, and which external teams or partners must deliver before internal work can proceed.

## Inputs
- PROGRAM_STATE.yaml `dependencies` and `jira.full_dependreq_status`
- Jira DEPENDREQ data (from jira-ingestor)
- Milestone map (blocking relationships)

## Dependency Types
```
EXTERNAL_TEAM    — another Uber team must deliver (DEPENDREQ)
PARTNER          — external partner must deliver (Bancorp, Tabapay, vendor)
INTERNAL_SEQUENTIAL — workstream B cannot start until workstream A delivers
REGULATORY       — compliance or legal requirement must be met before proceeding
TECHNICAL        — service A must be built before service B can integrate
```

## Process
1. Pull all DEPENDREQs from PROGRAM_STATE.yaml — check current status in Jira
2. For each dependency: check commitment status, owner, last update, ETA
3. Flag stalled dependencies (no update in 14+ days after being filed)
4. Flag uncommitted dependencies (Needs Triage) — these are not committed regardless of program assumptions
5. Build internal dependency sequence — which milestones block which
6. Identify the critical path: longest dependency chain to launch
7. Flag any dependency on the critical path that is uncommitted or stalled

## Dependency Status Codes
```
COMMITTED        — team confirmed delivery
NOT_COMMITTED    — Needs Triage or no response
STALLED          — Waiting on Reporter / no update 14+ days
COMMITTED_STALE  — Committed but no update in 30+ days (re-verify)
CLOSED           — delivered / done
BLOCKED          — dependent team is waiting on something from the requesting team
```

## Outputs
- Full dependency map with current status per dependency
- Critical path dependency list
- Stalled and uncommitted dependency flags
- Internal workstream blocking sequence

## Rules
- DEPENDREQ "Needs Triage" = not committed — always flag, never assume commitment
- A DEPENDREQ committed 90+ days ago with no update is COMMITTED_STALE — re-verify
- If the requesting team hasn't followed up on a stalled DEPENDREQ in 14 days, flag this as an ownership gap
- Partner dependencies (Bancorp, Tabapay) have longer lead times — flag if ETA is within 60 days and not yet started
