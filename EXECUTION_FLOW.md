# TPM OS Execution Flow

## Full Pipeline

```
AGENT 1 — Retrieval Orchestrator
    ↓ locates and prioritizes all sources for the program
AGENT 2 — Ingestion Agent
    ↓ pulls raw signals from all located sources
AGENT 3 — Context Fusion Agent
    ↓ reconciles contradictions, builds unified truth, updates PROGRAM_STATE.yaml
AGENT 4 — Execution Verification Agent
    ↓ compares reported status vs. implementation evidence, flags fake greens
AGENT 5 — Risk & Delta Agent
    ↓ updates risk registry, tracks what changed since last run, maps dependencies
AGENT 6 — Planning Agent
    ↓ converts truth into next actions, sequences dependencies, maps owners
AGENT 7 — Executive Synthesis Agent
    ↓ generates Slack update, one-slider, status board, or Gantt
AGENT 8 — Governance Agent
    ↓ validates evidence, scores confidence, flags unresolved uncertainty
        ↓
    TPM REVIEW & APPROVAL
```

---

## Entry Points

Not every run starts at Agent 1. Common entry points:

| Trigger | Start At | End At |
|---|---|---|
| New program onboarding | Agent 1 | Agent 8 |
| Weekly status update | Agent 2 | Agent 8 |
| Dependency check | Agent 4 | Agent 5 |
| Quick risk pulse | Agent 5 | Agent 7 |
| Executive output only | Agent 7 | Agent 8 |
| Truth verification | Agent 3 | Agent 4 |

---

## State Flow

Each agent reads `PROGRAM_STATE.yaml` and writes updates back to it.

```
PROGRAM_STATE.yaml
    ← written by: Context Fusion, Execution Verification, Risk & Delta, Planning
    → read by: all agents
```

---

## Operating Rules

- Archived Gmail is always checked, not optional
- Source priority: PRD > ERD > uPlan > Jira > GitHub > Slack/Gmail > meeting notes
- No output is published without Governance Agent review
- Human approval is final — the AI prepares, the TPM governs
- Delta-first: every run compares against previous PROGRAM_STATE.yaml
