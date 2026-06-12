# Skill 20 — complexity-estimator

## Purpose
Provide rough complexity estimates for unscoped or unstarted work. Calibrate confidence in delivery timelines based on scope size and team bandwidth.

## Inputs
- Unstarted milestone or workstream description
- PRD/ERD scope signals (number of services, integrations, compliance requirements)
- Team size and known bandwidth constraints
- Comparable historical work (if available in PROGRAM_STATE.yaml)

## Estimation Dimensions
```
SCOPE_SIZE       — number of services touched, new endpoints, data models
INTEGRATION_COMPLEXITY — external partner integrations, new protocols (e.g. NACHA)
COMPLIANCE_OVERHEAD — regulatory requirements adding review cycles
DEPENDENCY_COUNT — number of upstream dependencies before work can start
TEAM_BANDWIDTH   — known constraints (headcount, competing priorities, holidays)
```

## Rough Sizing Output
```
XS — <1 week, single engineer, no external dependencies
S  — 1-2 weeks, 1-2 engineers, minor external dependency
M  — 2-6 weeks, small team, 1-2 external dependencies
L  — 6-12 weeks, full team, multiple external dependencies, compliance review
XL — 3+ months, cross-team, regulatory certification, partner integration
```

## Process
1. Read scope signals from PRD, ERD, and Jira
2. Count integration points, new services, and regulatory requirements
3. Apply sizing heuristics
4. Adjust for known bandwidth constraints
5. Flag if the estimated size conflicts with the remaining timeline

## Outputs
- Rough size estimate (XS/S/M/L/XL)
- Confidence in estimate (HIGH/MEDIUM/LOW based on scope clarity)
- Timeline conflict flag (if size + remaining time = infeasible)
- Assumptions list (what the estimate depends on)

## Rules
- Estimates are rough — do not present as commitments
- Always state assumptions explicitly
- If scope is unclear, estimate the range (S–M) not a single point
- A timeline conflict flag must surface in executive output — never suppress
- Compliance, partner certification, and regulatory review always add L or XL time regardless of eng complexity
