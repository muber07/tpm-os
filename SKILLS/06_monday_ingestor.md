# Skill 06 — monday-ingestor

## Purpose
Parse Monday.com board exports (.xlsx) uploaded by the TPM, store versioned snapshots, compute deltas vs prior snapshots, and feed signals into PROGRAM_STATE.yaml.

**Note:** Monday.com API is not used. All ingestion is file-based via Excel exports.

---

## Inputs
- `file_path` — absolute path to the uploaded `.xlsx` file
- `snapshot_date` — (optional) override date; defaults to file mtime or today (YYYY-MM-DD)

---

## Board Detection
Inspect sheet names in the workbook:
- Sheet name contains `"loc"` (case-insensitive) → **LOC Board** schema
- Sheet name contains `"policy"` (case-insensitive) → **Policy/Procedure Board** schema
- Neither matches → flag as unknown board, halt ingestion, prompt user to clarify

---

## Parse Logic

### LOC Board — `Uber Earners LOC`
**Main sheet columns to extract:**
| Column | YAML field |
|--------|-----------|
| Item ID (or row identifier) | `id` |
| Name | `name` |
| Status | `status` |
| Task Type | `task_type` |
| Priority | `priority` |
| % Complete | `pct_complete` |
| Proposed Start | `proposed_start` |
| Proposed End | `proposed_end` |
| Responsible Group | `responsible_group` |
| Completion Risk | `completion_risk` |
| Overview | `overview` |

**Updates sheet columns to extract:**
| Column | YAML field |
|--------|-----------|
| Item Name | `item_name` |
| User | `user` |
| Created At | `created_at` |
| Update Content (body) | `content` |

### Policy/Procedure Board — `Uber Earners Policy Procedure Board`
**Main sheet columns to extract:**
| Column | YAML field |
|--------|-----------|
| Item ID | `id` |
| Name | `name` |
| Status | `status` |
| Date Received | `date_received` |
| Date Returned | `date_returned` |
| Date Approved | `date_approved` |
| RID # | `rid_number` |
| Notes/Comments | `notes` |

**Updates sheet columns to extract (same as LOC):**
- Item Name, User, Created At, Update Content

---

## Snapshot Storage

```
/Users/milton.lee/Projects/TPM OS/MONDAY_SNAPSHOTS/
  loc_board/
    YYYY-MM-DD.yaml       ← one snapshot per upload date
    latest.yaml           ← copy of most recent snapshot
  policy_board/
    YYYY-MM-DD.yaml
    latest.yaml
```

### Snapshot YAML format

**LOC Board example:**
```yaml
snapshot:
  board: loc
  uploaded: 2026-05-22
  source_file: Uber_Earners_LOC_2026-05-22.xlsx
  items:
    - id: "12025938853"
      name: "Bancorp to share guidelines on model validation requirements"
      status: "In Process - WithTBBK"
      task_type: "Action"
      priority: null
      pct_complete: null
      proposed_start: null
      proposed_end: null
      responsible_group: null
      completion_risk: null
      overview: null
  updates:
    - item_name: "KYC Vendors"
      user: "Elisabeth Martinmaas"
      created_at: "02/April/2026 05:00:44 AM"
      content: "Uber is currently using Veriff..."
```

**Policy Board example:**
```yaml
snapshot:
  board: policy
  uploaded: 2026-05-22
  source_file: Uber_Earners_Policy_Procedure_Board_2026-05-22.xlsx
  items:
    - id: "12345678901"
      name: "Credit Policy v3"
      status: "Under Review"
      date_received: "2026-04-10"
      date_returned: null
      date_approved: null
      rid_number: "RID-042"
      notes: "Awaiting Bancorp legal sign-off"
  updates:
    - item_name: "Credit Policy v3"
      user: "John Smith"
      created_at: "15/April/2026 09:15:00 AM"
      content: "Sent revised draft to Bancorp on 4/14..."
```

---

## Delta Computation

Compare current snapshot vs `latest.yaml` on:

| Signal | Detection |
|--------|-----------|
| Completed items | Status changed to `Done`, `Closed`, `Complete`, `Approved` |
| Status regressions | Status moved backward (e.g., `Done` → `In Process`) — flag as anomaly |
| New items | Item ID present in current but not in prior |
| Removed items | Item ID present in prior but not in current |
| % Complete increases | `pct_complete` increased — progress signal |
| New updates | Update entry (item_name + created_at) not present in prior snapshot |
| High-risk items | `completion_risk: High` AND status not `Done` |
| Stuck items | Status unchanged from prior AND `completion_risk: High` |

### First run behavior
If no `latest.yaml` exists: output baseline summary only (item count, status distribution). No delta computed.

---

## Output Format

```
--- Monday LOC Board Delta (2026-05-22 vs 2026-05-20) ---

Completed (status → Done/Closed/Approved): [N]
  - "Item name" (was: In Process)

Status changes: [N]
  - "Item name": "Old Status" → "New Status"

New items added: [N]
  - "Item name" (Type: Action | Risk: High)

Removed items: [N]
  - "Item name" (last status: In Process)

Net-new updates from Bancorp/partners: [N]
  - "Item name" — [User] on [Date]: "[excerpt, first 120 chars]..."

High-signal items (Completion Risk = High / status stuck): [N]
  - "Item name" | Status: In Process | Risk: High | Unchanged since prior snapshot

[DRAFT — pending TPM review]
```

---

## PROGRAM_STATE.yaml Update

After successful ingestion, update the following fields:

```yaml
evidence_store:
  last_updated: YYYY-MM-DD          # today's date
  sources_scanned:
    - monday_loc_board               # append if not already present
    - monday_policy_board            # append if relevant
```

Do NOT modify any other PROGRAM_STATE fields from this skill alone. Delta signals should be passed to Agent 03 (context-fusion) and Agent 05 (risk-delta) for full integration.

---

## Rules
- Never infer status from cell color alone — use the text value of the Status column
- If `completion_risk` is not a column in the export, set to `null` (do not guess)
- If a sheet has duplicate Item IDs, flag as data quality issue and deduplicate by taking the last row
- If the snapshot date already exists in the snapshot directory, overwrite and log the replacement
- After saving snapshot, copy to `latest.yaml`
- Every delta claim must cite the specific item ID and prior/current values — no unsupported assertions
- All output is DRAFT until TPM confirms

---

## Invocation

```
ingest monday /path/to/Uber_Earners_LOC_2026-05-22.xlsx
ingest monday /path/to/Uber_Earners_Policy_Procedure_Board_2026-05-22.xlsx
```

Expected outputs:
1. Snapshot YAML saved to `MONDAY_SNAPSHOTS/{board_type}/YYYY-MM-DD.yaml`
2. `latest.yaml` updated
3. Delta summary printed to console
4. `PROGRAM_STATE.yaml` evidence_store fields updated
