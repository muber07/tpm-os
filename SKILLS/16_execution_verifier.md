# Skill 16 — execution-verifier

## Purpose
Verify reported execution status against actual implementation evidence. Flag fake greens, stalled streams, and claims without evidence.

## Inputs
- PROGRAM_STATE.yaml milestone statuses
- Jira epic and task data (from jira-ingestor)
- GitHub PR and commit activity (from github-ingestor)

## Fake Green Patterns
```
REPLACED_EPIC      — old epic closed, new open epic covers same scope
PREMATURE_CLOSE    — parent epic closed, sub-tasks still open
ALL_TODO           — epic "In Progress" but every sub-task is Open/To Do
STALE_UPDATE       — last Jira update >7 days during active execution phase
SCOPE_REMOVAL      — tasks closed as "no longer needed" without ERD finalization
NO_CODE            — In Progress milestone with no merged PRs or open PRs
NEW_TASK_ADDED     — new task added to an "almost done" epic = scope still evolving
```

## Process
1. For each milestone marked In Progress or Done: pull Jira epic status and sub-task distribution
2. Check GitHub for PRs merged to relevant services since milestone entered In Progress
3. Apply fake green pattern matching
4. For each flag: record the evidence (Jira ticket, last update date, PR count)
5. Adjust milestone confidence based on evidence quality
6. Write verification results to PROGRAM_STATE.yaml `execution_verification`

## Confidence Rules
```
DONE + all sub-tasks closed + PRs merged = HIGH confidence
IN PROGRESS + some sub-tasks In Progress + PRs open = MEDIUM confidence
IN PROGRESS + all sub-tasks Open/To Do = LOW confidence (potential fake green)
IN PROGRESS + no GitHub activity = VERY LOW confidence
DONE + successor epic open = flag, likely fake green
```

## Outputs
- Fake green report with evidence per flag
- Stalled stream list
- Confidence adjustment per milestone
- Updated execution_verification in PROGRAM_STATE.yaml

## Rules
- Do not accept reported status at face value — always check the ticket
- A task closed as "no longer needed" without design finalization = scope still evolving, not complete
- "Updated today" requires checking what changed — a comment update ≠ implementation progress
- Missing GitHub evidence does not confirm nothing is happening — but it lowers confidence
