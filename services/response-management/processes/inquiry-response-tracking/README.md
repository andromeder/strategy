---
name: "Inquiry & Response Tracking"
function: "Regulatory, Audit & Issues Management"
subteam: ""              # spans F5.2 Audit & Reg Prep / F5.3 Reg Response / F5.4 Client Due Diligence
region: "Global"
owner: ""                # tbd
trigger: "Event (inquiry received)"
frequency: ""            # tbd — continuous intake, not a scheduled cycle
status: in-progress
automation_potential: "" # leave blank per docs/schema.md until pain points/time cost are confirmed
---

# Inquiry & Response Tracking

## Description

Tracks every inquiry the [Response Management Service](../../README.md)
receives across its three channels — Audit (Internal and External),
Regulatory (examiner), and Client (due-diligence) — from receipt through
to a closed, submitted response. An analyst coordinates each inquiry; the
actual response content comes from a separate response owner (typically
the control/domain owner), which is why timeliness has two people to
manage, not one.

## Trigger & Frequency

Event-driven: a new inquiry received from Internal Audit, an external
auditor, a regulator/examiner, or a client due-diligence request. Not a
scheduled cycle — inquiries arrive continuously and each one starts its
own due-date clock.

## Inputs

- Audit inquiries (Internal and External)
- Regulatory (examiner) inquiries
- Client due-diligence inquiries

## Outputs

- Audit responses
- Regulatory responses
- Client responses
- Overview reporting: open-inquiry volume by exam/audit and by domain,
  and a due-date aging view for SLA management

## Systems / Tools

Not yet specified; assume manual (email/spreadsheet) intake and tracking
until confirmed otherwise — same working assumption used for Aggregated
Issues Reporting before it was deepened.

## Process Steps

1. Trigger: inquiry received (Audit, Regulatory, or Client channel)
2. Analyst logs the inquiry — type, due date, description, domain, and
   which exam/audit/client engagement it belongs to
3. Analyst routes the underlying request to the response owner (the
   control/domain owner accountable for that content)
4. Response owner supplies the substantive response; analyst assembles
   and submits it against the due date
5. If returned for clarification/resubmission, repeat step 4 before the
   item can close
6. Inquiry closes; response details and closed date are logged

## Manual Steps

Working assumption: all six steps are currently manual — intake logging,
routing to the response owner, and resubmission handling in particular
are where a missed handoff would show up first.

## Time Cost

Not yet measured.

## Dependencies

- Response owners are the same control/domain owners recorded in
  [docs/reference/domains.md](../../../../docs/reference/domains.md) —
  routing depends on that lookup staying current.
- Feeds the Service's Overview reporting (open volume, aging/SLA risk)
  the same way Aggregated Issues Reporting feeds IT Risk Issues
  Management's reporting.

## Matrix Relationships

Spans three F5 subteams by channel — F5.2 Audit & Reg Prep, F5.3 Reg
Response, F5.4 Client Due Diligence — coordinated as one tracking process
rather than three separate ones, which is the whole reason this Service
exists as a single point of coordination instead of three disconnected
teams each running their own log.

## Frameworks

Not yet mapped. See [docs/industry-frameworks.md](../../../../docs/industry-frameworks.md).

## Pain Points

Draft, inferred rather than confirmed with the team:

- Manual intake across three channels likely means three different entry
  habits (or none) before anything is centrally logged.
- The analyst/response-owner handoff is an easy place for a due date to
  slip silently — nothing here currently flags it.
- Resubmission/clarification cycles (the `Red` KPI in the service
  catalog) aren't visibly tracked as their own event, just inferred from
  a late close date.

## Automation Potential

Preliminary read: **High** for the tracking/aggregation layer (intake
logging, due-date bucketing, routing) — same shape as Aggregated Issues
Reporting. Lower confidence on the response-drafting step itself, which
is judgment-heavy.

## Sample data

See [data/](data/) for six synthetic extracts — Audit/Regulatory/Client
Inquiries and their matching Audit/Regulatory/Client Responses — shaped
to prototype the Overview reporting above before real system access is
wired up.
