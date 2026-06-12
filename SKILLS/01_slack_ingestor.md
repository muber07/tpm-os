# Skill 01 — slack-ingestor

## Purpose
Retrieve and extract operational signals from Slack channels and DM threads relevant to a program.

## Inputs
- List of channel IDs and DM thread IDs from PROGRAM_STATE.yaml `slack_channels` and `dm_group_threads`
- Last ingestion timestamp (for delta retrieval)

## Process
1. For each channel: retrieve messages since last ingestion timestamp (default: last 7 days)
2. For each DM thread: retrieve full thread or delta since last scan
3. Extract signals: decisions, blockers, risks, action items, ownership changes, milestone references
4. Tag each signal with: channel, sender, timestamp, message link
5. Flag messages containing: "blocked", "risk", "slip", "delay", "concern", "escalate", "owner", "who owns", "action item", "by [date]"
6. Surface thread replies — do not read only top-level messages

## Signal Extraction Rules
- Named person + decision verb = DECISION signal
- "blocked by" / "waiting on" / "can't proceed" = BLOCKER signal
- "risk", "concern", "if X then Y" = RISK signal
- "can you", "please", "by [date]", "@person" = ACTION signal
- Date + milestone name = MILESTONE signal

## Outputs
- Structured signal set tagged by type/source/date/author
- Raw message excerpts as evidence strings

## Rules
- Scan all channels listed — do not skip low-traffic channels
- DMs are first-class sources — never deprioritized vs. public channels
- If a channel is inaccessible, flag it — do not silently skip
- Search queries supplement direct reads when channel volume is high
