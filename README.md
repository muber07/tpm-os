# TPM OS — AI-Powered TPM Execution Intelligence

> Autonomous signal gathering · Evidence-based state updates · Human approval gate · Instant outputs

Built by Milton Lee, FinProd/Lending TPM at Uber. TPM OS turns fragmented program signals into a single verified truth model — and challenges every proposed update before anything commits.

---

## What it does

Running a complex program means synthesizing dozens of signals every day across Jira, Slack, Google Docs, and meeting notes. TPM OS automates the gathering and synthesis, then stops for your decision before writing anything.

**The pipeline in one sentence:** Fetch signals → Synthesize a state delta → Detect contradictions → Devil's Advocate challenges every claim → You approve → Verify against live sources → Generate outputs → Publish.

The only hard stop is **you**. The pipeline never auto-advances past the human approval gate.

---

## The 8-Stage Pipeline

| Stage | Agent | Mode | What it does |
|-------|-------|------|--------------|
| 1 | **SignalFetcher** | Automatic | Pulls Jira, Slack, Google Docs. Writes `signals.md` with timestamped citations. Never synthesizes. |
| 2 | **Synthesizer** | Automatic | Proposes a state delta. Scores confidence per field: HIGH (2+ sources), MEDIUM (1 source), LOW (inferred). |
| 2.5 | **Contradiction Detector** | Automatic | Finds conflicts between sources. Every contradiction cites both sources with ticket IDs or timestamps. |
| 3 | **Devil's Advocate** | Automatic | Adversarially challenges the delta across 6 categories. Every challenge must cite a ticket ID or Slack timestamp — vague concerns are rejected. Emits APPROVED or REJECTED. |
| 3.5 | **You Approve** | Your decision | See proposed delta, DA findings, contradictions, and confidence scores on one screen. Reply `approved` or correct and loop back. |
| 4 | **Verifier** | Automatic | Pulls live Jira and Slack post-approval to confirm committed values. Stamps every field with `verified_at`. |
| 5 | **Renderer** | Automatic | Generates newsletter, exec brief, risk tracker, or escalation memo from verified state. Every claim is sourced. |
| 6 | **Publisher** | Automatic | Publishes outputs and notifies stakeholder Slack channels. Archives under `programs/[name]/OUTPUTS/`. |

### Devil's Advocate — 6 Challenge Categories

The DA must provide evidence for every challenge or the challenge is invalid:

- **Staleness** — field hasn't been verified recently
- **Contradictions** — two sources disagree on the same fact
- **Ownership gaps** — milestone has no named owner
- **Unverified commitments** — verbal agreement not in Jira
- **Optimism bias** — date assumes no slip in any dependency
- **Missing signal** — a key stakeholder or workstream has no recent signal

A **REJECTED** delta loops back to the Synthesizer with `da_challenges.md`. Loops until DA approves or you override.

---

## Quick Start

### 1. Create a program directory

```bash
mkdir -p programs/my-program
cp PROGRAM_STATE_template.yaml programs/my-program/PROGRAM_STATE.yaml
```

Then create `programs/my-program/config.yaml`:

```yaml
program_name: "My Program"
jira_project: MYPROJ
jira_initiative: MYPROJ-001
slack_channels:
  - "#my-program-eng"
  - "#my-program-pm"
slack_dms:
  - alice@company.com
  - bob@company.com
stakeholders:
  - name: Alice (PM Lead)
  - name: Bob (Eng Lead)
```

### 2. Run the pipeline

```bash
# Full end-to-end pipeline run
run tpm-os program=my-program

# Individual stages
run tpm-os stage=signal-fetch program=my-program
run tpm-os stage=synthesize program=my-program
```

### 3. Approve

```
# To commit the proposed state
approved

# To correct and loop back
The M3 launch date is Sep 30, not Oct 15. Loop back to Synthesizer.
```

---

## Common Commands

| Command | What happens |
|---------|-------------|
| `run tpm-os program=<name>` | Full pipeline: fetch → synthesize → detect → DA → pause → verify → render → publish |
| `generate newsletter program=<name>` | Generate weekly HTML stakeholder newsletter from verified state |
| `generate exec-brief program=<name>` | One-page executive summary: status, risks, decisions needed |
| `generate escalation program=<name>` | Escalation memo with context, ask, and path to green |
| `generate risk-tracker program=<name>` | Risk table with owner, likelihood, impact, mitigation |
| `run verify program=<name>` | Live Jira + Slack cross-check. Fields not verified in 7 days → STALE |

---

## Directory Structure

```
TPM OS/
├── SYSTEM_PROMPT.md              # Core agent identity and principles
├── CLAUDE.md                     # Claude Code operating instructions
├── EXECUTION_FLOW.md             # Pipeline map
├── TAXONOMY.md                   # Field definitions and terminology
├── PROGRAM_STATE_template.yaml   # Blank template — copy per program
│
├── programs/
│   └── my-program/
│       ├── config.yaml           # Jira project, Slack channels, stakeholders
│       ├── PROGRAM_STATE.yaml    # Single source of truth (gitignored — contains live data)
│       ├── signals.md            # Raw gathered signals (auto-generated)
│       ├── proposed_delta.yaml   # Synthesizer output (pre-approval)
│       ├── contradictions.md     # Conflict surface report
│       ├── da_challenges.md      # Devil's Advocate findings
│       └── OUTPUTS/
│           └── 2026-06-11_newsletter.html
│
├── AGENTS/                       # 8 reasoning engine definitions
│   ├── 01_retrieval_orchestrator.md
│   ├── 02_ingestion_agent.md
│   ├── 03_context_fusion_agent.md
│   ├── 04_execution_verification_agent.md
│   ├── 05_risk_delta_agent.md
│   ├── 06_planning_agent.md
│   ├── 07_executive_synthesis_agent.md
│   └── 08_governance_agent.md
│
└── SKILLS/                       # 26+ modular capabilities
    ├── 01_slack_ingestor.md
    ├── 04_jira_ingestor.md
    ├── 12_contradiction_detector.md
    ├── 16_execution_verifier.md
    ├── 26_newsletter_generator.md
    └── ...
```

---

## Skills Library

### Ingestors
Pull signals from external sources.

`jira-fetch` · `slack-fetch` · `gdoc-fetch` · `meeting-notes-fetch` · `dependency-fetch` · `ticket-status-fetch` · `milestone-fetch` · `epic-fetch`

### Intelligence
Synthesize, analyze, and challenge.

`synthesize-delta` · `detect-contradictions` · `score-confidence` · `identify-risks` · `flag-staleness` · `track-decisions` · `map-dependencies` · `verify-commitments`

### Renderers
Generate outputs from verified state.

`newsletter-generator` · `exec-brief` · `risk-tracker` · `escalation-memo` · `decision-log` · `dependency-report` · `timeline-builder` · `stakeholder-update`

---

## Confidence Scoring

Every field in `PROGRAM_STATE.yaml` carries a confidence score:

| Score | Criteria |
|-------|----------|
| **HIGH** | 2+ independent sources agree |
| **MEDIUM** | 1 source confirms |
| **LOW** | Inferred — no direct source |

And a `verified_at` timestamp. Fields not cross-checked in 7 days are automatically flagged as **STALE** on the next pipeline run.

---

## Design Principles

1. **Truth over optics** — every field traces back to a source with a date. No confidently wrong status updates.
2. **Evidence-required** — the DA cannot raise a vague concern. Every challenge cites a specific ticket ID, Slack timestamp, or date gap.
3. **Human in the loop** — the pipeline never auto-advances past the approval gate. Full context, one screen.
4. **Multi-program by design** — one `config.yaml` per program. Skills are program-agnostic.
5. **PRD-first** — program state is built from primary sources, not memory.

---

## Visual Overview

**[`tpm_os_overview.html`](tpm_os_overview.html)** — Interactive 4-tab dashboard with animated orbital pipeline diagram, clickable agent nodes, phase accordion, Get Started guide, and Skills library.

**[`tpm_os_skills_diagram.html`](tpm_os_skills_diagram.html)** — Dark neon skills map.

---

## What's gitignored

`PROGRAM_STATE.yaml` and all program output files are excluded from this repo — they contain live program data. Only the blank template (`PROGRAM_STATE_template.yaml`) is tracked.

---

Built with [Claude Code](https://claude.ai/code).
