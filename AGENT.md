# UI SCSS Framework — AI Agent Reference Guide

> **Source:** [github.com/bahtirek/SCSS](https://github.com/bahtirek/SCSS)  
> **Version:** v1.0  
> **Type:** CSS/SCSS-only, zero JavaScript, framework-agnostic

---

## Overview

`ui` is a production-grade SCSS design system for enterprise SaaS applications. It works with Angular, React, Vue, Svelte, or plain HTML and ships **no JavaScript**. All interactivity is driven by state classes (e.g. `.is-open`, `.is-active`) toggled by the host framework.

---

## Installation

### Via CDN
```html
<link rel="stylesheet" href="https://unpkg.com/ui/dist/ui.css" />
```

### Via npm
```bash
npm install ui
```

### SCSS Import
```scss
// Full framework
@use 'ui/scss/ui';

// Individual parts only
@use 'ui/scss/tokens/colors';
@use 'ui/scss/components/button';
```

---

## Theme Setup

Apply a `data-theme` attribute to `<html>` to control the color scheme:

```html
<html data-theme="light">   <!-- default -->
<html data-theme="dark">
<html data-theme="company"> <!-- custom brand -->
```

Dark mode also auto-activates via `prefers-color-scheme: dark` when no `data-theme` is set.

---

## Architecture

```
scss/
├── core/          → reset, root vars, globals, animations, mixins
├── tokens/        → colors, spacing, typography, radius, shadows, breakpoints, z-index
├── themes/        → light, dark, company (custom scaffold)
├── utilities/     → display, flex, spacing, typography, sizing, position, visibility, responsive
├── layout/        → container, grid, section, stack
├── forms/         → field, input, textarea, select, checkbox, radio, switch
└── components/    → button, card, accordion, dropdown, modal, menu, nav, table, link
```

---

## Token System

### CSS Custom Property Patterns

| Group       | Pattern                  | Example              |
|-------------|--------------------------|----------------------|
| Space       | `--space-{key}`          | `--space-md`         |
| Text size   | `--text-{key}`           | `--text-lg`          |
| Font weight | `--font-{key}`           | `--font-semibold`    |
| Line height | `--leading-{key}`        | `--leading-normal`   |
| Letter spacing | `--tracking-{key}`    | `--tracking-wide`    |
| Radius      | `--radius-{key}`         | `--radius-lg`        |
| Shadow      | `--shadow-{key}`         | `--shadow-md`        |
| Transition  | `--transition-{speed}`   | `--transition-base`  |

### Spacing Scale
```scss
// Available keys: xs, sm, md, lg, xl, xxl, xxxl
padding: var(--space-md);   // 1rem
gap: var(--space-lg);       // 1.5rem
```

### Typography Scale
```scss
font-size: var(--text-sm);    // 14px
font-size: var(--text-base);  // 16px
font-size: var(--text-2xl);   // 24px
font-weight: var(--font-semibold);
line-height: var(--leading-relaxed);
```

---

## Semantic Color Variables

Components only reference semantic variables — never raw color values:

```css
/* Surface */
--bg-base, --bg-subtle, --bg-elevated, --bg-overlay

/* Border */
--border-default, --border-strong, --border-subtle

/* Text */
--text-primary, --text-secondary, --text-tertiary,
--text-disabled, --text-inverse, --text-on-brand

/* Brand */
--color-brand, --color-brand-hover, --color-brand-subtle

/* Status */
--color-success, --color-success-bg
--color-danger,  --color-danger-bg
--color-warning, --color-warning-bg
--color-info,    --color-info-bg

/* Form chrome */
--input-bg, --input-border, --input-border-focus,
--input-placeholder, --color-focus
```

---

## Components

### Button

```html
<!-- Variants -->
<button class="btn btn--primary">Primary</button>
<button class="btn btn--secondary">Secondary</button>
<button class="btn btn--outline">Outline</button>
<button class="btn btn--ghost">Ghost</button>
<button class="btn btn--danger">Danger</button>
<button class="btn btn--danger-outline">Danger Outline</button>

<!-- Sizes: --xs  --sm  (default md)  --lg  --xl -->
<button class="btn btn--primary btn--sm">Small</button>

<!-- States -->
<button class="btn btn--primary btn--loading">Loading</button>
<button class="btn btn--primary" disabled>Disabled</button>

<!-- Full width -->
<button class="btn btn--primary btn--block">Full Width</button>

<!-- Group -->
<div class="btn-group">
  <button class="btn btn--outline">Day</button>
  <button class="btn btn--outline">Week</button>
  <button class="btn btn--outline">Month</button>
</div>
```

### Card

```html
<article class="card">
  <div class="card__header">
    <h3 class="card__title">Title</h3>
  </div>
  <div class="card__body">Content</div>
  <div class="card__footer">
    <button class="btn btn--primary btn--sm">Action</button>
  </div>
</article>

<!-- Modifiers: --flat  --bordered  --elevated  --interactive  --flush  --horizontal -->
<a href="/detail" class="card card--interactive">...</a>
```

### Modal

```html
<!-- Toggle visibility: modalEl.classList.toggle('is-open') -->
<div class="modal is-open" role="dialog" aria-modal="true" aria-labelledby="modal-title">
  <div class="modal__panel">
    <div class="modal__header">
      <div>
        <h2 class="modal__title" id="modal-title">Title</h2>
        <p class="modal__description">Description text.</p>
      </div>
      <button class="modal__close" aria-label="Close">✕</button>
    </div>
    <div class="modal__body">Content</div>
    <div class="modal__footer">
      <button class="btn btn--outline">Cancel</button>
      <button class="btn btn--primary">Confirm</button>
    </div>
  </div>
</div>

<!-- Size modifiers: --sm  --lg  --xl  --full -->
<!-- Variant: --drawer (side panel) -->
```

### Accordion

```html
<!-- Zero JS — uses native <details> -->
<div class="accordion">
  <details class="accordion__item" open>
    <summary class="accordion__trigger">
      Question text
      <svg class="accordion__trigger__icon"><!-- chevron --></svg>
    </summary>
    <div class="accordion__content">
      <div class="accordion__body">Answer text.</div>
    </div>
  </details>
</div>

<!-- JS variant: toggle .is-open on .accordion__item -->
```

### Nav

```html
<!-- Top navbar -->
<nav class="navbar">
  <a href="/" class="navbar__brand">Logo</a>
  <ul class="navbar__links">
    <li><a href="#" class="nav-link is-active">Home</a></li>
    <li><a href="#" class="nav-link">About</a></li>
  </ul>
  <div class="navbar__actions">
    <button class="btn btn--primary btn--sm">Sign in</button>
  </div>
</nav>

<!-- Tabs -->
<nav class="tabs">
  <a href="#" class="tabs__item is-active">Overview</a>
  <a href="#" class="tabs__item">Settings</a>
</nav>

<!-- Breadcrumb -->
<ol class="breadcrumb">
  <li class="breadcrumb__item"><a href="/" class="breadcrumb__link">Home</a></li>
  <li class="breadcrumb__item"><a href="/users" class="breadcrumb__link">Users</a></li>
  <li class="breadcrumb__item"><span class="breadcrumb__current">Alice</span></li>
</ol>
```

---

## Forms

### Field Wrapper (required for all inputs)

```html
<div class="field">
  <label class="field__label" for="email">
    Email
    <span class="field__label__required" aria-hidden="true">*</span>
  </label>
  <div class="field__control">
    <input class="input" id="email" type="email" required />
  </div>
  <span class="field__hint">Hint text.</span>
  <span class="field__error">Error message.</span>
</div>

<!-- Show error state -->
<div class="field is-invalid">...</div>
```

### Input Variants

```html
<!-- Sizes: --sm  (default)  --lg -->
<input class="input input--sm" type="text" />
<input class="input input--lg" type="text" />

<!-- With icon -->
<div class="input-icon">
  <svg class="input-icon__icon"><!-- icon --></svg>
  <input class="input" type="search" placeholder="Search…" />
</div>

<!-- With addons -->
<div class="input-group">
  <span class="input-group__addon">https://</span>
  <input class="input" type="url" placeholder="example.com" />
  <span class="input-group__addon">.io</span>
</div>
```

### Checkboxes, Radios, Switch

```html
<!-- Checkbox -->
<label class="check-label">
  <input class="checkbox" type="checkbox" />
  <span>Accept terms</span>
</label>

<!-- Radio group -->
<div class="check-group">
  <label class="check-label"><input class="radio" type="radio" name="plan" /> Free</label>
  <label class="check-label"><input class="radio" type="radio" name="plan" /> Pro</label>
</div>

<!-- Toggle switch -->
<label class="switch-label">
  <input class="switch" type="checkbox" role="switch" />
  <span class="switch-label__text">
    <span class="switch-label__title">Notifications</span>
    <span class="switch-label__description">Get alerts when something happens.</span>
  </span>
</label>
```

---

## Utilities

### Display & Visibility
```html
<div class="flex">...</div>
<div class="grid">...</div>
<div class="hidden">...</div>
<span class="sr-only">Screen reader only</span>
```

### Flex
```html
<div class="flex items-center justify-between gap-md">
  <span>Left</span>
  <span>Right</span>
</div>

<div class="flex flex-col gap-sm">...</div>
```

### Grid
```html
<!-- Fixed columns (1–12 available) -->
<div class="grid-3 gap-md">
  <div>1</div><div>2</div><div>3</div>
</div>

<!-- Auto-fill responsive -->
<div class="auto-grid-md gap-md">...</div>

<!-- 12-column with spans -->
<div class="grid-12">
  <div class="col-span-8">Main</div>
  <div class="col-span-4">Aside</div>
</div>
```

### Spacing
```html
<div class="p-md">padding: 1rem</div>
<div class="px-lg py-sm">padding inline/block</div>
<div class="mt-xl mb-md">margin top/bottom</div>
<div class="mx-auto">horizontally centered</div>
```

### Typography
```html
<p class="text-sm text-secondary">Small secondary</p>
<h2 class="text-3xl font-bold tracking-tight">Large heading</h2>
<p class="text-center leading-relaxed">Centered relaxed paragraph</p>
<span class="uppercase tracking-wider font-semibold text-xs">Label</span>
<p class="truncate">Truncated with ellipsis…</p>
```

---

## Responsive System

The framework is **mobile-first**. Prefix any utility with a breakpoint to apply it at that width and above.

| Prefix | Min-width |
|--------|-----------|
| `sm:`  | 640px     |
| `md:`  | 768px     |
| `lg:`  | 1024px    |
| `xl:`  | 1280px    |
| `2xl:` | 1536px    |

> **Note:** In HTML, colon-prefixed classes work without escaping. In `.scss` files, escape the colon: `.md\:flex-row`.

---

## Customisation

### Override Tokens (before import)
```scss
// my-tokens.scss — override BEFORE @use
$color-blue: (
  600: #7c3aed,
  700: #6d28d9,
);
$spacing: (
  md: 1.125rem,
);

@use 'ui/scss/ui'; // picks up overrides via !default
```

### Custom Theme
```scss
[data-theme='my-brand'] {
  --color-brand:        #7c3aed;
  --color-brand-hover:  #6d28d9;
  --color-brand-subtle: #f5f3ff;
  --bg-base:            #ffffff;
  // ... remaining semantic variables
}
```
Apply with `<html data-theme="my-brand">`.

### Global Radius Override
```css
:root {
  --component-radius: var(--radius-lg);
}
```

### Extend a Component
```scss
// Low specificity means extensions work naturally
.btn--brand-gradient {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  border-color: transparent;
  color: white;

  &:hover { filter: brightness(1.1); }
}
```

---

## Build

### Requirements
- Node.js 18+
- Dart Sass 1.70+ (`sass` npm package — not `node-sass`)

### Commands
```bash
npm install          # install dependencies
npm run build        # compressed → dist/ui.css
npm run build:expanded  # readable → dist/ui.expanded.css
npm run watch        # watch mode for development
```

### Bundler Integration

**Vite**
```js
// vite.config.js
export default defineConfig({ css: { preprocessorOptions: { scss: {} } } })
// main.js
import 'ui/scss/ui.scss'
```

**webpack**
```js
{ test: /\.scss$/, use: ['style-loader', 'css-loader', 'sass-loader'] }
```

**Angular**
```json
"styles": ["node_modules/ui/scss/ui.scss", "src/styles.scss"]
```

---

## Accessibility Notes

- **Focus rings** — `focus-visible` outlines on all interactive elements; keyboard-only, no mouse trigger.
- **Reduced motion** — all transitions respect `prefers-reduced-motion`.
- **WCAG AA** — default light/dark themes meet contrast ratio requirements.
- **Semantic HTML assumed** — use correct elements (`<button>`, `<a>`, `<label>`, ARIA attributes); the framework styles them, you supply the semantics.
- **Screen reader utilities** — `.sr-only` / `.not-sr-only`.

---

## Key Patterns for AI Agents

1. **State is class-based.** To open a modal: add `.is-open`. To mark a nav link active: add `.is-active`. To disable a form field: add `.is-disabled`. No JS events or data attributes needed — just class toggles.
2. **Always wrap form controls in `.field`.** This is required for correct label, hint, and error layout.
3. **Never use raw color values in components.** Always reference semantic CSS variables like `--color-brand` or `--text-primary`.
4. **Token overrides go before `@use`.** SCSS `!default` means the first declaration wins.
5. **Zero JavaScript shipped.** Any interactivity (dropdowns, modals, accordions) must be wired up by the consuming framework via class manipulation.

---

*MIT License — ui contributors*
