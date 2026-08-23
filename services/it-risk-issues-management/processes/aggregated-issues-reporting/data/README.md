# Sample Data — Aggregated Issues Reporting

**All files here are synthetic**, most recently regenerated on 2026-08-02
to give automation prototyping something realistic to run against — 3x
the original volume, spanning roughly the last 18 months so trend/YoY
reporting has something to compute against. No real finding, issue, audit
result, or person is represented. Field names and value ranges are a
reasonable guess at what the three source systems export — confirm
against the real system exports before building anything that depends on
exact column names.

| File | Represents | Rows |
|---|---|---|
| `findings_extract.csv` | Step 2 pull from the Findings system | 30 |
| `enterprise_issues_extract.csv` | Step 2 pull from the Enterprise Issues system | 24 |
| `internal_audit_extract.csv` | Step 2 pull from the Internal Audit system | 18 |
| `master_file_2026-08-02.csv` | Step 3–4 output: the integrated, dated master file | 72 |

## Schema notes

- `findings_extract.csv`: `Finding_ID, Title, Severity, Status, Date_Identified, Target_Remediation_Date, Closed_Date, Owner, Region, Business, Risk_Category, Analyst, Analyst_Received_Date, Analyst_Decision, Analyst_Decision_Date, Second_Line_Review_Date, Second_Line_Result, Last_Update_Date, Last_Update_Status, Last_Update_Note`
- `enterprise_issues_extract.csv`: `Issue_ID, Title, Linked_Finding_ID, Risk_Rating, Status, Region, Business, Risk_Category, Date_Identified, Target_Remediation_Date, Closed_Date, Owner, Analyst, Analyst_Received_Date, Analyst_Decision, Analyst_Decision_Date, Second_Line_Review_Date, Second_Line_Result, Last_Update_Date, Last_Update_Status, Last_Update_Note` — `Date_Identified` was backfilled onto this extract (see below); it wasn't present in earlier revisions.
- `internal_audit_extract.csv`: `Audit_Finding_ID, Audit_Name, Rating, Status, Due_Date, Closed_Date, Owner, Region, Business, Risk_Category, Last_Update_Date, Last_Update_Status, Last_Update_Note` — no Analyst fields; the analyst-review workflow below applies only to Findings and Enterprise Issues.
- `master_file_2026-08-02.csv`: `Master_ID, Source_System, Source_ID, Title, Severity_Rating, Status, Owner, Region, Business, Risk_Category, Date_Identified, Due_Date, Closed_Date, Last_Update_Date, Last_Update_Status, Last_Update_Note, As_Of_Date` —
  the common schema step 3 (integration) maps all three sources into. The
  Analyst/2LOD fields are intentionally **not** carried into the master
  file, same reasoning as before: they only apply to 2 of the 3 sources,
  so they stay in their own extracts rather than forcing a column that's
  always blank for Internal Audit.
- `Linked_Finding_ID` on some Enterprise Issues rows is intentionally
  blank — not every escalated issue traces back to a single Finding.

## Analyst review workflow (Findings &amp; Enterprise Issues only)

Added to support the Service Performance report. Replaces the earlier,
vaguer `Reviewer`/`Review_Status` fields on Findings — same underlying
concept, made specific enough to actually report on:

- **`Analyst`** — one of 5 people: E. Marsh, O. Adeyemi, V. Kowalczyk,
  F. Nakamura, B. Torres. Roughly evenly assigned (10–11 records each
  across the 54 Findings + Enterprise Issues).
- **`Analyst_Received_Date`** — when the analyst got the item for review.
- **`Analyst_Decision`** — one of `Approved`, `Approved - Sent to 2LOD`,
  or `Rejected - Returned to Owner`; blank if still awaiting decision
  (14 of 54 records — the current queue). Only **High**-severity items
  that the analyst approves get sent to Second Line of Defense; everything
  else terminates at the analyst's decision.
- **`Analyst_Decision_Date`** — when that decision was made. The gap
  between this and `Analyst_Received_Date` is "time to review."
- **`Second_Line_Review_Date`** / **`Second_Line_Result`** (`Pass`/`Fail`)
  — populated only when `Analyst_Decision` is `Approved - Sent to 2LOD`.

**Each analyst was given a distinct performance profile** so the
Service Performance report's trend lines show something real rather than
flat noise: E. Marsh is consistently fast and low-reject; O. Adeyemi and
V. Kowalczyk are steady mid-pack; F. Nakamura starts fast and trends
**worse** over the year (rising review time and 2LOD fail rate); B. Torres
starts slow and trends **better** (a coaching/improvement story). All of
it is invented to demonstrate the reporting pattern — replace with real
performance data once available.

**Pending-queue methodology**: the 14 not-yet-decisioned records were
chosen from the pool of non-`Closed` records (a `Closed` record must have
already cleared analyst review) with an independently-assigned queue age
(5–150 days, weighted toward shorter waits with a deliberate long tail) —
*not* derived from `Date_Identified`, because a real backlog is exactly a
mix of fresh items and neglected old ones, which is what the Aging-by-
analyst table is supposed to surface.

## What's new in this revision

- **3x volume**: 30/24/18 (was 10/8/6), spanning roughly February 2025
  through July 2026, so there's enough history for quarter-over-quarter
  and year-over-year comparisons, not just a single point-in-time snapshot.
- **`Closed_Date`** added to all three extracts and the master file —
  previously only `Status` said "Closed" with no date to trend against.
- **Analyst review workflow** (see above) — replaces Findings' old
  `Reviewer`/`Review_Status` fields and extends the same concept to
  Enterprise Issues.
- **RNV status** (Resolved Not Verified by Internal Audit) added to
  `internal_audit_extract.csv` — the business says it's remediated, but
  Internal Audit hasn't independently confirmed yet. Treated as still-open
  for reporting purposes (not `Closed`) since verification is outstanding.
- **`Last_Update_Status`/`Last_Update_Note`** added to all non-closed
  issues across all three sources: `On Track` or `Off Track`. Off-track
  rows carry a two-sentence explanation; on-track rows leave the note
  blank by design (nothing to explain). Closed issues have no
  last-update fields — there's nothing left to track toward.
- **Overdue coverage**: 9 of the 72 issues are currently past due (as of
  2026-08-02), ranging from 19 to 127 days overdue, spread across both
  Findings/Enterprise Issues and Internal Audit (including one RNV-
  adjacent case). Deliberate, not incidental — the Issue Aging Report
  needs real overdue items to report on.

## Conventions carried over from the prior revision

- `Business` remains a synthetic set (now 4 values: Technology
  Infrastructure, Enterprise, Payments, Retail Banking) and `Risk_Category`
  an 8-value set (Access Management, Data Protection, Network Security,
  Third Party Risk, Patch Management, Application Security, Cloud
  Security, Governance & Documentation) — both invented, both need
  confirming against real taxonomy.
- **"Coming due" convention**: an issue counts toward the 30/60/90-day
  windows only if its status isn't `Closed` and its due date is on or
  after the as-of date (2026-08-02) — already-overdue items are excluded,
  not double-counted. Windows are cumulative (the 90-day count includes
  everything in the 60-day one, which includes the 30-day one).
- **Quarter/YoY convention** (new, used by the Issue Aging Report):
  "past quarter" is a trailing 90-day window, not a calendar quarter;
  "year over year" compares that window to the same 90-day window
  exactly one year earlier. Opened is by `Date_Identified`, closed is by
  `Closed_Date`.
