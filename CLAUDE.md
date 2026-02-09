# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Lostit is a single-page marketing website for a B2B hotel lost-and-found logistics service powered by DHL Express. The entire site is a single HTML file (`Lostit page/index.html`, ~2600 lines) with no build system, no backend, and no package manager.

## Development

**No build step required.** Open `Lostit page/index.html` directly in a browser or serve it with any static file server. All dependencies are loaded via CDN (Tailwind CSS, Font Awesome 6.4.0, Google Fonts).

There are no tests, no linter, and no CI/CD configured.

## Architecture

### Single-file structure
Everything lives in `Lostit page/index.html`:
- **Lines ~39-785**: Embedded `<style>` block (custom CSS on top of Tailwind)
- **Sections in order**: Navigation (sticky) → Hero → Partner marquee → How It Works (3-step) → Solutions (hotel/guest cards) → DHL partnership → Hotel logos grid → Footer
- **JavaScript at bottom**: i18n system, mobile menu, scroll-reveal (IntersectionObserver), logo fallback chain

### Internationalization (i18n)
- 5 languages: ES (default), EN, CAT, FR, PT
- All translatable strings use `data-i18n` attributes on HTML elements
- Translation data is in an embedded `<script id="i18n-script">` containing a `T` object keyed by language code
- `applyLang(lang)` applies translations; selection persists via `localStorage['lostit_lang']`
- Browser language auto-detected on first visit, falling back to Spanish

### External API dependencies
- **Clearbit Logo API** (`logo.clearbit.com`): fetches hotel chain logos dynamically
- **Google Favicon API**: fallback when Clearbit fails
- Fallback chain: Clearbit → Google Favicon → plain text hotel name

### Design system
- Custom Tailwind theme colors: `dhl` (#FFCC00 yellow), `dhlred` (#D40511)
- Dark backgrounds: `#1a122e`, `#05070a`, `#0f0a18`
- Glass-morphism panels via `backdrop-filter: blur`
- Scroll-triggered `.reveal` → `.is-visible` animations
- Floating hero animation via `@keyframes float`
- Supports `prefers-reduced-motion: reduce`

### Image assets (in `Lostit page/`)
- `background.png` (8.8 MB) — hero background
- `heroe.png` (2.45 MB) — superhero character
- `caja.png` (1.86 MB) — package/box visual

These are uncompressed and large; consider optimizing if performance becomes a concern.
