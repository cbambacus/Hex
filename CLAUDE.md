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
- Color scheme: black (#000) background, white text, magenta (#e535e5) accents
- System font stack (-apple-system, BlinkMacSystemFont, Segoe UI, Roboto)
- Diamond-shaped activity indicators with randomized pulsing animations

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

## Conventions
- HTML entities for special characters (e.g. `&#8592;` for back arrow ←)
- Line breaks in job titles use `<br>` tags
- CSS cubic-bezier timing for smooth transitions
- Bottom-row tiles use `.disabled` class for greyed-out appearance
