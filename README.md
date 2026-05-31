# ui

> Production-grade SCSS UI framework for enterprise SaaS applications.

ui is a **CSS/SCSS-only** design system built for internal company UI systems. It is framework-agnostic — works equally well with Angular, React, Vue, Svelte, or plain HTML — and ships zero JavaScript.

---

## Table of contents

- [Philosophy](#philosophy)
- [Quick start](#quick-start)
- [Architecture](#architecture)
- [Token system](#token-system)
- [Theme engine](#theme-engine)
- [Components](#components)
- [Forms](#forms)
- [Utilities](#utilities)
- [Responsive system](#responsive-system)
- [Accessibility](#accessibility)
- [Build](#build)
- [Customisation](#customisation)
- [Roadmap](#roadmap)

---

## Philosophy

ui follows four guiding principles:

1. **Tokens first.** Every design decision — color, spacing, radius, shadow — is defined as a token. Tokens map to CSS custom properties so themes can change the entire system without touching component code.
2. **State over JS.** Components expose their appearance via state classes (`.is-open`, `.is-active`, `.is-disabled`). Your framework — or zero lines of JS — controls those classes. ui only styles them.
3. **Low specificity.** No `!important`. No deep selector nesting. Utilities sit at the same specificity level as components so composition works naturally.
4. **Accessible by default.** Every interactive component includes `focus-visible` rings, `prefers-reduced-motion` support, and makes semantic HTML assumptions.

---

## Quick start

### CDN (pre-built)

```html
<link rel="stylesheet" href="https://unpkg.com/ui/dist/ui.css" />
```

### npm

```bash
npm install ui
```

Then in your SCSS entry point:

```scss
// Full framework
@use 'ui/scss/ui';

// Or individual parts
@use 'ui/scss/tokens/colors';
@use 'ui/scss/components/button';
```

### Theme setup

Add `data-theme` to your root element to control the color scheme:

```html
<!-- Light (default — no attribute needed) -->
<html data-theme="light">

<!-- Dark -->
<html data-theme="dark">

<!-- Custom company brand -->
<html data-theme="company">
```

Dark mode also responds to `prefers-color-scheme: dark` automatically when no `data-theme` attribute is present.

---

## Architecture

```
ui/
├── scss/
│   ├── core/
│   │   ├── _reset.scss          Modern CSS reset
│   │   ├── _root.scss           Semantic CSS variable declarations
│   │   ├── _globals.scss        HTML element defaults
│   │   ├── _animations.scss     Keyframes + animation utilities
│   │   └── _mixins.scss         Reusable SCSS mixins & functions
│   │
│   ├── tokens/
│   │   ├── _colors.scss         Color palettes (SCSS maps)
│   │   ├── _spacing.scss        Spacing scale → CSS vars
│   │   ├── _typography.scss     Font sizes, weights, line heights → CSS vars
│   │   ├── _radius.scss         Border radius scale → CSS vars
│   │   ├── _shadows.scss        Shadow scale → CSS vars
│   │   ├── _breakpoints.scss    Breakpoint map + bp() function
│   │   └── _zindex.scss         index scale → CSS vars
│   │
│   ├── themes/
│   │   ├── _light.scss          :root light theme values
│   │   ├── _dark.scss           [data-theme=dark] + prefers-color-scheme
│   │   └── _company.scss        Custom brand theme scaffold
│   │
│   ├── utilities/
│   │   ├── _display.scss
│   │   ├── _flex.scss
│   │   ├── _spacing.scss        Margin + padding helpers
│   │   ├── _typography.scss     Text size, weight, alignment
│   │   ├── _sizing.scss         Width, height, overflow
│   │   ├── _positions.scss      Position, inset, index
│   │   ├── _visibility.scss     Opacity, cursor, pointer-events
│   │   └── _responsive.scss     Breakpoint-prefixed variants
│   │
│   ├── layout/
│   │   ├── _container.scss      Responsive max-width container
│   │   ├── _grid.scss           CSS grid system (1–12 columns)
│   │   ├── _section.scss        Page section padding
│   │   └── _stack.scss          Vertical stack, cluster, switcher
│   │
│   ├── forms/
│   │   ├── _field.scss          Label + hint + error wrapper
│   │   ├── _input.scss          Text inputs + addon groups
│   │   ├── _textarea.scss       Auto-growing textarea
│   │   ├── _select.scss         Styled native select
│   │   ├── _checkbox.scss       Checkbox + radio (shared base)
│   │   ├── _radio.scss          Radio stub
│   │   └── _switch.scss         Toggle switch
│   │
│   ├── components/
│   │   ├── _button.scss         Button variants, sizes, group
│   │   ├── _card.scss           Card + parts + modifiers
│   │   ├── _accordion.scss      Collapse panel (native details)
│   │   ├── _dropdown.scss       Contextual menu
│   │   ├── _modal.scss          Dialog + drawer
│   │   ├── _menu.scss           Command palette / context menu
│   │   ├── _nav.scss            Navbar, sidenav, tabs, breadcrumb
│   │   ├── _table.scss          Data table + pagination
│   │   └── _link.scss           Link styles + badge/chip
│   │
│   └── ui.scss                Framework entry point
│
├── example/
│   └── index.html               Component showcase
├── package.json
└── README.md
```

---

## Token system

All tokens are defined as SCSS maps and exposed as CSS custom properties.

### Colors

```scss
@use 'ui/scss/tokens/colors' as *;

// Access a value
background: map-get($color-blue, 600);

// All palettes available:
// $color-neutral, $color-blue, $color-green,
// $color-red, $color-yellow, $color-orange
```

### Spacing

```scss
// SCSS map
$spacing: (xs: 0.25rem, sm: 0.5rem, md: 1rem, lg: 1.5rem, xl: 2rem, xxl: 3rem, xxxl: 4rem)

// CSS variable usage
padding: var(--space-md);
gap: var(--space-lg);
```

### Typography

```scss
// CSS variable usage
font-size: var(--text-sm);      // 14px
font-size: var(--text-base);    // 16px
font-size: var(--text-2xl);     // 24px

font-weight: var(--font-semibold);
line-height: var(--leading-relaxed);
letter-spacing: var(--tracking-tight);
```

### All available CSS custom properties

| Group      | Pattern                    | Example                         |
|------------|----------------------------|---------------------------------|
| Space      | `--space-{key}`          | `--space-md`                  |
| Text size  | `--text-{key}`           | `--text-lg`                   |
| Font weight| `--font-{key}`           | `--font-semibold`             |
| Leading    | `--leading-{key}`        | `--leading-normal`            |
| Tracking   | `--tracking-{key}`       | `--tracking-wide`             |
| Radius     | `--radius-{key}`         | `--radius-lg`                 |
| Shadow     | `--shadow-{key}`         | `--shadow-md`                 |
| index    | `--{key}`              | `--modal`                   |
| Transition | `--transition-{speed}`   | `--transition-base`           |

---

## Theme engine

### Semantic variables

Components never reference raw color values. They only use semantic variables:

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

/* Semantic states */
--color-success, --color-success-bg
--color-danger,  --color-danger-bg
--color-warning, --color-warning-bg
--color-info,    --color-info-bg

/* Interactive chrome */
--input-bg, --input-border, --input-border-focus, --input-placeholder
--color-focus
```

### Creating a custom theme

1. Copy `scss/themes/_company.scss` to your project.
2. Set the semantic CSS variables to your brand values.
3. Apply with `data-theme="company"` on `<html>`.

```scss
// my-theme.scss
[data-theme='my-brand'] {
  --color-brand:        #7c3aed;
  --color-brand-hover:  #6d28d9;
  --color-brand-subtle: #f5f3ff;
  --bg-base:            #ffffff;
  // ... rest of semantic variables
}
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
<button class="btn btn--danger-outline">Danger outline</button>

<!-- Sizes -->
<button class="btn btn--primary btn--xs">XS</button>
<button class="btn btn--primary btn--sm">SM</button>
<button class="btn btn--primary">MD</button>
<button class="btn btn--primary btn--lg">LG</button>
<button class="btn btn--primary btn--xl">XL</button>

<!-- States -->
<button class="btn btn--primary btn--loading">Loading</button>
<button class="btn btn--primary" disabled>Disabled</button>

<!-- Full width -->
<button class="btn btn--primary btn--block">Full width</button>

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
    <h3 class="card__title">Card title</h3>
  </div>
  <div class="card__body">
    <p>Card content goes here.</p>
  </div>
  <div class="card__footer">
    <button class="btn btn--primary btn--sm">Action</button>
  </div>
</article>

<!-- Modifiers: --flat  --bordered  --elevated  --interactive  --flush  --horizontal -->
<a href="/detail" class="card card--interactive">...</a>
```

### Modal

```html
<!-- Trigger with JS: modalEl.classList.toggle('is-open') -->
<div class="modal is-open" role="dialog" aria-modal="true" aria-labelledby="modal-title">
  <div class="modal__panel">
    <div class="modal__header">
      <div>
        <h2 class="modal__title" id="modal-title">Dialog title</h2>
        <p class="modal__description">Supporting description text.</p>
      </div>
      <button class="modal__close" aria-label="Close">✕</button>
    </div>
    <div class="modal__body">
      <p>Modal content.</p>
    </div>
    <div class="modal__footer">
      <button class="btn btn--outline">Cancel</button>
      <button class="btn btn--primary">Confirm</button>
    </div>
  </div>
</div>

<!-- Sizes: --sm  --lg  --xl  --full -->
<!-- Variant: --drawer (side panel) -->
```

### Accordion

```html
<!-- Native <details> — zero JS required -->
<div class="accordion">
  <details class="accordion__item" open>
    <summary class="accordion__trigger">
      Question text
      <svg class="accordion__trigger__icon">...</svg>
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

### Field wrapper

Every form control should be wrapped in `.field` for consistent label + hint + error layout:

```html
<div class="field">
  <label class="field__label" for="email">
    Email
    <span class="field__label__required" aria-hidden="true">*</span>
  </label>
  <div class="field__control">
    <input class="input" id="email" type="email" required />
  </div>
  <span class="field__hint">We'll never share your email.</span>
  <span class="field__error">Please enter a valid email address.</span>
</div>

<!-- Add .is-invalid to .field to show the error message -->
<div class="field is-invalid">...</div>
```

### Input variants

```html
<!-- Sizes -->
<input class="input input--sm" type="text" />
<input class="input" type="text" />
<input class="input input--lg" type="text" />

<!-- With icon -->
<div class="input-icon">
  <svg class="input-icon__icon"><!-- search icon --></svg>
  <input class="input" type="search" placeholder="Search…" />
</div>

<!-- With addon -->
<div class="input-group">
  <span class="input-group__addon">https://</span>
  <input class="input" type="url" placeholder="example.com" />
  <span class="input-group__addon">.io</span>
</div>
```

### Checkboxes, radios, switch

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

<!-- Switch -->
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

### Display

```html
<div class="flex">...</div>
<div class="grid">...</div>
<div class="hidden">...</div>
<span class="sr-only">Screen reader only text</span>
```

### Flex

```html
<div class="flex items-center justify-between gap-md">
  <span>Left</span>
  <span>Right</span>
</div>

<div class="flex flex-col gap-sm">
  <div>A</div>
  <div>B</div>
</div>
```

### Grid

```html
<!-- Fixed columns -->
<div class="grid-3 gap-md">
  <div>1</div><div>2</div><div>3</div>
</div>

<!-- Auto-fill responsive -->
<div class="auto-grid-md gap-md">
  <!-- as many columns as fit at min 16rem -->
</div>

<!-- Column span -->
<div class="grid-12">
  <div class="col-span-8">Main</div>
  <div class="col-span-4">Aside</div>
</div>
```

### Spacing

```html
<div class="p-md">padding: 1rem</div>
<div class="px-lg py-sm">padding-inline: 1.5rem; padding-block: 0.5rem</div>
<div class="mt-xl mb-md">margin-block-start: 2rem; margin-block-end: 1rem</div>
<div class="mx-auto">centered</div>
```

### Typography

```html
<p class="text-sm text-secondary">Small secondary text</p>
<h2 class="text-3xl font-bold tracking-tight">Large heading</h2>
<p class="text-center text-tertiary leading-relaxed">Centered relaxed</p>
<span class="uppercase tracking-wider font-semibold text-xs">Label</span>
<p class="truncate">This text will be truncated with an ellipsis if too long</p>
```

---

## Responsive system

ui is **mobile-first**. Breakpoint prefixes apply a utility at that viewport width and above.

### Breakpoints

| Prefix | Min-width |
|--------|-----------|
| `sm:`  | 640px     |
| `md:`  | 768px     |
| `lg:`  | 1024px    |
| `xl:`  | 1280px    |
| `2xl:` | 1536px    |

### Usage

```html
<!-- Stack on mobile, row on md+ -->
<div class="flex flex-col md:flex-row gap-md">
  <aside>Sidebar</aside>
  <main>Content</main>
</div>

<!-- Hidden on mobile, visible from lg -->
<nav class="hidden lg:flex">Desktop nav</nav>

<!-- Full width on mobile, auto from sm -->
<button class="btn btn--primary w-full sm:w-auto">Submit</button>

<!-- Responsive padding -->
<section class="py-lg xl:py-xxl">...</section>

<!-- Responsive grid -->
<div class="grid-1 md:grid-2 lg:grid-3 gap-md">...</div>
```

> **Note:** In HTML, prefix classes with colons need no escaping. In CSS files, colons in class names are escaped with a backslash: `.md\:flex-row`.

---

## Accessibility

ui is built with accessibility as a first-class concern:

- **Focus rings** — all interactive components have `focus-visible` outlines that respect `--color-focus`. They appear only for keyboard navigation, not mouse clicks.
- **Reduced motion** — all transitions and animations are wrapped in `@media (prefers-reduced-motion: no-preference)` or disabled via the `reduce` variant.
- **Color contrast** — the default light and dark themes meet WCAG AA contrast ratios for text on surface.
- **Semantic HTML** — components assume correct markup (`<button>`, `<a>`, `<label>`, `<fieldset>`, ARIA attributes). ui styles the element; you supply the semantics.
- **Screen-reader utilities** — `.sr-only` / `.not-sr-only` for visually hidden but accessible content.

---

## Build

### Requirements

- Node.js 18+
- Dart Sass 1.70+ (`sass` npm package — **not** `node-sass`)

### Commands

```bash
# Install dependencies
npm install

# Build compressed CSS → dist/ui.css
npm run build

# Build expanded (readable) CSS
npm run build:expanded

# Watch for changes during development
npm run watch
```

### Output

```
dist/
├── ui.css           # Compressed production build
└── ui.expanded.css  # Expanded development build
```

### Integrating with bundlers

**Vite**
```js
// vite.config.js
import { defineConfig } from 'vite'
export default defineConfig({
  css: { preprocessorOptions: { scss: {} } }
})
```
```js
// main.js
import 'ui/scss/ui.scss'
```

**webpack + sass-loader**
```js
// webpack.config.js
{ test: /\.scss$/, use: ['style-loader', 'css-loader', 'sass-loader'] }
```

**Angular**
```json
// angular.json
"styles": ["node_modules/ui/scss/ui.scss", "src/styles.scss"]
```

---

## Customisation

### Override tokens

Declare your overrides **before** importing ui (SCSS `!default` flag):

```scss
// my-tokens.scss
$color-blue: (
  600: #7c3aed,  // override brand blue with purple
  700: #6d28d9,
);
$spacing: (
  md: 1.125rem,  // slightly larger base spacing
);

@use 'ui/scss/ui'; // picks up your overrides
```

### Override component radius globally

```css
:root {
  --component-radius: var(--radius-lg); /* rounder components */
}
```

### Extend a component

Because ui uses low specificity, you can safely extend or override:

```scss
// my-button-extension.scss
.btn--brand-gradient {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  border-color: transparent;
  color: white;

  &:hover {
    filter: brightness(1.1);
  }
}
```

---

## Roadmap

### v1.1
- [ ] Toast / notification component
- [ ] Tooltip component
- [ ] Progress bar + spinner
- [ ] Avatar + avatar group
- [ ] Popover positioning engine (pure CSS anchor positioning)

### v1.2
- [ ] Data visualization color tokens
- [ ] Skeleton loader component
- [ ] File upload drop zone
- [ ] Date input styling
- [ ] Slider / range input

### v1.3
- [ ] RTL (right-to-left) full support (logical properties already in place)
- [ ] CSS layers (`@layer`) architecture for zero-specificity conflicts
- [ ] Container query responsive variants (`cq:grid-2`)
- [ ] Print stylesheet
- [ ] High-contrast mode support (`forced-colors`)

### v2.0
- [ ] Component API documentation site
- [ ] Figma token export / import pipeline
- [ ] SCSS → CSS-only build option (no Sass required at runtime)
- [ ] Storybook integration guide
- [ ] Token audit tooling

---

## License

MIT © ui contributors
