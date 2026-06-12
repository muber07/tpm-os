# Skill 10 — uplan-ingestor

## Purpose
Retrieve program plan data from uPlan — milestones, owners, target dates, and program health indicators.

## Inputs
- uPlan URL(s) from PROGRAM_STATE.yaml
- Program name for search fallback

## Process
1. Attempt direct retrieval via uPlan URL
2. If direct retrieval fails: search uSearch for uPlan documents matching the program name
3. Extract: milestone map, target dates, owner assignments, health status, open risks
4. Cross-reference uPlan milestones against PROGRAM_STATE.yaml milestone map — flag divergences
5. Check uPlan last-updated date — if >14 days, flag as potentially stale

## Known Limitations
- uPlan URLs are not always indexed in uSearch
- If uPlan is inaccessible: flag as inaccessible, note it as a gap in the truth model, do not assume uPlan is empty
- uPlan milestone dates may not reflect current execution reality — cross-reference against Jira

## Outputs
- uPlan milestone map (if accessible)
- Target date divergences vs. PROGRAM_STATE.yaml
- Inaccessibility flag if uPlan could not be retrieved
- Staleness flag if uPlan not updated recently

## Rules
- Never silently skip uPlan — always flag if inaccessible
- uPlan dates are exec-facing commitments — divergence from Jira reality is a contradiction, not a minor note
- If uPlan shows a different launch date than the PRD, this is a top-level contradiction
