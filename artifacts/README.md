# Artifacts

Self-contained HTML snapshots of the pages published via the Artifact
tool during this project — one file each, inline CSS and vanilla JS, no
build step, no external dependencies.

| File | Live version | Covers |
|---|---|---|
| `strategy-2026-2029.html` | https://claude.ai/code/artifact/f7fa8255-330e-4c5c-b8dd-847358f608ae | Department 2026–2029 strategy: org structure (F1–F8/30 subteams, reconciled against working-session pillar names), risk taxonomy, three cross-cutting themes, People/Process/Technology (Program Walrus), US/EU/APAC regional detail, all six center pillars, pain-point summary, and a 17-item open-items list. Governance pilots the standard section template (mission, capabilities & achievements, gaps & 3-year deliverables by subgroup, milestone timeline) — the other five pillars are marked "template not yet applied" pending rollout. |

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

## These are snapshots, not the source of truth

The document is drafted and reasoned about turn-to-turn in conversation;
this HTML file is the rendered output at the point it was last
committed. If the live artifact gets updated in a later session without
also updating the file here, this copy will be stale — re-export/re-copy
the file rather than hand-editing the HTML out of sync with its live
counterpart.
