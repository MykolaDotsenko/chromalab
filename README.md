# ChromaLab — Accessible Color System Studio

[![Quality](https://github.com/MykolaDotsenko/Color-Picker-react-training-app/actions/workflows/quality.yml/badge.svg)](https://github.com/MykolaDotsenko/Color-Picker-react-training-app/actions/workflows/quality.yml)

**Turn one seed color into a 50–950 scale, inspect its values, check WCAG contrast and export CSS variables.**

This repository began as an eight-button React color-picker exercise. The current version keeps the UI small and moves the interesting work into testable color functions.

**Demo status:** no public deployment is claimed currently.

## What it does

- native color picker + editable 3/6-digit HEX;
- deterministic 50–950 tint/shade generation;
- HEX / RGB / HSL inspection;
- WCAG relative-luminance and contrast ratios;
- AA / AAA labels;
- automatic black/white foreground choice;
- live preview;
- copyable swatches;
- CSS custom-property export;
- versioned saved seed color.

## Color domain

```text
seed
 ↓
normalize
 ↓
palette + RGB/HSL + contrast
 ↓
CSS token output
 ↓
React presentation
```

The color module has no React, DOM, storage or clipboard dependency.

The palette uses deterministic sRGB mixing. It deliberately does **not** claim perceptual uniformity; OKLCH would be a better choice if evenly perceived lightness steps became a product requirement.

## Contrast

ChromaLab implements WCAG relative luminance and derives contrast as:

```text
(Llighter + 0.05) / (Ldarker + 0.05)
```

Tests include the canonical black/white 21:1 case and grade boundaries.

## Stack

- React 18
- Vite 5
- JavaScript
- modern CSS
- Web Storage
- Clipboard API
- Node built-in tests
- ESLint
- GitHub Actions

No color library, state library, router or component framework.

## Accessibility

The studio uses native inputs, explicit labels, `aria-invalid`, visible focus, live copy status, reduced-motion and forced-colors fallbacks.

The product treats contrast calculation as domain behaviour rather than a decorative accessibility badge.

## Quality

```bash
npm ci
npm run check
```

Tests cover HEX normalization, RGB round trips, color mixing, token generation, WCAG math, foreground selection, CSS serialization and corrupt persistence.

## Run locally

```bash
npm ci
npm run dev
```

## Quick review

- [`src/domain/color.js`](./src/domain/color.js) — color math
- [`src/components/ColorPickerApp.jsx`](./src/components/ColorPickerApp.jsx) — UI composition
- [`src/storage/colorStorage.js`](./src/storage/colorStorage.js) — saved seed boundary
- [`tests/color.test.js`](./tests/color.test.js) — regression tests

## Scope

ChromaLab is a compact color utility, not a Figma replacement or full design-token platform. The repository stays public as part of the progression from a small React exercise to a testable domain-focused UI.
