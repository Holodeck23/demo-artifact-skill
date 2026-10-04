---
name: project-to-portfolio
description: >
  Turn an existing project or repo into a shipped portfolio artifact — a splash page that
  sells it PLUS a self-contained interactive demo. Also improve an existing landing
  page and embedded demo when that is the user's scope. Trigger on "put X in the portfolio", "build a demo for X", "make a
  portfolio page for a project", "turn this repo into a demo", or any request to present
  something already built as a shareable, clickable thing. NOT for greenfield products —
  this skill's whole premise is that the artifact already exists and must be READ, not
  imagined.
---

# project-to-portfolio

**One line:** scour an existing project and assemble it into a shipped portfolio artifact —
a splash that sells it, plus a self-contained interactive demo.

This skill is the pipeline. `demo-artifact` is the packaging standard it hands off to for
each agreed artifact. Packaging alone does not supply a usable demo; this pipeline
connects the product evidence, visitor journey and verification.

Evidence base: three full runs against real repos. Run 1 was rebuilt three times; runs 2
and 3, with RECON done first, had zero rebuilds. That delta is the skill.

## Choose the deliverable from the brief

For a new portfolio piece, the default is the two files below. For an existing website
or an explicit landing-only brief, improve its existing route and embed the useful
interactive slice there. Do not create a second demo route just to satisfy the default.
Keep any established release links, hosting configuration and evidence boundaries.

| File | What it is | Done when |
|---|---|---|
| `<project>/index.html` | splash — sells the thing, pushes into the demo | multiple "Launch the demo" CTAs (nav, hero, launch band, closing) |
| `<project>/demo/index.html` | the demo — a working, clickable slice of the product | opens from `file://` with the network off and you can actually use it |

**The visitor must reach a usable result.** A splash with a dead launch link is unfinished.
A page with an embedded demo can be complete when it matches the brief and passes the
same behavior checks. A result may be a local simulation, but label it plainly and make
the promised slice work.

## The one failure this skill exists to prevent

**Designing from a description when the artifact is one click away.** Every rebuild in run 1
had the same root cause:

| Rebuild | What was used | What should have been opened |
|---|---|---|
| 1 | the README | the actual components — the real control model |
| 2 | an invented palette | the screenshots in the repo — the app's real look |
| 3 | a card's `href` | the reference demo's live URL itself |

Plus a fourth class: **verifying over `python3 -m http.server`**, which sends no framing/CSP
headers and resolves bare directories — so it silently passes exactly the two bug classes
that then fail in production.

A pointer that says "look first" gets skipped. So RECON is gated: it produces a written
artifact, and later phases consume it. **No design decision may be made before `recon.md`
exists.**

## Phase 0 — RECON (produces `recon.md`; nothing else starts without it)

**Before anything else — confirm the target is worth featuring.** Recon reads the disk, and
the disk does not know who a project was built for or whether it should be seen publicly.
Confirm the target from the user's request and context; ask only when it is unclear.
An unattributed third-party
brand in the source is a signal to ask, not a default to quietly neutralise. (One run was
built end to end and then withdrawn: it was a throwaway for a named client, which no file on
the machine recorded.)

Write `recon.md` in the workspace's canonical location for reports, with evidence beside
it; record the code checkout separately. Use `<project>/recon.md` only where local rules
allow reports in the project. Required sections:

- **Locate the source.** Find the canonical repo and any relevant parked or duplicated
  sibling before creating a checkout. Reuse the source; do not assume duplicates exist.
- **The running artifact.** Screenshots in the repo, design explorations, a build output,
  or actually start it. Record **hex values and font stacks**. "Clean and modern" is not
  recon and does not satisfy this section.
  Open the screenshots as images, including the exact states the demo will show. A file
  inventory or a screenshot gallery added to the landing page is not visual inspection.
  Record the layout, panel proportions, type sizes, controls and state transitions for
  those states. Prefer building a browser demo from the real components and styles with
  a local data adapter when feasible; avoid maintaining an invented imitation beside them.
- **The full surface, from the code.** Enumerate every page/route from the router or route
  table (`routes.ts`, `App.tsx`, `urls.py`…) and list them in recon.md with what the demo
  will cover or deliberately omit. Scope the demo around a complete useful journey;
  it need not reproduce the whole application. A screenshot in `assets/` is dated
  evidence, not the current UI — it can predate a whole redesign. (One run built a single scrolling overview from a pre-SPA
  preview image; the live app had 11 routed pages.)
  The same holds below page level: any component the demo re-creates is rebuilt from its
  SOURCE file and its real name, never from its README. A README describes; the source is
  the referent. (Another run rebuilt a dashboard from `README.md` while the module defining
  its 17 panels sat beside it, and shipped an agent under the wrong name because the README
  abbreviated it.)
- **Every third-party surface the demo imitates** (Telegram, Slack, Gmail) is a reference
  too: record its real layout parts before building, then render and compare against it.
- **The reference.** If the brief says "like X", OPEN X and record its tokens and page
  structure. Never infer X from a link.
- **The data/domain layer.** Content libraries, domain tokens, migrations, seed data. The
  architecture story lives here, not in the README.
- **The aesthetic direction.** Use the user's established choice when available.
  Otherwise ask if product fidelity versus a new marketing identity would materially
  change the work. State a direction and one memorable interaction; scaffolds do not
  determine the palette, typography or headline.
- **Original language.** If the product is not English-first, the demo opens in its original
  language with a working toggle. Translating it to English flattens the local specificity
  that is often the entire differentiator.

## Phase 1 — THESIS

One non-obvious decision worth showing. Not "I built an app" — the decomposition.

> Example, from a contract-drafting tool: *the model never writes the contract; it picks a
> posture and the text is retrieved.*

Name the audience and the result they care about. An engineering portfolio may lead with
the structural decision; a product landing page should lead with the visitor's problem
and show its resolution. Map each important promise to a working interaction or real
product evidence. Do not use repeated abstract slogans in place of that connection.

## Phase 2 — BUILD (the agreed surface)

Build the **demo first**. It is the hard half and the thing being sold; a splash written
before the demo exists ends up promising something the demo does not do.

- **demo** — one self-contained HTML. Seeded data, no backend, no keys, no model call.
  Opens from `file://` with the network off. Cover the journey agreed in recon, using
  current source for the relevant controls and states.
- **splash** — a marketing page that sells and pushes into the product. Multiple "Launch
  the demo" CTAs. NOT a case study with a provenance section; that reads as an academic
  exercise.
- Wear the product's own skin so launching feels like entering the product.
- Architecture section: **only when the thesis is structural.** Do not force it.

Each portable artifact follows the `demo-artifact` packaging standard: single file,
everything inline, no CDN or runtime font downloads, works offline.

For any interactive work, read [Demo acceptance](references/demo-acceptance.md) before
building and again before declaring completion. Write a short interaction contract:
visitor action → visible result → state that persists → reset behavior. Remove apparent
controls that the scoped demo does not support, or label them clearly as unavailable.

## Phase 3 — HONESTY PASS (non-negotiable; it is the credibility move)

Cross-tab the real data, find a genuine gap, put it on the page.

> Example: 7 of 9 risk×stance cells populated, mass on a diagonal — so the axes are
> modelled independent and populated correlated.

Naming your own gap reads as someone who understands their system. Never manufacture a fake
weakness, and never soften a real one into a feature.

## Phase 4 — VERIFY (behavior, rendering and delivery are separate)

Both scripts ship with this skill, in `scripts/`.

```bash
python3 scripts/check-links.py <project>/index.html <project>/demo/index.html
node scripts/check-render.mjs <project>/index.html <project>/demo/index.html --width 390,1280
```

`check-links.py` is stdlib-only and needs nothing. `check-render.mjs` needs Playwright
(`npm i playwright && npx playwright install chromium`) — installed either in the project
being checked or beside the script; it resolves from both, and names the fix if it is absent.

| # | Check | Mechanism |
|---|---|---|
| 1 | Every internal `href`/`src` resolves to a FILE, not a directory | `check-links.py` — resolves as a `file://` browser would |
| 2 | CSS specificity: no bare-element descendant rule outranking a component class | `check-render.mjs` — compares the winning rule's computed colour against overridden class rules |
| 3 | Contrast ≥ 4.5:1 (3:1 large) against the **resolved** ancestor background | `check-render.mjs` — composites translucent layers up the tree |
| 4 | 390px: page overflow 0; wide things scroll inside their own container | `check-render.mjs --width 390` |
| 5 | Full interaction run, programmatic, **against the deployed URL** — not localhost | manual/Playwright, per demo |
| 6 | The splash's "Launch the demo" CTA actually opens the demo | click it, in the place it will be viewed |

These scripts prove only the properties they measure. Neither is an interaction test,
and neither certifies that the demo is useful or that the deployed page matches the file.
Build on the project's existing browser harness to exercise the interaction contract
from the visitor's entry point. Assert observable results, denial/cancel behavior,
cross-view persistence and reset. Inspect every apparent control, including decorative
elements that selectors for buttons would miss. Visually inspect the result on narrow
and wide screens, then repeat the same journey in the actual delivery context.
Compare those rendered states beside the product references at comparable viewport sizes.
List intentional deviations (sample data, larger text, narrow-screen layout) and resolve
unintentional ones before claiming product fidelity. A passing interaction suite cannot
overrule a visible mismatch with the app. Borrow a reference demo's interaction structure,
not its unrelated brand or invented product content.

Report each result with its environment: offline file, local HTTP, hosted preview or
production. If publishing is not authorized, finish the local work and explicitly leave
production verification open. Re-run the relevant gates after changes; do not describe
a few passing clicks as an unqualified "interactive pass".

Check 2 is the one that cannot be caught by reading: `.navlinks a` beats `.btn-dark`, and the
CSS reads correctly in isolation. It shipped in two separate runs.

`check-render.mjs` is mutation-tested: injecting `div.mutwrap a { color:#111 }` over
`.btn-x { color:#fff }` produces `FAIL specificity`, and known-good pages produce none.

## Phase 5 — SHIP

When publishing is authorized, deploy, wire up the agreed index or portfolio links, then
confirm the live URL and run the same visitor journey there. An HTTP 200 only proves
retrievability. Keep source commits, pushes and live deployment distinct in the handoff.

Host-specific things that have bitten this pipeline, worth checking on yours:

- **If your host has an auth/passcode layer, a new directory is usually gated by default.**
  Whatever allowlist your proxy or middleware reads, add the new path to it in the same
  commit. This is invisible locally — `file://` and any dev server serve it fine.
- **Confirm which host is actually production before quoting a URL.** A vanity domain can
  be pinned to an old deployment that does not follow production. Check what the deploy
  actually published, every run — this has been wrong twice.
- **Re-check ~60s after deploy.** Edge networks propagate static files before proxy config,
  so a public path can 401 for about a minute after the assets already return 200.
- **Check the framing headers in the delivery context.** `X-Frame-Options: DENY` will block
  a demo from being iframed by its own splash page. A local dev server will not show you this.

## Standing decisions (settled across three runs — do not relitigate)

- **Variant generation is not part of this flow.** When the artifact exists, variants are
  four inventions competing with a real thing. Recon replaces them. Keep variant-first
  methods for greenfield.
- **Architecture section only when the thesis is structural.**
- **Non-English products open in their original language,** with a working toggle.
- **The agreed journey must work end to end, whether embedded or on its own route.**
