# TPM OS

An autonomous TPM Execution Intelligence System for managing complex multi-team
programs at Uber. Built by Milton Lee (FinProd/Lending TPM).

## What it does

TPM OS turns fragmented signals — Jira tickets, Slack threads, Google Docs,
dependency requests — into a single live truth model. It surfaces what changed,
what's blocked, and what's at risk, before you have to ask.

## Core Principle

**Truth over optics.** Every field in `PROGRAM_STATE.yaml` has a `last_verified`
date. If it's stale, the system flags it. No confidently wrong status updates.

## Visual Overview

**[`tpm_os_overview.html`](tpm_os_overview.html)** — Interactive 4-tab dashboard:
- **Overview** — 8 benefit cards + interactive orbital pipeline diagram with animated flow dots and clickable agent nodes
- **Pipeline** — Phase-by-phase accordion with reads/writes per stage
- **Get Started** — Setup guide, directory structure, common commands
- **Skills** — Full skills library organized by category (Ingestors · Intelligence · Renderers)

**[`tpm_os_skills_diagram.html`](tpm_os_skills_diagram.html)** — Dark neon skills map

## Architecture

| File | Purpose |
|------|---------|
| `CLAUDE.md` | Operating instructions for Claude Code |
| `SYSTEM_PROMPT.md` | Core reasoning principles |
| `EXECUTION_FLOW.md` | Pipeline: Retrieval → Fusion → Verification → Output |
| `AGENTS/` | 8 reasoning engines (onboarder, ingestor, verifier, risk radar, etc.) |
| `SKILLS/` | 26+ modular capabilities (Jira ingestor, delta analyzer, newsletter generator, etc.) |

## Programs Loaded

- **US Earner Lending (In-House)** — Uber's in-house consumer lending program

## Key Commands

| Command | What happens |
|---------|-------------|
| `run weekly update` | Pull live Jira + Slack, diff against last state, generate highlights |
| `execution verification` | Check every DEPENDREQ against live Jira, flag stale fields |
| `risk pulse` | Refresh risk registry with current evidence |
| `generate status board` | Draft exec-ready status output |
| `generate one-slider` | Draft single-page executive summary |

## Ground Truth Rule

Never write to PROGRAM_STATE from memory alone. Every status update is pulled
live from Jira before being written. Every field tracks when it was last verified.

---

Built with [Claude Code](https://claude.ai/code).
