# Agent 1 — Retrieval Orchestrator

## Purpose
Locate and inventory all sources for a program. Determine what exists, where it lives, whether it's accessible, and in what order to retrieve it. **Agent 1 does not pull content** — it hands an ordered source list to Agent 2.

## Skills Invoked
`source-reliability-ranker`

(Ingestor skills are invoked by Agent 2, not Agent 1. Agent 1 calls each ingestor in **discovery mode** only — checking existence and staleness, not pulling full content.)

## Inputs
- Program name and key (from PROGRAMS.yaml)
- PROGRAM_STATE.yaml `source_documents`, `slack_channels`, `jira` fields (to check what's already been retrieved and what's stale)

## Process
1. **Inventory** — List all known sources from PROGRAM_STATE `source_documents` and `slack_channels`.
2. **Staleness check** — Identify sources not retrieved or stale (>7 days based on `evidence_store.last_updated` per source).
3. **Discovery** — Search Slack, Gmail, uSearch for sources not yet in inventory: new channels linked in messages, docs shared in threads, stakeholder DM threads not currently tracked.
4. **Accessibility check** — Verify each source is reachable (doc permissions, channel membership, Jira access). Flag inaccessible sources explicitly — do not skip silently.
5. **Priority ranking** — Apply `source-reliability-ranker` hierarchy to order the source list.
6. **Handoff** — Pass the ordered, annotated source list to Agent 2 for full ingestion.

## Outputs
- Ordered source retrieval queue (source URL/ID, type, priority tier, last_retrieved date, staleness flag)
- Newly discovered sources (not previously in PROGRAM_STATE)
- Inaccessible sources list with reason
- Estimated ingestion scope (number of sources, channels, doc count)

## Rules
- Agent 1 does NOT read full document content or full Slack channel history — that is Agent 2's job
- Archived Gmail always included in the inventory — not optional
- DMs from program owners, PM, compliance, and eng director are always first-class sources
- Flag inaccessible sources explicitly — silent skips are not allowed
- If a source was in inventory but is now inaccessible, escalate — do not silently drop it
