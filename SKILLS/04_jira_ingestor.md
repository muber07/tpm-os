# Skill 04 — jira-ingestor

## Purpose
Retrieve execution state from Jira — epics, tasks, DEPENDREQs, and ticket-level progress for a given program.

## Inputs
- Jira project key (e.g. FPEARNERLE)
- DEPENDREQ ticket list from PROGRAM_STATE.yaml
- Cross-project epic list (e.g. FPFOUND, FPLENDING)
- Last scan timestamp

## Process
1. Pull all epics under the program's Jira project — note status, owner, last updated date
2. For each In Progress or recently updated epic: pull sub-tasks and note status distribution (Open / In Progress / Closed)
3. Pull all DEPENDREQ tickets listed in PROGRAM_STATE.yaml — check status (Committed / Needs Triage / Waiting on Reporter / Closed)
4. Scan for newly created tickets in the project since last ingestion
5. Flag epics where: all sub-tasks Open (pre-implementation), epic closed but successor epic open (scope continuation), ticket stale (>7 days no update in active execution)
6. Check cross-project epics (FPFOUND, FPLENDING) for same program scope

## DEPENDREQ Status Interpretation
```
Committed       → team has agreed to deliver
Needs Triage    → not yet reviewed by dependent team — NOT committed
Waiting on Reporter → dependent team pushed back — stalled, needs action from requester
Closed          → done or cancelled
```

## Outputs
- Epic status map (In Progress / Open / Closed) with last update dates
- DEPENDREQ commitment status table
- Newly created or updated tickets since last scan
- Fake green flags (all-Open epics, stale epics, replaced epics)
- Sub-task progress distribution per active epic

## Rules
- Never treat a closed parent epic as "done" without checking sub-tasks and successor epics
- DEPENDREQ "Needs Triage" = not committed — always flag this, never assume commitment
- "Updated today" ≠ "progressing" — check what specifically changed
- Cross-project epics for the same program count — always scan FPFOUND, FPLENDING, etc.
