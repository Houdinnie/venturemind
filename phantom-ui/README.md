---
name: nomad-flow-ui
description: VentureMind Nomad Flow — Structure-Aware Skeleton Design System | Powered by phantom-ui | MIT
metadata:
  homepage: https://phantom-ui.ing
  repo: github.com/Aejkatappaja/phantom-ui
  version: 0.10.1
  bundle: "~8kb gzipped"
  framework: Web Component (Lit) — React/Vue/Angular/Svelte/Solid/Qwik/HTMX/plain HTML
---

# Nomad Flow UI — Skeleton Design System

> Built on **phantom-ui** by Aejkatappaja · 376 GitHub stars · MIT · v0.10.1

Nomad Flow UI is VentureMind's official design system built on phantom-ui. It provides structure-aware skeleton loaders, animated states, and accessible loading indicators across all Nomad Flow pages and agent interfaces.

---

## Installation

```bash
# npm
npm install @aejkatappaja/phantom-ui

# CDN (drop-in, no build step)
<script src="https://cdn.jsdelivr.net/npm/@aejkatappaja/phantom-ui/dist/phantom-ui.cdn.js"></script>
```

Post-install auto-setup (handled by phantom-ui's postinstall script):
- **JSX types** auto-generated for React/Solid/Qwik
- **SSR pre-hydration CSS** auto-injected for Next.js/Nuxt/SvelteKit/Remix/Qwik

---

## Quick Start

```html
<!-- Wrap any content that loads asynchronously -->
<phantom-ui loading>
  <div class="dashboard-card">
    <img src="/avatar.png" width="48" height="48" style="border-radius:50%" />
    <h3>Ada Lovelace</h3>
    <p>First computer programmer, built the first algorithm.</p>
  </div>
</phantom-ui>
```

`loading` attribute shows shimmer overlay. Remove it to reveal real content.

---

## Animation Modes

| Mode | Description | Use Case |
|------|------------|---------|
| `shimmer` (default) | Diagonal gradient sweep — ltr/rtl/ttb/btt | Most loading states |
| `pulse` | Opacity fade 0.3 → 1.0 | Subtle, card-based UIs |
| `breathe` | Slow sinusoidal opacity cycle | Ambient, premium feel |
| `solid` | Static gray blocks | No animation, high-contrast |

Direction control (shimmer mode only):
```html
<phantom-ui loading animation="shimmer" shimmer-direction="rtl">
  <!-- shimmer sweeps right-to-left -->
</phantom-ui>
```

---

## Components

### 1. DashboardCardSkeleton
Card with avatar, name, bio, and action button.

```html
<phantom-ui loading animation="shimmer" count="3" count-gap="16">
  <div class="dashboard-card">
    <img src="/avatar.png" width="48" height="48" style="border-radius:50%" />
    <h3>Loading...</h3>
    <p>Description loading...</p>
    <button>Action</button>
  </div>
</phantom-ui>
```

### 2. TableSkeleton
Multi-row table with avatar, name, email, and status.

```html
<phantom-ui loading animation="pulse" count="5" count-gap="8">
  <div class="table-row">
    <img src="/placeholder.png" width="32" height="32" />
    <span>Name</span>
    <span>email@domain.com</span>
    <span class="badge">Status</span>
  </div>
</phantom-ui>
```

### 3. MetricsRowSkeleton
KPI row — three metric tiles per row.

```html
<phantom-ui loading data-shimmer-no-children>
  <div class="metrics-row">
    <span>$48.2k</span>
    <span>2,847 founders</span>
    <span>42ms p99</span>
  </div>
</phantom-ui>
```

### 4. ChartSkeleton
Chart area with title, legend, and plot area.

```html
<phantom-ui loading shimmer-direction="ttb">
  <div class="chart-container">
    <h3>Revenue Forecast</h3>
    <div class="legend">
      <span>Actual</span><span>Projected</span>
    </div>
    <div class="plot-area">
      <img data-shimmer-width="600" data-shimmer-height="300" />
    </div>
  </div>
</phantom-ui>
```

### 5. FormSkeleton
Input fields, selects, and submit button.

```html
<phantom-ui loading stagger="0.05">
  <div class="form-fields">
    <input placeholder="Company Name" />
    <select>Jurisdiction...</select>
    <input placeholder="Tax ID" />
    <button>Submit</button>
  </div>
</phantom-ui>
```

---

## VentureMind Design Tokens

```css
:root {
  /* Shimmer palette */
  --nf-shimmer-color: rgba(99, 102, 241, 0.3);   /* Indigo glow */
  --nf-shimmer-bg: rgba(99, 102, 241, 0.08);
  --nf-shimmer-duration: 1.5s;

  /* Brand palette */
  --nf-brand-primary: #6366f1;   /* Indigo-500 */
  --nf-brand-secondary: #8b5cf6;  /* Violet-500 */
  --nf-brand-accent: #f59e0b;     /* Amber-500 */

  /* Surface tokens */
  --nf-card-bg: rgba(99, 102, 241, 0.05);
  --nf-card-border: rgba(99, 102, 241, 0.15);
  --nf-surface-bg: #0f0f23;
  --nf-surface-fg: #f1f5f9;
}
```

---

## Accessibility

| Concern | Solution |
|---------|----------|
| Screen readers | `aria-busy="true"` set on `<phantom-ui loading>` |
| Keyboard nav | Real focusable elements preserved during loading |
| Motion sensitivity | Set `--shimmer-duration: 0` or use `animation="solid"` |
| Reduced motion | Respects `prefers-reduced-motion: reduce` |

---

## Performance

phantom-ui benchmarks (Chrome):

| Leaf Elements | Measurement Time |
|---------------|-----------------|
| 334 (100 nodes) | ~20ms |
| 1,667 (500 nodes) | ~25ms |
| 3,334 (1,000 nodes) | ~31ms |

Full measure → render cycle in a **single frame**. No debouncing or virtualization needed.

---

## SSR / Next.js / Nuxt

```bash
# Auto-setup (postinstall handles this)
bunx @aejkatappaja/phantom-ui init
```

For Next.js App Router, the SSR CSS is auto-injected into `app/layout.tsx`:
```ts
import "@aejkatappaja/phantom-ui/ssr.css";
```

This prevents placeholder text from flashing during hydration.

---

## Framework Examples

### React / Next.js
```tsx
import "@aejkatappaja/phantom-ui";
import { useQuery } from "@tanstack/react-query";

function FoundersList() {
  const { data, isLoading } = useQuery({ queryKey: ["founders"], queryFn: fetchFounders });
  return (
    <phantom-ui loading={isLoading} count={5} count-gap={8}>
      <div className="founder-row">
        <img src="/avatar.png" width="32" height="32" />
        <span>Founder Name</span>
        <span>founder@nomad.io</span>
      </div>
    </phantom-ui>
  );
}
```

### Vue
```vue
<template>
  <phantom-ui :loading="isLoading" count="5" count-gap="8">
    <div class="founder-row">
      <img src="/avatar.png" width="32" height="32" />
      <span>{{ placeholder.name }}</span>
    </div>
  </phantom-ui>
</template>
```

### Svelte
```svelte
<phantom-ui loading={$isLoading} count={5} count-gap={8}>
  <div class="founder-row">
    <img src="/avatar.png" width="32" height="32" />
    <span>Placeholder Name</span>
  </div>
</phantom-ui>
```

---

## Fine-Grained Control

| Attribute | Effect |
|-----------|--------|
| `data-shimmer-ignore` | Keep element visible during loading (logos, brand marks) |
| `data-shimmer-no-children` | Capture element as one block (dense metric groups) |
| `data-shimmer-width` | Force measured width (px) |
| `data-shimmer-height` | Force measured height (px) |
| `stagger` | Delay (seconds) between each block's animation |
| `reveal` | Fade-out duration (seconds) when loading ends |

```html
<phantom-ui loading>
  <div class="brand-header" data-shimmer-ignore>Nomad Flow</div>
  <div class="metrics" data-shimmer-no-children>
    <span>$1.2M ARR</span>
    <span>847 Founders</span>
  </div>
  <img src="/chart.png" data-shimmer-width="600" data-shimmer-height="300" />
</phantom-ui>
```

---

## Custom CSS Properties

```css
phantom-ui {
  --shimmer-color: rgba(99, 102, 241, 0.35);
  --shimmer-duration: 1.8s;
  --shimmer-bg: rgba(99, 102, 241, 0.1);
}
```

---

## Bundle Size

| Build | Size |
|-------|------|
| CDN (Lit included) | ~22kb / ~8kb gzipped |
| ES Module (bundler) | ~2kb (Lit assumed in dep tree) |

---

## Acknowledgements

phantom-ui builds on prior art from:
- [page-skeleton-webpack-plugin](https://github.com/ElemeFE/page-skeleton-webpack-plugin) (ElemeFE, 2018)
- [@findify/skeleton-generator](https://github.com/findify/skeleton-generator) (~2019)

Reimagined as a **single universal Web Component** instead of framework-specific adapters.
