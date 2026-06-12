# Skill 09 — github-ingestor

## Purpose
Retrieve implementation evidence from GitHub — PRs merged, commits, deployment activity, and code review status for program-relevant services.

## Inputs
- Service list from PROGRAM_STATE.yaml (new services being built + modified services)
- Date range (default: last 7 days)

## Process
1. For each service: search recent commits and PRs
2. Classify PRs and commits by workstream (e.g. disbursements, identity, FMS, UUE) based on service name and PR description
3. Extract:
   - PRs merged (evidence of implementation progress)
   - PRs open >7 days (potential stalls)
   - First commit to a new service (evidence that implementation has started)
   - Test coverage PRs
   - Deployment-related commits
4. Cross-reference against Jira "In Progress" tasks — if a task is In Progress but no PR exists, flag it
5. Flag services with no recent commit activity during active execution phase

## Services to Monitor (US Earner Lending)
New: `fp-underwriting`, `fp-financing-management-core`, `fp-financing-management-ledger`
Modified: `finprod-lending`, `financing`, `web-financing`, `earner-payments-presentation`, `earner-arrears`

## Outputs
- PR activity summary per service per week
- First-commit evidence for new services (confirms implementation started)
- Stalled PR list (open >7 days)
- Services with no activity (potential fake green flag)
- Cross-reference gaps (Jira says In Progress, GitHub shows nothing)

## Rules
- No PR merged = no confirmed implementation, regardless of reported status
- A new service with no commits is pre-implementation — flag it
- Stale PRs in review are a signal — they may indicate review bottlenecks
