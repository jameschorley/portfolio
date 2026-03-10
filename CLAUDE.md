# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Personal portfolio site. No build system, no framework, no package manager — everything is self-contained standalone HTML files with embedded CSS and JS.

## Workflow

Open any file directly in a browser. There is no dev server, no build step, and no compilation required.

```bash
open index.html                 # macOS — open in default browser
open -a "Google Chrome" index.html
```

After making changes, commit and push:

```bash
git add <file>
git commit -m "descriptive message"
git push
```

Remote: `https://github.com/jameschorley/portfolio`

## Architecture

Each HTML file is fully self-contained — all CSS lives in a `<style>` block in `<head>`, all JS lives in a `<script>` block before `</body>`. No external JS dependencies; Google Fonts and `picsum.photos` (placeholder images) are the only external resources.

### `index.html` — main portfolio

The file is structured in sections top-to-bottom, mirroring the visual page order:

1. **CSS custom properties** (`--bg`, `--text`, `--gold`, etc.) defined in `:root` — change these to retheme the entire page.
2. **Site header** — fixed, gains `.scrolled` class via scroll listener → frosted glass effect.
3. **Hero** — full-viewport intro with staggered CSS `animation-delay` reveals.
4. **Projects accordion** — each `.project` article contains:
   - `.project-header` — clickable, calls `toggleProject()`.
   - `.project-body` — collapsed via `max-height: 0` by default; `toggleProject()` sets `body.style.maxHeight = body.scrollHeight + 'px'` to animate open.
   - `.media-scroll` — horizontal flex strip; gradient fades updated by `updateFades()` on scroll.
5. **Lightbox** — fixed overlay (`#lightbox`); `openLightbox(itemEl)` collects all `.media-item` siblings, renders image or `<video>` depending on `data-type`. Keyboard navigation via `keydown` listener.
6. **Custom cursor** — `#cursor-dot` snaps instantly; `#cursor-ring` lags behind via `requestAnimationFrame` lerp.

### Adding a project

Copy an existing `<article class="project">` block. Each `.media-item` needs three `data-` attributes:
- `data-src` — full image/video URL shown in lightbox
- `data-caption` — text shown in lightbox footer
- `data-type` — `"image"` or `"video"`

Video items also need `class="is-video"` on `.media-item` and the play-icon SVG inside `.media-item-overlay`.

## Design Tokens

| Token | Value | Usage |
|---|---|---|
| `--bg` | `#080808` | Page background |
| `--text` | `#f0ede6` | Primary text |
| `--gold` | `#c9a96e` | Accent, interactive states |
| `--text-dim` | `#7a7570` | Secondary text, metadata |
| `--scroll-h` | `280px` | Fixed height of all media strip items |

**Fonts:** Cormorant Garamond (display/titles) + Space Mono (metadata/UI). Both loaded from Google Fonts.

## Git Conventions

- Commit every meaningful change with a clear message describing *what changed and why*
- Push after every commit so GitHub always has the latest
- Commit message format: imperative mood, present tense — e.g. `Add project 6 — motion graphics reel`, `Fix lightbox keyboard nav on Safari`
