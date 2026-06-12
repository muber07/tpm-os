# Skill 26 — newsletter-generator

## Purpose
Generate an executive-level program narrative newsletter from PROGRAM_STATE.yaml. The goal is NOT to summarize meetings or activities. The goal is to surface what changed, why it matters, what leaders should worry about, and what needs to happen next.

## Core Principle
Executives care about: **Progress, Changes, Risks, Decisions, Impact.**

Every output must answer:
- "Are we on track?"
- "What changed?"
- "Why should I care?"
- "What do we need to do?"

## Inputs
- PROGRAM_STATE.yaml (milestones, risks, dependencies, owners)
- Delta report from Slack DMs and conversations only (for "What Changed" section — never from Jira dependency tickets)
- Current date

## Output
- `/Users/milton.lee/Projects/newsletter.html` — overwrite on each run (canonical rendered artifact)
- `/Users/milton.lee/Projects/TPM OS/NEWSLETTERS/YYYY-MM-DD_newsletter.md` — markdown snapshot per generation (permanent record)
- TerraBlob: `https://personal.uberinternal.com/milton.lee/us-earner-lending-newsletter-YYYY-MM-DD.html`
- Marked DRAFT until TPM review clears it

## Design Reference
`newsletter.html` is the canonical template. CSS variables, strip widths, pill classes, and layout structure are defined there. Do not deviate from the template without explicit instruction.

---

## Required Thinking Process

Before writing any narrative, execute these four steps:

### Step 1: Identify Progress
Extract only meaningful accomplishments. Ignore: meeting logistics, status reporting, coordination activities. Keep: decisions made, deliverables completed, dependencies resolved.

### Step 2: Identify What Changed
Ask: "What is different this week compared to last week?"
Examples: scope increased, ownership changed, new dependency identified, requirement clarified, timeline shifted. This is often the most important section.

### Step 3: Identify Risk & Impact
For every issue found, ask: "So what?"

BAD: "Waiting for Bancorp templates"
GOOD: "Compliance scope remains undefined, creating launch timeline risk"

BAD: "Need policy review"
GOOD: "Compliance has become the critical path to launch"

Convert activities into business impact.

### Step 4: Identify Required Actions
Only include actions that materially reduce risk. Avoid task lists. Focus on: decisions, escalations, dependency closures, resource asks.

---

## Output Format

### Headline
Format: `[Outcome] due to [driver]`

Examples:
- "Launch timeline risk is increasing due to unresolved compliance scope"
- "Program execution remains on track; risk concentrated in external dependencies"
- "Readiness improved following repayment model alignment"

The headline should contain the entire story.

---

### Section 1: What's Going Well
- Progress, wins, resolved issues
- Maximum 4 bullets

### Section 2: What Changed *(most important section)*
- New information, scope changes, ownership changes, new dependencies
- Source: Slack DMs and conversations only — never Jira dependency tickets
- Maximum 4 bullets

### Section 3: Why It Matters
Convert observations into consequences.

BAD: "Waiting on compliance review"
GOOD: "Compliance is now the primary critical path"

BAD: "Repayment model undecided"
GOOD: "Delayed decisions may create downstream rework"

Maximum 4 bullets.

### Section 4: What Needs To Happen Next
- Decisions, escalations, critical path items only
- Avoid operational tasks
- Maximum 4 bullets

### Executive Takeaway
One sentence. The distilled conclusion for a VP or GM reading only the bottom of the page.

---

## Milestone Display Rules (within newsletter HTML)
- Workstream order is fixed: **M1 → M2 → M5 → M3 → M6 → M4 → M7/8/9** — do NOT sort by RAG status
- Status strip colors: RED = #E53E3E, AMBER = #D69E2E, GREEN = #38A169, NOT_STARTED = #718096
- **At Risk or Blocked**: status pill + reason note only. No owners.
- **In Execution or Not Started**: status pill only. No detail.
- Notes must cite a source (Jira ticket or Slack ref). No source → flag [UNVERIFIED].

## Dependency Table Rules
- All DEPENDREQ ticket numbers must be clickable: `https://t3.uberinternal.com/browse/DEPENDREQ-XXXXX`
- P0 dependencies appear first
- Stalled deps highlighted; closed deps dimmed (opacity 0.5) at bottom
- No ETA and not closed → flag [NO ETA]

## Risk Section Rules
- RED risks only
- Each risk: description, owner, mitigation status
- Each risk must include a **"Path to Green"** callout (green left-border box)
- Unowned risks flagged: [UNOWNED]

## Footer Rules
- Always include DRAFT watermark
- Do NOT include a "Distributed to:" line

---

## Executive Writing Rules

1. Never describe meetings
2. Never repeat action items
3. Never write chronological summaries
4. Always explain impact
5. Convert activities into outcomes

BAD: "The team met with Bancorp."
GOOD: "Bancorp clarified Uber will own policy drafting."

BAD: "Reviewing repayment options."
GOOD: "Repayment structure remains unresolved and is blocking downstream decisions."

---

## Trigger Phrases
- "generate newsletter"
- "generate weekly newsletter"
- "partner newsletter"

## Rules
- DRAFT watermark always present — never remove before TPM review
- Never omit stalled dependencies or unverified risks to make the newsletter look cleaner
- Output path is always `/Users/milton.lee/Projects/newsletter.html`
- Every run overwrites the previous file — no versioning at file level
