# Skill 07 — doc-ingestor

## Purpose
Read and extract structured information from Google Docs — PRDs, ERDs, program plans, open questions docs, and decision logs.

## Access
**Route through `/google-workspace` skill — never call google-mcp directly.**
Use: `gdoc_read` via `/google-workspace` to retrieve document content.

## Inputs
- Document URLs from PROGRAM_STATE.yaml `source_documents`
- Document type (PRD / ERD / program plan / open questions / other)

## Process
1. Retrieve document content via `/google-workspace` (gdoc_read).
2. Extract by document type:
   - PRD: goals, launch date, milestones, product decisions, open questions, out of scope
   - ERD: services involved, data flows, design decisions, open questions, dependencies
   - Program plan: milestone map, owner assignments, target dates, risks
   - Open questions doc: unresolved items, owners, last updated
3. Flag internal contradictions within a document (e.g. two different launch dates in the same PRD).
4. Tag all extracted information with document name and section.

## Large Document Handling
- Documents >100KB: use offset/limit reads via `/google-workspace`, prioritize executive summary, goals, milestones, risks, and open questions sections
- Never silently truncate — flag when a document was partially read

## Outputs
- Structured signal set by document type
- Internal contradictions within document
- Key decisions, milestones, dates, and owners
- Open questions list

## Rules
- PRD is the highest-authority source — always read fully before synthesizing
- ERD is the technical source of truth — design decisions in ERD override Slack discussions
- Flag any document that cannot be accessed — do not silently skip
- Internal document contradictions are real signals — always surface them
- Never call google-mcp tools directly — always route through `/google-workspace`
