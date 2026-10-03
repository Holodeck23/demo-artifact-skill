---
name: demo-artifact
description: >
  Package a demo, landing page or shareable single-page artifact as self-contained HTML
  with embedded assets and an offline core experience. Use for portable HTML deliverables
  or alongside project-to-portfolio. Preserve an existing website's stack and the user's
  chosen visual direction; this skill governs packaging and verification, not a fixed look.
---

# Demo Artifact — the self-contained HTML standard

For a portable HTML deliverable, ship one `.html` with inline CSS, script and assets.
Its core experience must work from disk without a network. This does not replace a
requested document format or force a framework website into a single file. Follow the
user's requested delivery surface and the workspace's canonical save locations.

## Pair with design, not a fixed aesthetic
If an aesthetics/design skill is active, it governs the look; this skill governs packaging
and the offline guarantee. The assets below are starting points, not proof of design
quality. Choose type, hierarchy and copy for the product and audience. A page should have
one clear promise, an observable payoff and an obvious next action.

## Reuse the useful parts of a scaffold
`assets/` holds four building blocks. Reuse a matching one for new work; preserve useful
structure in an existing artifact rather than replacing it to match a template:

| Asset | Use for |
|-------|---------|
| `standalone-light-scaffold.html` | reports, audits, findings docs (bone/cream, sidebar nav) — for *informing* |
| `standalone-dark-scaffold.html` | demos, pitches, showcases, landing exports (dark, kinetic hero) — for *impressing* |
| `hero-template.html` | the condensed period-per-line hero + feature/steps/CTA sections |
| `design-tokens.css` | the token system + collision-checked accent presets |

The light and dark treatments are options, not purpose-based requirements. Fork one, name it
`product-name-v1.html`, change the `PRODUCT_SLUG` string near the top of the `<script>`
(it isolates the localStorage keys), then replace content and delete sections you don't need.
Restraint matters: use four or five section types, not all of them.

Everything inside `hero-template.html`'s `<body>` is fictional example copy for a made-up
product called Meridian. It is there to show the shape, not to be kept. Replace all of it.

## The rules this skill enforces (encode these, don't just link)

**(a) Self-contained core.** For portable output, use one `.html` with inline CSS/JS,
embedded fonts and images or inline SVG. No runtime CDN dependencies. Outbound anchors
are allowed; they are not offline resources. Preserve authorized hosted integrations
where required, ensure the core works without them and do not make blanket privacy
claims contradicted by analytics or other requests.

**(b) Theme with semantic tokens.** Centralize palette values in `:root`. The supplied
scaffold can be rebranded with one primary token:
```css
--color-accent: #2D6EE8;   /* the only line you swap per product */
```
Collision-checked presets (all safe against the locked AI-action yellow `#F0C040`):
Signal blue `#2D6EE8` · Teal green `#1A9B62` · Violet `#7C3AED` · Cyan `#0891B2`.
In this scaffold, yellow is reserved for AI actions; keep brand and state colors
distinguishable. Those token values are a preset, not a universal brand requirement.
Change the system deliberately when the product calls for it, then remeasure contrast.

**(c) Headline follows the promise.** The supplied condensed, period-per-line hero is
one option. Do not require a font, three lines, three colors or this sentence structure
on every product. Its example:
```
ONE DASHBOARD.   ← bone/white (the thing)
EVERY GUEST.     ← accent      (the scope)
FULLY AUTOMATED. ← outcome color (what they get)
```
Replace example claims with supported benefits. For a product landing page, show the
result of the promise near the headline; a list of capabilities is not a substitute.

**(d) Palette is a design choice.** The warm-dark and bone assets are useful presets.
Preserve or evolve the product's identity when requested. Do not impose a light/dark
treatment simply because the artifact is a demo or report.

**(e) Offline gate — necessary, not sufficient.** Open the file in a browser context
with networking disabled and exercise its core journey. Capture failed requests and
JavaScript errors. Prefer a per-context offline setting; do not turn off the user's
machine-wide Wi-Fi. Search source for external resources as a supplement, distinguishing
anchors and license URLs from live-loaded dependencies.

Passing offline rendering does not establish interaction quality. Every apparent control
must work within the agreed scope, be clearly unavailable, or be removed. A screenshot,
composer-shaped rectangle or success message alone is not a working demo. If paired with
`project-to-portfolio`, use its interaction contract and demo-acceptance reference.

**(f) Outbound links/CTAs — real anchors, never `window.open()`.** When the file is hosted
inside a sandboxed viewer (a Claude Artifact, an iframe embed, some email previewers), the
sandbox blocks script-triggered popups — `onclick="window.open(url)"` silently does nothing,
with no error. Use `<a href="url" target="_blank" rel="noopener">` instead; a real anchor
click survives the sandbox. Before calling any artifact with an outbound link done, click it
once in the place it will actually be viewed. "The code looks right" is not the same check as
"I clicked it and it opened." This rule exists because a `window.open()` CTA shipped on a live
deck and the button was dead for everyone who touched it.

**Font note:** all three HTML assets use a condensed *system* stack
(`'Barlow Condensed','Arial Narrow',system-ui,sans-serif`) and ship zero webfonts, on
purpose — a CDN font link breaks rule (a) quietly, because the page still renders, just in
the wrong face. If you want the real Barlow Condensed, embed it as a base64 `@font-face`.
Never re-add a runtime font `<link>` to portable output. Include embedded fonts' copyright
and license text in the distributed artifact.

## Ship path (reference, don't automate)
A finished single-file `index.html` is already deployable as-is: drag it into Netlify Drop,
push it to a GitHub Pages repo, or commit it as `app-name/index.html` in any Vercel-connected
repo and it goes live on the next push. The point of the single-file rule is that the deploy
step is never the hard part. Mention the path; don't run it unless asked.

## Known contrast gaps in the shipped assets (verify before you trust them)

Run `project-to-portfolio/scripts/check-render.mjs` over anything you build from these.
Measured against WCAG AA on the assets as shipped:

| Asset | Status |
|---|---|
| `standalone-light-scaffold.html` | **passes** at 390px and 1280px |
| `standalone-dark-scaffold.html` | muted text `#555250` on `#0d0d0d` is 2.5:1; accent CTAs (white on `#7B61FF`) are 4.2:1 |
| `hero-template.html` | the hot-pink accent is the issue: white on `#FF2D78` is 3.56:1, pink on bone is 3.14:1 |

These are palette-level, not layout bugs. Decide deliberately: either darken the accent
until it clears 4.5:1, or reserve the accent for large text only (3:1) and keep body copy on
the neutral tokens. What you must not do is ship it unmeasured and assume it is fine.

## Honest limit
The scaffolds supply structure and tokens; only verification establishes the offline
guarantee. Judge copy, usability and visual quality separately. In the handoff, name the
environment actually tested and leave untested hosted behavior explicit.
