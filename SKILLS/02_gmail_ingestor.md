# Skill 02 — gmail-ingestor

## Purpose
Retrieve and extract operational signals from Gmail — sent, received, and threads involving program stakeholders.

## Access
**Route through `/google-workspace` skill — never call google-mcp directly.**
Use: `gmail_search` via `/google-workspace` to search threads.

## Inputs
- Program stakeholder email list (from PROGRAM_STATE.yaml `owners` and `stakeholders`)
- Last ingestion timestamp

## Process
1. Search Gmail (via `/google-workspace`) for threads involving program owners, PM, compliance lead, eng director, and key partners (Bancorp, Tabapay contacts) since last ingestion.
2. Search for program-name keywords ("earner lending", "in-house", "inhousing") in subject lines and body.
3. Extract signals: decisions, commitments, blockers, compliance requirements, partner communications.
4. Tag each signal with: sender, recipient, date, email subject as evidence reference.
5. Flag emails containing: "approved", "rejected", "committed", "blocked", "concerned", "risk", "deadline", "requirement", "escalate".

## Signal Priority
- Emails from/to partner contacts (Bancorp, Tabapay, Experian) = HIGH — often contain commitments not in Slack
- Emails from Legal/Compliance = HIGH — may contain requirements predating Slack discussions
- Emails from exec stakeholders (GPM, VP) = HIGH — escalations and strategic direction

## Outputs
- Structured signal set tagged by type/source/date/author
- Email subject + sender as evidence reference (no full email bodies in output)

## Rules
- Always search both sent and received — outbound commitments matter
- Partner emails are often not in Slack — do not assume Slack coverage is complete
- If a key decision appears in email only, flag it for cross-referencing against Jira/Slack
- Never call google-mcp tools directly — always route through `/google-workspace`
