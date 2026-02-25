# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

SkillRecruiter is a hexagon tile-based recruitment dashboard prototype for visualizing and managing open job positions. It is a **frontend-only prototype** built with vanilla HTML/CSS/JavaScript — no frameworks, no build tools, no package manager.

## Running

Open any HTML file directly in a browser. No build step or dev server required.

- `hexagon-hover-prototype-mini.html` — Primary/latest prototype with job creation flow
- `hexagon-hover-prototype-details.html` — Variant with expanded card content on hover
- `hexagon-hover-prototype.html` — Original baseline prototype

## Architecture

Each HTML file is a self-contained single-file app (HTML + CSS + JS inlined). There is no shared code between prototypes.

### Visual System
- 5-column CSS Grid of hexagonal tiles using `clip-path: polygon()`
- Regular hexagons: width × 2/√3 height ratio with clip-path `polygon(50% 0%, 100% 25%, 100% 75%, 50% 100%, 0% 75%, 0% 25%)`
- `--stroke-width` CSS variable (`:root`) controls all border/stroke widths globally
- Diamond-shaped activity indicators with randomized pulsing animations (2:3 width:height ratio)
- Search console: IBM Plex Mono Medium, bordered box with slide-down transition, blinking green cursor

### Typography
- **Inter Light (300)** — large display text: tile numbers, job titles, candidate counts
- **Inter Regular (400)** — body text, labels, UI elements (set on `body`)
- **IBM Plex Mono Medium (500)** — search console / terminal output
- Font files in `Fonts/` directory, loaded via `@font-face`

### Brand Guidelines (Skill)
Reference: https://pentagrams.my.canva.site/skill-brand-guidelines-v-1-0/
- Core brand concept: "Edge" — advanced, precision, speed
- Primary brand color: `#990690` (deep magenta), accent: `#ff4cf7` (bright magenta)
- Secondary colors: yellow `#f7e800`, orange `#ff8a00`
- Black `#000`, white `#fff`, support gray `#f4f1f0`
- Tone: direct, incisive, intelligent
- Grid-based layout, modern minimalist aesthetic

### Interaction Model
- **Tile hover**: Border turns magenta, reveals inline stats or card content
- **Tile click**: Zoom transition from tile position into detail/create screen
- **Zoom transitions**: Transform-origin calculated from tile center and cursor position; grid zooms out (scale 8) while detail screen zooms in (scale 0.5→1)
- **State via DOM classes**: `.zooming-out`, `.zoomed`, `.active`, `.disabled` plus an `isAnimating` flag to prevent overlapping transitions

### Data
- Job positions stored as an in-memory JS object (`jobData`) with fields: count, department, location, level, salary, description
- No persistence layer — all state is lost on refresh
- `lancedb/` directory is reserved for planned vector database integration

### Job Creation (mini prototype only)
- "+" tile opens a form via zoom transition
- Required field: job title (validates with red border flash)
- Save dynamically creates a new tile in the DOM and adds to `jobData`

### Search Console (mini prototype)
- Activated by "Find Candidates" on Job Summary screen
- Replaces job description/meta with a terminal-style log box
- Lines appear every 5 seconds with dummy progress text
- "Review Candidates" / "Modify Search" return to Job Summary
- Transition: content snap-hides, console slides down from divider position (0.5s ease-in)

## Conventions
- HTML entities for special characters (e.g. `&#8592;` for back arrow ←)
- Line breaks in job titles use `<br>` tags
- CSS cubic-bezier timing for smooth transitions
- Bottom-row tiles use `.disabled` class for greyed-out appearance
- Action button icons use mini hexagons (same clip-path as tiles) with `vector-effect: non-scaling-stroke`
- Placeholder text: italic, `#333`
