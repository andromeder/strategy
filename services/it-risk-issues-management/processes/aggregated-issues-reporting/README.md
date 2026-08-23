---
name: "Aggregated Issues Reporting"
function: "Regulatory, Audit & Issues Management"
subteam: ""              # tbd — doesn't cleanly map to F5.1–F5.4; confirm actual owning subteam
region: "Global"
owner: ""                # tbd
trigger: "Date (scheduled)"
frequency: ""            # tbd — commonly monthly for this kind of roll-up; confirm actual cadence
status: in-progress
automation_potential: "" # leave blank per docs/schema.md until pain points/time cost are confirmed
---

# Aggregated Issues Reporting

## Description

Produces the recurring aggregated issues report for
[IT Risk Issues Management Service](../../README.md) by pulling data from
the three core issue-tracking systems — Findings, Enterprise Issues, and
Internal Audit — consolidating it into a single master dataset, archiving
that dataset for historical/trend purposes, and distributing a report to
stakeholders.

## Trigger & Frequency

Date-driven (scheduled), not event-driven — it runs on a calendar, not in
response to a specific finding or issue. Exact cadence not yet confirmed;
reports like this are commonly monthly. Confirm actual frequency before
treating anything downstream (e.g. time-cost estimates) as reliable.

## Inputs

- Findings extract — Core Issues System (Findings)
- Enterprise Issues extract — Core Issues System (Enterprise Issues)
- Internal Audit extract — Core Issues System (Internal Audit)

## Outputs

- Aggregated Issues Report — distributed to stakeholders
- Master file — the integrated dataset, archived for historical purposes

## Systems / Tools

- Core Issues System — Findings module
- Core Issues System — Enterprise Issues module
- Core Issues System — Internal Audit module
- Integration/report-authoring tool — not yet specified; assume manual
  (e.g. spreadsheet) until confirmed otherwise

## Process Steps

1. Trigger: scheduled date
2. Retrieve data from the Core Issues Systems — Findings, Enterprise
   Issues, Internal Audit
3. Integrate the three extracts into a single master file
4. Save the master file for historical purposes
5. Create the report
6. Distribute the report

## Manual Steps

Working assumption is that all six steps above are currently performed
manually — that assumption is the whole reason this process was picked to
deepen first. Confirm which steps (if any) already have partial tooling.

## Time Cost

Not yet measured. This is the first thing to capture from the actual team
before rating automation potential with any confidence.

## Dependencies

- Draws on the Findings and Enterprise Issues data that the rest of IT
  Risk Issues Management Service manages.
- Output (IT Risk Reports) is consumed by Governance & Leadership and
  Security Leadership per the Consumer × Product map in
  [docs/reference-architecture.md](../../../../docs/reference-architecture.md).

## Matrix Relationships

Pulls from three systems that likely sit with different owners — Findings
and Enterprise Issues within this Service, Internal Audit likely owned
externally to Global IT Risk entirely (Internal Audit is a Consumer group,
not a function in the org chart). Worth confirming whether pulling
Internal Audit data requires a formal data-sharing agreement rather than
a normal system integration.

## Frameworks

Not yet mapped. See [docs/industry-frameworks.md](../../../../docs/industry-frameworks.md)
for candidate frameworks referenced by F5 generally (FFIEC IT Examination
Handbook, IIA IPPF).

## Pain Points

Draft, inferred from the shape of the process rather than confirmed with
the team — validate before acting on these:

- Manual pulls from three disparate systems, each likely with its own
  export format — time-consuming and a source of transcription errors.
- Manual integration/reconciliation into one master file, with no
  guarantee the three systems use consistent IDs, categories, or severity
  scales.
- Manual archiving with no described versioning/naming convention —
  a real risk of an overwritten or lost historical file.
- Manual report creation and distribution — a lag between data pull and
  report delivery, and no guarantee of a consistent format cycle to cycle.

## Automation Potential

Preliminary read: **High**, pending confirmation of time cost and pain
points above. Every step — retrieval, integration, archiving, report
generation, distribution — is a mechanical, repeatable operation with a
clear input and output at each stage, which is exactly why it was chosen
as the first process to deepen ahead of the more judgment-heavy parts of
this Service (triage, remediation plan review).

## Sample data

See [data/](data/) for synthetic sample extracts (Findings, Enterprise
Issues, Internal Audit) and a synthetic master file, shaped to match the
inputs/outputs above — generated to give automation prototyping something
concrete to run against before real system access is wired up.
