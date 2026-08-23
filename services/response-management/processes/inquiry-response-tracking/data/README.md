# Sample Data — Inquiry & Response Tracking

**All files here are synthetic**, generated 2026-08-02 to prototype the
Response Management Service's Overview reporting. No real inquiry,
response, exam, audit, or person is represented. Field names and value
ranges are a reasonable guess at what an inquiry-tracking process would
capture; confirm against the real workflow before building anything that
depends on exact column names.

| File | Represents | Rows |
|---|---|---|
| `audit_inquiries.csv` | Inquiries received from Internal/External Audit | 26 |
| `regulatory_inquiries.csv` | Inquiries received from regulators/examiners | 20 |
| `client_inquiries.csv` | Inquiries received from clients (due diligence) | 24 |
| `audit_responses.csv` | Closed-out response log — Audit channel | 17 |
| `regulatory_responses.csv` | Closed-out response log — Regulatory channel | 12 |
| `client_responses.csv` | Closed-out response log — Client channel | 15 |

70 inquiries total (23 open / 47 closed); 44 responses (one per closed
inquiry).

## Schema

All 6 files share one column set:

`Inquiry_ID (or Response_ID + Inquiry_ID on the Responses files),
Channel, Inquiry_Type, Date_Received, Date_Due, Description, Analyst,
Response_Owner, Domain, Status, Closed_Date, Response_Details,
Exam_Audit_Name, Is_Client_Inquiry, Client_Name`

- **`Channel`** — `Audit`, `Regulatory`, or `Client`. Not asked for
  explicitly but added since it's the natural key the Overview/Audit/
  Regulatory/Client nav tabs filter on, and it's implicit in which file a
  row came from anyway.
- **`Analyst`** — one of 5 people who coordinate inquiries: K. Reyes,
  T. Familusi, H. Lindqvist, S. Park, D. Marchetti. A different role than
  the [IT Risk Issues Management](../../../it-risk-issues-management/)
  Findings analysts — this team only coordinates Response Management
  inquiries.
- **`Response_Owner`** — **not** independently invented. It's looked up
  from each row's `Domain` against
  [docs/reference/domains.md](../../../../../docs/reference/domains.md)'s
  `Domain_Owner` column, so the person on the hook for the underlying
  content is always the same standing SME for that control domain,
  consistent across every inquiry in that domain and across services.
- **`Domain`** — one of the 8 domains in the shared reference file
  (Access Management, Data Protection, Network Security, Third Party
  Risk, Patch Management, Application Security, Cloud Security,
  Governance & Documentation) — same taxonomy IT Risk Issues Management
  uses for `Risk_Category`.
- **`Status`** / **`Closed_Date`** — `Open` rows have no `Closed_Date` or
  `Response_Details` yet. `Closed` rows have both populated.
- **`Response_Details`** — 1–3 sentences, always populated on Closed
  rows, blank on Open ones. Written from one of two templates: a clean
  first-submission close, or a resubmission/clarification close (see
  Rework rate below).
- **`Exam_Audit_Name`** — the audit/exam/engagement the inquiry is tied
  to. For Client rows this is prefixed with the client name (e.g.
  `Acme Bank – Annual DDQ 2026`) since a client due-diligence "engagement"
  is really a client-specific review, not a shared audit/exam name.
- **`Is_Client_Inquiry`** / **`Client_Name`** — `Y`/populated only on
  Client-channel rows; `N`/blank on Audit and Regulatory rows.

## Responses files are the closed-out subset, not a separate population

Each `<channel>_responses.csv` is the matching **closed** subset of that
channel's `<channel>_inquiries.csv` — same `Inquiry_ID`, all the same
field values, plus its own `Response_ID`. Modeled this way (rather than
independently invented rows) because a "response" is the output side of
an inquiry that's already fully described by the inquiry record once
closed — duplicating the row under its own ID keeps the two datasets
literally consistent (an Audit Response can never disagree with its
Audit Inquiry) while still giving the Response side its own file, the
way a real response-repository system would export it separately from
an intake-tracking system.

## Due-date bucketing & RAG convention (used by the Overview report)

Buckets, measured from the as-of date (2026-08-02) against `Date_Due`,
for **Open** rows only:

| Bucket | Definition | RAG |
|---|---|---|
| Overdue | Due date before as-of date | Red |
| Due ≤24h | Due date = as-of date | Red |
| Due ≤48h | Due date = as-of date + 1 | Amber |
| Due ≤5 business days | Within 5 business days beyond the 48h bucket | Amber |
| Due ≤20 business days | Within 20 business days beyond the 5-day bucket | Green |
| Beyond 20 business days | Everything else | Green |

Open records' due dates were **hand-placed** (not purely random) to
guarantee every bucket has at least one record in every channel — with
only 8–9 open records per channel, pure random placement risked leaving
a bucket empty and making the aging table look broken rather than just
sparse.

## Rework-rate convention

19% of closed inquiries are modeled as requiring resubmission/
clarification (closing after the original due date, with a
resubmission-flavored `Response_Details` sentence) — matching the
`Red`-rated "Responses requiring resubmission/clarification: 19%" KPI
already published for this Service in
[docs/service-catalog.md](../../../../../docs/service-catalog.md).
Non-reworked closures land on or slightly before the due date.

## Conventions carried over from IT Risk Issues Management's data

- Deterministic generation via seeded Python `random` (`random.seed(7)`)
  for reproducibility.
- `Date_Received` spans roughly the last 12 months (mid-August 2025
  through late July 2026) so there's enough history for a future
  Service Performance trend report, the same reasoning behind IT Risk
  Issues Management's 18-month data window.
