# Skill 03 — archive-email-retriever

## Purpose
Retrieve historical email context predating the active ingestion window. Surface decisions, commitments, and context from earlier program phases.

## Access
**Route through `/google-workspace` skill — never call google-mcp directly.**
Use: `gmail_search` via `/google-workspace` with date range filters for historical threads.

## Inputs
- Program name and key stakeholders
- Historical date range (default: program start date to 90 days ago)
- Specific topics to search (e.g. "Bancorp requirements", "KYC scorecard", "compliance BRD")

## Process
1. Search archived Gmail (via `/google-workspace`) for program-related threads outside the active 7-day window.
2. Prioritize: partner communications, compliance/legal threads, strategic decisions, escalation chains.
3. Extract signals with historical dates — these anchor the program timeline.
4. Cross-reference against PROGRAM_STATE.yaml `historical_timeline` — fill gaps.
5. Flag any archived decision that contradicts current program state.

## When to Invoke
- New program onboarding (always)
- When a contradiction is detected and its origin is unclear
- When a stakeholder references "what was agreed" without a current-source citation
- When the program timeline has gaps

## Outputs
- Historical signal set with dates
- Timeline gap fills
- Contradictions between archived and current signals

## Rules
- Always invoked during program onboarding — not optional
- Archived email often contains original commitments that Slack threads reference — treat as authoritative
- Do not assume recent Slack coverage supersedes archived email
- Never call google-mcp tools directly — always route through `/google-workspace`
