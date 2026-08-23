# Artifacts

Self-contained HTML snapshots of the pages published via the Artifact
tool during this project — one file each, inline CSS and vanilla JS, no
build step, no external dependencies.

| File | Live version | Covers |
|---|---|---|
| `strategy-2026-2029.html` | https://claude.ai/code/artifact/f7fa8255-330e-4c5c-b8dd-847358f608ae | Department 2026–2029 strategy, paged by section with collapsible nav groups: Overview (org structure, risk taxonomy, People), Processes/Risks/Controls, Technology (Program Walrus), Regional (US/EU/APAC), Functions (the six center pillars), Operations (Issues Management, Response Management — summarized from the two files below), Financial (placeholder), consolidated roadmap, pain-point summary, and open items. Governance pilots the standard section template (mission, capabilities & achievements, gaps & 3-year deliverables, milestone timeline) pending rollout to the rest. |
| `it-risk-issues-management.html` | https://claude.ai/code/artifact/ae791543-54b6-4745-a6d7-434fecab19f1 | Mirrored from `work-automation`. IT Risk Issues Management service overview: fixed process nav (Reporting default), Service Performance / Issues Summary / Issue Aging Report on the sample data in `services/it-risk-issues-management/`. |
| `response-management.html` | https://claude.ai/code/artifact/402cf547-0dc7-4f8e-af24-bdf2e2c6ecb8 | Mirrored from `work-automation`. Response Management (Audit - Internal and External, Regulatory, Client) service overview: fixed nav (Overview default; Audit, Regulatory, Client, Service Performance not yet built), cross-channel inquiry Overview report on the sample data in `services/response-management/`. |

## Design system for future artifacts

These HTML artifacts share a common design baseline via [design-system.css](design-system.css), carried over from the `work-automation` project's operating-model artifacts so this repo's documents read as the same visual language. Use it as the starting point for every new artifact here.

### Baseline principles

- Keep a calm paper-and-ink palette with a small set of semantic tokens for paper, ink, muted, line, and status colors.
- Use serif headlines for narrative structure and monospace labels for metadata and section cues.
- Favor restrained borders, generous whitespace, and consistent card-like containers.
- Reuse shared patterns such as hero headers, stat rows, filter bars, and callouts instead of inventing new visual treatments for each artifact.

### Workflow

1. Start each new artifact with the same page shell: eyebrow, title, deck, metadata row, and labeled sections.
2. Link the page to [design-system.css](design-system.css) near the top of the HTML head.
3. Keep any page-specific styling in the existing inline style block, but make it override the shared tokens only when needed.

**The `<link>` only works here, in the repo copy.** The Artifact tool's
publishing target has a strict CSP that blocks external stylesheet
requests, so the *live* published version of each page must stay fully
self-contained (all CSS inline) — it can't fetch `design-system.css` at
all. The repo copies additionally carry the `<link>` tag purely for
local/GitHub viewing, since it's a no-op there (the inline `<style>`
already defines the same tokens, so the linked sheet doesn't change
anything even where it does load) — don't remove the inline tokens when
adding the link, and don't expect the link to do anything when the page
is opened via its `claude.ai/code/artifact/...` URL.

## Underlying data for the mirrored artifacts

`it-risk-issues-management.html` and `response-management.html` embed
their sample data inline (no runtime `fetch`), but the source CSVs they
were built from are mirrored too, at [../services/](../services/) and
[../docs/reference/domains.md](../docs/reference/domains.md) — see
[../services/README.md](../services/README.md) for the scope of what
was and wasn't carried over from `work-automation`.

## These are snapshots, not the source of truth

The document is drafted and reasoned about turn-to-turn in conversation;
this HTML file is the rendered output at the point it was last
committed. If the live artifact gets updated in a later session without
also updating the file here, this copy will be stale — re-export/re-copy
the file rather than hand-editing the HTML out of sync with its live
counterpart.
