# CLAUDE.md — catMaxTarot

This file documents the codebase structure, conventions, and workflows for AI assistants working on this repository.

## Project Overview

**catMaxTarot** is a static personal link-in-bio website for a tarot reader named Max. It is hosted via GitHub Pages at `www.catmaxtarot.com`.

The site presents a terminal/hacker aesthetic — dark background, monospace fonts, green terminal prompt — and links visitors to Max's social media profiles.

## Repository Structure

```
catmaxtarot/
├── index.html   # The entire website (HTML + CSS + JS, all inline)
└── CNAME        # GitHub Pages custom domain: www.catmaxtarot.com
```

This is an intentionally minimal repository. There are **no build tools, no package managers, no frameworks, and no external dependencies beyond Google Fonts**. Everything lives in a single `index.html`.

## Technology Stack

| Layer      | Technology                                      |
|------------|-------------------------------------------------|
| Markup     | HTML5 (single file)                             |
| Styling    | Vanilla CSS (inline `<style>` block in HTML)    |
| Scripting  | Vanilla JavaScript (inline `<script>` in HTML)  |
| Fonts      | Google Fonts — JetBrains Mono, Crimson Pro      |
| Hosting    | GitHub Pages (static, no server-side logic)     |
| Domain     | `www.catmaxtarot.com` via `CNAME` file          |

## Visual Design System

All CSS variables are defined in `:root` at the top of the `<style>` block:

| Variable        | Value                         | Usage                          |
|-----------------|-------------------------------|--------------------------------|
| `--bg`          | `#0a0a0f`                     | Page background                |
| `--surface`     | `#111118`                     | Card/container background      |
| `--border`      | `#1a1a25`                     | Borders and dividers           |
| `--green`       | `#4ade80`                     | Primary accent (terminal green)|
| `--green-dim`   | `#22633a`                     | Muted green                    |
| `--green-glow`  | `rgba(74, 222, 128, 0.15)`    | Green glow effects             |
| `--amber`       | `#d4a853`                     | Name/title display color       |
| `--amber-dim`   | `rgba(212, 168, 83, 0.3)`     | Amber glow/shadow              |
| `--text`        | `#c8c8d0`                     | Primary body text              |
| `--text-dim`    | `#5a5a6e`                     | Secondary/muted text           |
| `--purple`      | `#a78bfa`                     | Prompt path color              |

## HTML Structure

```
<body>
  .vignette              — Fixed radial gradient overlay for depth
  .particles#particles   — JS-generated floating particle dots
  .card                  — Centered terminal-style card
    .titlebar            — macOS-style window chrome (traffic light dots + title)
    .terminal            — Main content area
      .prompt            — Shell prompt line with blinking cursor
      .identity          — Name, role label, and tagline
      .tarot-sigil       — Decorative divider with card suit symbols (♠ ♥ ♦ ♣)
      .links             — 2-column grid of social media links
      .footer            — Status indicator and version badge
```

## JavaScript

The only JavaScript (at the bottom of `<body>`) generates 20 floating particle elements:

```js
const container = document.getElementById('particles');
for (let i = 0; i < 20; i++) {
  const p = document.createElement('div');
  p.className = 'particle';
  p.style.left = Math.random() * 100 + '%';
  p.style.animationDuration = (8 + Math.random() * 12) + 's';
  p.style.animationDelay = (Math.random() * 10) + 's';
  p.style.width = p.style.height = (1 + Math.random() * 1.5) + 'px';
  container.appendChild(p);
}
```

Particles float upward using the `@keyframes float` CSS animation.

## Animations

All entrance animations are CSS-only, staggered via `animation-delay`:

| Element         | Animation       | Delay  |
|-----------------|-----------------|--------|
| `.card`         | `cardIn`        | 0.3s   |
| `.prompt`       | `typeIn`        | 0.8s   |
| `.identity`     | `fadeUp`        | 1.2s   |
| `.tarot-sigil`  | `fadeUp`        | 1.5s   |
| `.links`        | `fadeUp`        | 1.8s   |
| `.footer`       | `fadeUp`        | 2.1s   |

## Social Links

| Platform   | URL                                             | Handle           |
|------------|-------------------------------------------------|------------------|
| Instagram  | `https://www.instagram.com/cat_max_tarot`       | @cat_max_tarot   |
| Telegram   | `https://t.me/cat_max_tarot`                    | @cat_max_tarot   |
| LINE       | `https://line.me/R/ti/p/@cat_max_tarot`         | @cat_max_tarot   |
| YouTube    | `https://www.youtube.com/@cat_Max_Tarot`        | @cat_Max_Tarot   |

Each `.link` element has a hover effect: a 2px green left-border accent animates in, the icon turns green, and a subtle green background tint appears.

## Responsive Design

A single breakpoint at `max-width: 500px` adjusts:
- Terminal padding reduced
- Name font size drops from 42px to 32px
- Social links grid switches from 2 columns to 1 column
- Link padding reduced

## Development Workflow

### Making Changes

Because the entire site is a single HTML file, edits are made directly to `index.html`. There is no build step, compilation, or local server required — open `index.html` in a browser to preview.

### Deployment

The site deploys automatically via GitHub Pages on push to the `master` branch. The `CNAME` file configures the custom domain. Do not remove or modify `CNAME`.

### Branch Convention

Feature branches follow the pattern: `claude/<description>-<session-id>`

Work on feature branches, then merge to `master` to deploy.

## Key Conventions

1. **No build tooling** — Do not introduce npm, webpack, vite, or any build system. This is intentionally a zero-dependency static site.
2. **Single file** — Keep all HTML, CSS, and JS in `index.html` unless there is a compelling reason to split (e.g., the file grows large enough that maintenance becomes difficult).
3. **CSS variables first** — Use the defined `:root` variables for all colors. Do not introduce new hardcoded hex values without adding a variable.
4. **No external CSS/JS** — Do not add CDN links for frameworks (Bootstrap, jQuery, etc.). The existing Google Fonts link is acceptable.
5. **Preserve the aesthetic** — The terminal/hacker dark theme is the core identity of the site. Any new UI elements must follow the existing color system, font choices, and animation style.
6. **Keep animations CSS-only** — Entrance and ambient animations use CSS keyframes. Avoid adding JS animation libraries.
7. **Accessibility** — Maintain `lang="en"` on `<html>`, `rel="noopener"` on external links, and meaningful `aria` attributes if interactive elements are added.
8. **Version bump** — The footer shows `v1.0`. Update this string when making significant visual or content changes.

## Common Tasks

### Adding a new social link

1. Copy an existing `.link` block inside the `.links` div in `index.html`
2. Update the `href`, SVG icon, platform label, and handle text
3. The 2-column grid layout accommodates even numbers of links best; for an odd count, consider a `grid-column: span 2` on the last item at mobile widths

### Changing the status message

Edit the text inside `.footer .status > span` (currently `"available for readings"`).

### Updating the custom domain

Edit `CNAME` — one line, the bare domain (e.g., `www.catmaxtarot.com`). Also update the GitHub Pages settings in the repository if needed.

### Modifying colors

Change the relevant CSS variable value in the `:root` block. The change propagates site-wide automatically.
