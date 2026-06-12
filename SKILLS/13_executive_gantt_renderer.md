# Skill 13 — executive-gantt-renderer

## Purpose
Generate a concise executive-style operational timeline for leadership review. One row per workstream. Weekly or milestone-based cadence. Optimized for scanability — not task-level detail.

## Inputs
- PROGRAM_STATE.yaml milestones, blockers, ETAs, checkpoint dates
- Launch date and confidence

## Rendering Rules

### Structure
- One row = one workstream or operational stream
- Columns = weeks or named milestone windows (not days)
- Bars contain short operational labels only

### Bar Labels
```
IN PROGRESS
DUE [MON DD]
WEEKS [N-N]
READY FOR REVIEW
BLOCKED
NOT STARTED
[CHECKPOINT NAME]
LAUNCH
```

### What to Show
- Milestone visibility
- Critical path
- Dependency timing
- Slips and delays
- Review windows (CP1, CP2, CP3)
- Launch checkpoint

### What to Exclude
- Microtasks and sub-tasks
- Dense legends
- Nested subtasks
- Verbose descriptions
- Day-level precision

## Output Format
Render as a Mermaid gantt with the following constraints:
- `dateFormat YYYY-MM-DD`
- `axisFormat %b %d` (week-level ticks, not daily)
- Section per workstream
- Milestone markers for checkpoints and launch
- Use `crit` only for items on the critical path blocking launch
- No more than 2–3 bars per workstream row

## Color / Status Semantics
- `crit` — critical path, blocking launch
- `active` — in progress, not blocking
- `done` — completed
- default (no tag) — scheduled, not yet started

## Rules
- Do not render individual Jira tasks as bars
- Do not include confidence scores or risk IDs in the chart
- Each bar label must be readable at a glance — max 4 words
- Checkpoints and launch must always be rendered as milestones
- If a workstream has no ETA, render it as NOT STARTED with no end date anchor
- Source all dates from PROGRAM_STATE.yaml — do not estimate or invent
