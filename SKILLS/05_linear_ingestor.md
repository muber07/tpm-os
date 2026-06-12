# Skill 05 — linear-ingestor

## Purpose
Retrieve project tracking data from Linear — initiatives, projects, issues, and status updates.

## Inputs
- Linear initiative URL or name
- Program name for keyword search

## Process
1. Pull the canonical initiative directly (see IDs below — do NOT search, go direct)
2. Pull initiative overview: status, owner, target date, linked projects
3. For each linked project: pull status, milestones, open issues
4. Extract signals: status changes, blocked issues, ownership, recent comments
5. Cross-reference against Jira data — Linear and Jira may track overlapping or complementary scope

---

## US Earner Financing — Canonical Linear IDs (verified 2026-05-28)

### Initiative
- **"US Earner Financing & In Housing"**
  - ID: `8e7d9e9c-86c9-49e2-9959-1301db4ca425`
  - URL: https://linear.app/uber/initiative/us-earner-financing-and-in-housing-36dad9fc0132
  - Owner: milton.lee@uber.com | Status: Active | Target: 2026-10-31
  - Parent: "Financial Product Portfolio" initiative
  - Dashboard: `caeb340f-577a-477e-952b-da5ab81701b4`

### Sub-Initiatives (milestone-aligned)
- M1 - Offer Integration: `5189fea1-e62a-4fea-a703-ae88c637c522`
- M2 - Application: `cfa3512a-6a2c-46c8-ac55-c0c1c42ae7f6`
- M5 - Direct Payments: `05530867-c08e-47e4-9192-46a6d60b9cff`

### Projects Linked to Initiative
| Project | ID | Status |
|---|---|---|
| US EL Open Topics | `3bbbd4ad-3f3a-45ff-a64e-0f1b17cf74f3` | Ready |
| Bancorp - LOC Integration | `41cec325-4f80-4c76-8287-ae8128b83312` | (check live) |
| Importing US Earner work to Linear | `6be5be32-6bdf-4622-ad95-0f6efb42f795` | Ready |
| Financing Management System ↔ Funds Flow | `3d78b51b-b921-4e28-80ea-71aac5c93e9c` | (check live) |
| Financing Product - Data Exchange | `95faba6b-cff0-4959-a9dd-432a12f5989d` | Open |
| Financing Product - Direct Payment | `89f93cfd-9c26-4e13-b7ae-9622caf43afb` | Open |

### Earner Financing Team Projects (team: `16c486e5-a4ad-4946-b86a-f150a4016537`, key: EARNERL)
Active (not trashed) as of 2026-05-28:

| Project | ID | Status | Lead |
|---|---|---|---|
| Financing Product - Entrypoint Experience | `c66d6a17-b30d-4999-93a8-275403a3a2dc` | **In Progress** | Daniele Boscolo |
| Financing Product - Applications | `370c1e2b-a35a-4529-96d3-3ad5a6387b97` | Open | Daniele Boscolo |
| Financing Product - Disbursements | `18085342-2de0-4cc6-8f45-d01e24ed65ee` | Open | Alcides |
| Financing Product - Direct Payment | `89f93cfd-9c26-4e13-b7ae-9622caf43afb` | Open | William Tustumi |
| Financing Product - Data Exchange | `95faba6b-cff0-4959-a9dd-432a12f5989d` | Open | Leao Liu Masur |
| Financing Product - Dashboard | `979eea26-1bf2-46de-aad4-fb164e522b36` | Open | William Tustumi |
| US Earner Lending (own underwriting) | `1b8452a1-5c88-4dde-82da-a71779939f62` | In Progress | milton.lee |
| US EL Open Topics | `3bbbd4ad-3f3a-45ff-a64e-0f1b17cf74f3` | Ready | — |
| PDM onboarding plan | `27ad458d-ead9-4d32-9e48-5e0aabdd6bec` | Open | Matheus Barbieri |
| AI Productivity Initiatives | `dd374ce3-f6db-4857-b1f1-02eb1265641d` | Open | Daniele Boscolo |

### Issue Scope
- EARNERL-* issues: team Earner Financing
- Also check: EPEX-*, FPFOUND-*, MONEYGS-*, MOHDSA-*, ERNRSK-*, DIENG-*, DISPRO-*, DERISK-*

---

## Known Issues / Watch List
- EARNERL-453: Mapper CalculateOfferTerms → repayment info — **In Progress** (Daniele Boscolo, May 28)
- EARNERL-738: Primary Oncall — **In Progress** (May 28)
- Check all issues with status "Triage" or no recent update — these are stale signals

---

## Outputs
- Initiative and project status from Linear
- Open issues and blockers per project
- Stale/unassigned issue flags

## Rules
- Use direct IDs above — do NOT search by name (search returns noise from other Earner teams)
- Always flag when Linear returns no results for a known project — do not silently skip
- Cross-reference Linear project status against Jira epic — they should align; divergence is a contradiction
- Trashed projects (pre-Jira import duplicates) can be skipped — use the active IDs above
- The initiative IS now accessible via MCP (confirmed 2026-05-28 — previous "inaccessibility" note is stale)
