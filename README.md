# Repo → sellable demo — two Claude Skills

Take a project you already built and turn it into something a stranger can click:
**a splash page that sells it, plus a working self-contained demo.**

Two skills, and you want both. They do different jobs and the difference is the whole
point:

| Skill | Job |
|---|---|
| **`project-to-portfolio`** | the **pipeline** — reads your repo, then builds the splash page *and* the interactive demo |
| **`demo-artifact`** | the **packaging standard** — guarantees each file it produces is one self-contained `.html` that works offline |

**Install `project-to-portfolio` if you want the demo.** On its own, `demo-artifact` will
give you a beautiful single-page landing page and stop there — it's the packaging layer, not
the pipeline. That's a real trap; it caught the first person who used this.

## What's in the box

```
project-to-portfolio/
  SKILL.md                          the pipeline: recon → thesis → build → honesty → verify → ship
  scripts/
    check-links.py                  every internal link resolves to a real file (stdlib only)
    check-render.mjs                specificity collisions, contrast, 390px overflow (Playwright)

demo-artifact/
  SKILL.md                          the single-file packaging rules
  assets/
    standalone-dark-scaffold.html   demos, pitches, showcases (dark, kinetic hero)
    standalone-light-scaffold.html  reports, audits, findings docs (bone, sidebar nav)
    hero-template.html              condensed period-per-line hero + sections
    design-tokens.css               token system + safe accent presets
```

Every HTML file here is a complete working page. Open any of them in a browser right now,
offline, and you'll see what the output looks like before you use it on anything.

## Install

**Claude Code (CLI):**
```bash
git clone https://github.com/Holodeck23/demo-artifact-skill
cp -R demo-artifact-skill/project-to-portfolio ~/.claude/skills/
cp -R demo-artifact-skill/demo-artifact        ~/.claude/skills/
```
Restart Claude Code. They work as project-level skills too — `.claude/skills/` inside a repo
behaves the same way and travels with the repo.

**Claude.ai (web/desktop):** Settings → Capabilities → Skills → upload each folder.

## How to use it

Point it at something that already exists:

> "Turn this repo into a portfolio piece — splash page and a working demo."

> "Build a demo for the app in ~/projects/whatever."

Then let it do recon first. That's the part that feels skippable and isn't: the pipeline
writes a `recon.md` before it designs anything, because every rebuild in its history came
from designing off a README instead of opening the actual thing.

You'll get two files: `<project>/index.html` (the splash) and `<project>/demo/index.html`
(the demo). If you only got one, the run isn't finished.

## Verifying before you send it

```bash
python3 project-to-portfolio/scripts/check-links.py  <project>/index.html <project>/demo/index.html
node    project-to-portfolio/scripts/check-render.mjs <project>/index.html <project>/demo/index.html --width 390,1280
```

`check-render.mjs` needs Playwright (`npm i playwright && npx playwright install chromium`).
`check-links.py` needs nothing.

It catches three things reading the code cannot: a CSS specificity collision where
`.navlinks a` silently beats `.btn-dark`, contrast measured against the *resolved* ancestor
background, and horizontal overflow at 390px.

## The one rule that matters

Before you call it done: turn the wifi off, double-click both files, and click the
"Launch the demo" button.

`grep -n 'http' yourfile.html` should only ever hit comments, SVG `xmlns` declarations, or
`data:` URIs — never a live-loaded font, script, stylesheet or image. If it needs the
network, it isn't finished.

## Example copy warning

Everything inside `hero-template.html`'s `<body>` is fictional filler for a made-up product
called "Meridian". It's there to show the shape. Replace all of it before you ship.

## Licence

MIT. Do whatever you like with it.
