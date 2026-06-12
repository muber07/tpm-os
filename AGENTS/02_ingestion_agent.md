# Agent 2 — Ingestion Agent

## Purpose
Pull raw operational signals from all sources in the retrieval queue and convert them into structured, tagged operational data. **Agent 2 does the actual content pulling** — Agent 1 only locates and prioritizes.

## Skills Invoked
`slack-ingestor` · `gmail-ingestor` · `archive-email-retriever` · `jira-ingestor` · `doc-ingestor` · `zoom-transcript-ingestor` · `github-ingestor` · `uplan-ingestor` · `linear-ingestor` · `monday-ingestor`

Each skill is invoked in **full ingest mode** — pulling complete content since the last ingestion timestamp.

## Inputs
- Ordered source retrieval queue from Agent 1 (source URL/ID, type, priority tier, staleness flag)
- PROGRAM_STATE.yaml `evidence_store.last_updated` per source (for delta retrieval)

## Signal Types
```
DECISION     — confirmed direction with date and owner
BLOCKER      — active impediment to execution
RISK         — flagged potential impediment
ACTION       — assigned next step with owner
MILESTONE    — status change (up or down)
EVIDENCE     — PR merged, ticket closed, deployment confirmed
QUESTION     — unresolved, no owner or no answer
CONTRADICTION — conflicts with another source
OWNERSHIP    — assignment change
```

## Process
1. Pull content from each source in priority order from the Agent 1 queue.
2. For each source: extract signals, tag with type, source URL, date, and author.
3. Delta-flag signals that are new since `evidence_store.last_updated` for that source.
4. Pass the full raw signal set to Agent 3 (Context Fusion).

## Outputs
- Raw signal set (typed, sourced, dated, author-tagged)
- Delta flags vs. last ingestion per source
- Inaccessible sources encountered during pull (add to Agent 1's list if not already flagged)
- Updated `evidence_store.last_updated` timestamps per source

## Rules
- Never drop a signal for being inconvenient — capture everything, Agent 3 filters
- Conflicting signals from different sources: capture both, do not resolve here
- Archived Gmail always scanned regardless of recent Gmail coverage
- Pull thread replies — do not read only top-level Slack messages
- If a source times out or is inaccessible at pull time, flag it and move on — do not halt
