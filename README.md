# ChromaLab — Color Scale & Contrast Studio

[![Quality](https://github.com/MykolaDotsenko/Color-Picker-react-training-app/actions/workflows/quality.yml/badge.svg)](https://github.com/MykolaDotsenko/Color-Picker-react-training-app/actions/workflows/quality.yml)

**Generate a deterministic 50–950 color scale, inspect HEX/RGB/HSL values, measure WCAG contrast, preview foreground choices and export CSS variables.**

This repository began as an eight-button React color-picker exercise. The current version keeps that learning history visible while turning the useful part into a small, testable color-engineering tool.

> **Repository name note:** `Color-Picker-react-training-app` is the historical GitHub name. The current product name is **ChromaLab**.

## Current status

The application builds and is covered by unit, browser and accessibility checks.

There is **no public deployment claimed currently**. The previous GitHub Pages workflow was removed because Pages was not enabled for this repository; keeping a permanently red deployment workflow would be worse evidence than stating the deployment status plainly.

## What it does

- native color picker + editable 3/6-digit HEX input;
- deterministic 50–950 tint/shade scale;
- HEX / RGB / HSL inspection;
- WCAG relative-luminance and contrast ratios;
- AA / AAA labels;
- automatic black/white foreground selection by measured contrast;
- live surface preview;
- copyable swatches;
- CSS custom-property export;
- browser-saved seed color with defensive fallback.

## Color domain

```text
seed
 ↓
normalize
 ↓
RGB / HSL / contrast
 ↓
50–950 scale
 ↓
CSS token export
 ↓
React presentation
```

The color module has no React, DOM, storage or clipboard dependency.

The scale uses deterministic sRGB mixing. It deliberately does **not** claim perceptual uniformity; OKLCH would be a better model if perceptually even lightness steps became a product requirement.

## Contrast

ChromaLab implements WCAG relative luminance and derives contrast as:

```text
(Llighter + 0.05) / (Ldarker + 0.05)
```

Tests include the canonical black/white **21:1** case and the AA / AAA grade boundaries.

The preview chooses black or white text by comparing the two measured contrast ratios. That is useful for a binary foreground recommendation, but it is not presented as a complete accessibility audit of an arbitrary design system.

## Architecture

```text
React UI
   ↓
pure color domain
   ├── normalization
   ├── RGB / HSL conversion
   ├── scale generation
   ├── luminance / contrast
   └── CSS serialization

browser storage
   ↓
validated seed color
```

No color library, router, state library or component framework is required for the current scope.

## Stack

- React 19
- Vite 8
- JavaScript / native ES modules
- Web Storage
- Clipboard API
- Node built-in test runner
- Playwright
- axe-core
- ESLint
- GitHub Actions

Runtime dependencies are only React and React DOM.

## Quality

```bash
npm ci
npm run check
npx playwright install chromium
npm run test:e2e
```

The suite covers:

- HEX normalization and RGB round trips;
- deterministic color mixing and scale generation;
- WCAG contrast math and foreground selection;
- CSS token serialization;
- corrupt persisted data;
- seed editing + reload persistence;
- invalid input recovery;
- export visibility;
- serious WCAG A/AA axe violations;
- desktop/mobile horizontal overflow.

CI is read-only: it verifies the committed lockfile rather than rewriting PR branches from inside a workflow.

## Run locally

Requires Node.js 22.22+.

```bash
npm ci
npm run dev
```

Production build:

```bash
npm run build
npm run preview
```

## Quick code review

- [`src/domain/color.js`](./src/domain/color.js) — color math and serialization
- [`src/components/ColorPickerApp.jsx`](./src/components/ColorPickerApp.jsx) — UI composition
- [`src/storage/colorStorage.js`](./src/storage/colorStorage.js) — persisted seed boundary
- [`tests/color.test.js`](./tests/color.test.js) — domain regression tests
- [`e2e/chromalab.spec.js`](./e2e/chromalab.spec.js) — browser/accessibility coverage
- [`.github/workflows/quality.yml`](./.github/workflows/quality.yml) — CI gate

## Scope

ChromaLab is a compact color utility and learning-history repository, not a Figma replacement or a complete design-system platform.

That constraint is intentional: the repository is most useful as evidence of color-domain logic, accessibility math, defensive persistence and proportionate React architecture.
