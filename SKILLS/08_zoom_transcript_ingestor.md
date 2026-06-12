# Skill 08 — zoom-transcript-ingestor

## Purpose
Extract decisions, actions, and commitments from Zoom meeting transcripts and recorded meeting notes.

## Inputs
- Zoom transcript text or meeting notes document URL
- Meeting date and attendees

## Process
1. Retrieve transcript or notes content
2. Extract:
   - Decisions made (look for: "we decided", "agreed", "confirmed", "will do", "let's go with")
   - Action items assigned (look for: "can you", "@person", "by [date]", "please")
   - Blockers raised (look for: "blocked", "waiting on", "can't move forward")
   - Risks flagged (look for: "concern", "risk", "what if", "worried about")
   - Questions left unresolved (look for: "TBD", "not sure", "need to figure out", "open question")
3. Attribute each signal to the speaker if transcript includes speaker labels
4. Tag with meeting date and attendees as evidence reference

## Outputs
- Structured signal set by type (decision / action / blocker / risk / question)
- Speaker attribution where available
- Meeting date as evidence timestamp

## Rules
- Meeting decisions are real decisions — do not deprioritize vs. Slack or email
- Unresolved questions from meetings should be tracked until closed
- If attendees include exec stakeholders, flag the meeting signals as high-priority
- Verbal commitments in meetings are real commitments — cross-reference against Jira/Slack to verify they were followed up
