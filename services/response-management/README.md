# Response Management (Audit - Internal and External, Regulatory, Client) Service

Reference-architecture Service (Regulatory & Issues group), renamed from
"Regulatory Response Management Service" — broadened from a
regulatory-only scope to cover all three inquiry channels the team
actually coordinates against: **Audit** (Internal and External), **Regulatory**
(examiner), and **Client** (due-diligence). Candidate owner: **F5.3**. Full
description, RAG status, KPIs, and finances live in
[docs/service-catalog.md](../../docs/service-catalog.md) and the
interactive version in the Process tab of the operating-model artifact —
this folder is where the deeper, automation-focused work happens.

## Description

The single point of coordination for inquiries received from three
directions: **Internal/External Audit** requests (evidence, walkthroughs,
sample testing), **Regulatory** examiner requests (document/interview
requests tied to a specific exam), and **Client** due-diligence requests
(security questionnaires, onboarding/annual reviews). Each inquiry is
coordinated by an analyst but answered by a **response owner** — the
subject-matter/control owner who actually supplies the underlying
content — a distinction that matters because analyst timeliness and
response-owner timeliness are two different things to manage.

A nice structural fit worth noting: the three inquiry channels map
cleanly onto three of F5's existing subteams —
[docs/org-structure.md](../../docs/org-structure.md) already lists
**F5.2 Audit & Reg Prep**, **F5.3 Reg Response**, and **F5.4 Client Due
Diligence** as separate subteams under Regulatory, Audit & Issues
Management, which is exactly the Audit / Regulatory / Client split this
Service's overview page is organized around.

## Overview page

Same pattern as [IT Risk Issues Management](../it-risk-issues-management/README.md):
a fixed nav across the top — **Overview, Audit, Regulatory, Client,
Service Performance** — with content changing based on what's selected.
Same published artifact family, see
[artifacts/README.md](../../artifacts/README.md) for the live link.

- **Overview** — cross-channel inquiry tracking: filters (Inquiry Type,
  Client, Exam/Audit Name), open-reviews-by-inquiry-volume and
  inquiries-by-domain (both stacked open/closed), and a due-date aging
  table (24h / 48h / 5 business days / 20 business days / beyond) by
  exam or audit with a drill-down to individual inquiries and a RAG chip
  per timeframe.
- **Audit**, **Regulatory**, **Client**, **Service Performance** — not
  yet built (same pattern as IT Risk Issues Management: one report at a
  time).

## Processes

| Process | Status | Path |
|---|---|---|
| Inquiry & Response Tracking | Documented | [processes/inquiry-response-tracking/](processes/inquiry-response-tracking/) |

This is very likely not the complete list — response drafting/QA and
exam/audit engagement setup are probably their own processes too, just
not documented yet.

## Why this Service, why this process first

Response Management is already sample-rated `Red` in
[docs/service-catalog.md](../../docs/service-catalog.md) (missed
regulator deadlines, high resubmission rate), same reasoning that made IT
Risk Issues Management the first Service deepened — a Red rating with a
mechanical, trackable workflow underneath it is exactly where automation
prototyping pays off first.

## Domain reference

Inquiries are tagged against the same 8-domain taxonomy IT Risk Issues
Management uses for `Risk_Category`, now pulled from a single shared
lookup rather than redefined per service — see
[docs/reference/domains.md](../../docs/reference/domains.md).
