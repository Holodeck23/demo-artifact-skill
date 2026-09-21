# demo-artifact — a Claude Skill

Turns any project, repo or idea into **one self-contained `.html` file** you can email,
AirDrop or double-click. No build step, no CDN, no server. Open it with the wifi off and
it still looks right.

It's the packaging standard, not a design opinion: it decides how the thing ships and
guarantees it works offline. What it looks like is still yours.

## What's in the box

```
demo-artifact/
  SKILL.md                            the skill itself — rules + when it fires
  assets/
    standalone-light-scaffold.html    reports, audits, findings docs (bone, sidebar nav)
    standalone-dark-scaffold.html     demos, pitches, showcases (dark, kinetic hero)
    hero-template.html                the condensed period-per-line hero + sections
    design-tokens.css                 the token system + safe accent presets
```

The three HTML files are complete working pages. Open any of them in a browser right now,
offline, and you'll see what the output looks like before you use it on anything.

## Install

**Claude Code (CLI):**
```
unzip demo-artifact-skill.zip
cp -R demo-artifact ~/.claude/skills/
```
Restart Claude Code. It's a project-level skill too — `.claude/skills/` inside a repo works
the same way and travels with the repo.

**Claude.ai (web/desktop):** Settings → Capabilities → Skills → upload the zip.

Once installed it fires on its own whenever a session is about to build a shareable HTML
page. You don't have to name it. If you want to force it, say "use demo-artifact".

## How to actually use it

Point it at something that exists:

> "Take this repo and build me a demo page for it. Single file."

> "Turn this audit into a one-pager I can send a client."

Then fork a scaffold rather than starting from blank — `standalone-dark-scaffold.html` to
impress, `standalone-light-scaffold.html` to inform. Change `PRODUCT_SLUG` near the top of
the `<script>` block (it namespaces the localStorage keys so two demos don't fight), swap
the one `--color-accent` line, replace the copy, delete the sections you don't need.

Deleting sections is most of the work. Use four or five, not all of them.

## The one rule that matters

Before you call it done: turn the wifi off and double-click the file.

That's the whole acceptance test. `grep -n 'http' yourfile.html` should only ever hit
comments, SVG `xmlns` declarations, or `data:` URIs — never a live-loaded font, script,
stylesheet or image. If it needs the network, it isn't finished.

## Example copy warning

Everything inside `hero-template.html`'s `<body>` is fictional filler for a made-up product
called "Meridian". It's there to show the shape. Replace all of it before you ship.

## Licence

Do whatever you like with it.
