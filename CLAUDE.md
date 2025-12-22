# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Artist portfolio website for Esmeralda (Esmé Belle) showcasing paintings, drawings, and 3D art. Static site built with HTML, CSS, and vanilla JavaScript using Vite as the dev server.

## Development Commands

```bash
npm run dev     # Start Vite dev server with hot reload
npm install     # Install dependencies (just Vite)
```

## Architecture

### File Structure
- `index.html` - Main portfolio page (~1,800 lines) containing all CSS, JS, and the artworks data array
- `about.html` - Artist biography page
- `assets/images/` - Artwork images and decorative cutout PNGs
- `assets/elements/artwork/` - Individual artwork detail photos

### Data-Driven Rendering
All artwork data lives in the `artworks` array (around line 1143 in index.html). Each entry has:
```javascript
{
    src: "path/to/image.jpg",     // or images: [...] for carousels
    type: "Paintings",            // Paintings | Drawings | 3D-Art
    title: "Artwork Title",
    medium: "Oil on canvas",
    size: "24x36 in",
    collection: "Collection Name"
}
```

### Key JavaScript Components
- **`Carousel` class** - Handles multi-image artwork navigation with swipe/click support
- **`renderArtworks(filterType)`** - Renders artwork cards, accepts filter parameter
- **`initObserver()`** - IntersectionObserver that updates floating metadata footer and background color based on viewport
- **`setBackground(type)`** - Changes page background color by artwork type
- **Custom cursor** - Circle following mouse, expands on hover via `attachCursorHover()`

### CSS Variables (defined in :root)
- `--bg-pinstripe-1`, `--bg-pinstripe-2` - Background colors
- `--bg-clay` - Clay beige accent
- `--text-main` - Primary text color
- `--font-body` - Body font (Bodoni Moda)

## Code Conventions

- All CSS and JS are embedded inline in HTML files
- 4-space indentation
- camelCase for JS functions, kebab-case for CSS classes
- New artwork: add to `artworks` array in index.html, place image in `assets/images/`
- Responsive breakpoint at 768px (mobile hamburger menu below this)

## Active Development

See `hero_plan.md` for the current hero animation development roadmap (Thailand Days motorcycle animation phases).
