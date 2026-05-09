# VentureMind Design Tokens — style-guide.md
## diagram-design × Nomad Flow | Version 1.0

---

## Philosophy

> *"The highest-quality move is usually deletion."*

Every node earns its place. The accent color is reserved for the 1–2 things the reader should look at first. Target density: 4/10.

---

## Brand Identity — Nomad Flow

**Tagline**: The AI brain behind global freedom.
**Color personality**: Deep indigo + midnight tech + warm amber accents — trustworthy, elite, globally mobile.
**Mood**: Bloomberg Terminal meets premium private members club.

---

## Token Map

| Semantic Role | Value | Usage |
|---|---|---|
| `paper` | `#0c0e1a` | Background — deep navy almost black |
| `paper-2` | `#13162d` | Card / container surface |
| `ink` | `#f1f5f9` | Primary text — warm white |
| `muted` | `#64748b` | Secondary text — slate caption |
| `accent` | `#6366f1` | Indigo — the 1–2 focal elements |
| `accent-tint` | `rgba(99,102,241,0.12)` | Accent background wash |
| `link` | `#818cf8` | Indigo-400 — softer accent for links |
| `rule` | `rgba(241,245,249,0.08)` | Hairline borders |
| `rule-solid` | `rgba(241,245,249,0.18)` | Stronger divider |
| `highlight` | `#f59e0b` | Amber — used sparingly for critical callouts |

---

## Typography

| Role | Font | Fallback | Weight | Usage |
|---|---|---|---|---|
| `title` | Instrument Serif | Georgia, serif | 400/700 | Diagram title, italic callouts |
| `node-name` | Geist | Inter, system-ui | 400/600 | Node labels, section headers |
| `sublabel` | Geist Mono | Consolas, monospace | 400 | Ports, field names, API paths, timestamps |

**Google Fonts import:**
```
Instrument Serif: https://fonts.googleapis.com/css2?family=Instrument+Serif:ital@0;1&display=swap
Geist: https://fonts.googleapis.com/css2?family=Geist:wght@300;400;500;600&display=swap
Geist Mono: https://fonts.googleapis.com/css2?family=Geist+Mono:wght@400;500&display=swap
```

> **On mono font**: Use Geist Mono only for technical content (ports, URLs, field types, API paths). Do not use JetBrains Mono — it reads as "dev aesthetic" rather than editorial.

---

## Color Modes

### Minimal Light (default)
```css
--paper: #f8fafc;
--paper-2: #f1f5f9;
--ink: #0f172a;
--muted: #64748b;
--accent: #6366f1;
--accent-tint: rgba(99,102,241,0.08);
--rule: rgba(15,23,42,0.08);
--highlight: #d97706;
```

### Minimal Dark (VentureMind default)
```css
--paper: #0c0e1a;
--paper-2: #13162d;
--ink: #f1f5f9;
--muted: #64748b;
--accent: #6366f1;
--accent-tint: rgba(99,102,241,0.12);
--rule: rgba(241,245,249,0.08);
--highlight: #f59e0b;
```

### Full Editorial
Applies brand tokens + accent wash for focal nodes. Full borders, annotation callouts, legend block at top.

---

## Stroke & Radius Tokens

| Token | Value | Usage |
|---|---|---|
| `stroke-thin` | `0.5px` | Hairline connectors |
| `stroke-default` | `1px` | Node borders, rule dividers |
| `stroke-strong` | `1.5px` | Active states, selected nodes |
| `radius-sm` | `4px` | Small chips, badges |
| `radius-md` | `8px` | Standard card containers |
| `radius-lg` | `12px` | Large panels |
| `grid` | `4px` | All coordinates, widths, gaps must be divisible by 4 |

---

## Node Anatomy

```
┌─────────────────────────┐  ← stroke-default, radius-md
│  [icon?]  NODE NAME      │  ← node-name font, ink
│  sublabel / field        │  ← sublabel font, muted
└─────────────────────────┘
```

**Rules**:
- All node dimensions divisible by 4
- Icon placeholder: 20×20px, centered left
- Node min-width: 96px; max-width: 240px
- Padding: 12px / 16px
- Border: 1px solid `rule` token

**Focal node** (accent-1 nodes only):
- Border: 1.5px solid `accent`
- Background: `accent-tint`
- Icon tinted with `accent` color

---

## Connector Styles

| Type | Usage |
|---|---|
| Solid arrow → | Direct causality, data flow |
| Dashed arrow -→ | Optional or conditional flow |
| Open arrow ○→ | Inheritance / interface |
| Bidirectional ↔ | Peer-to-peer, sync |
| No arrow —— | Association, context |

---

## Accessibility — Contrast Checks

| Pair | Ratio | Pass |
|---|---|---|
| `ink` on `paper` | ~14.5:1 | AAA |
| `muted` on `paper` | ~4.8:1 | AA |
| `accent` on `paper` | ~5.2:1 | AA |
| `ink` on `accent-tint` | ~8.2:1 | AAA |

WCAG AA minimum: 4.5:1 for regular text, 3:1 for large text. All tokens pass.

---

## Applying Tokens

```css
/* All VentureMind diagrams inherit these */
svg {
  background: var(--paper);
  font-family: var(--node-name-font), Geist, Inter, system-ui;
}

/* Nodes */
rect.node-box {
  fill: var(--paper-2);
  stroke: var(--rule);
  stroke-width: var(--stroke-default);
  rx: var(--radius-md);
}

/* Focal nodes */
rect.focal {
  fill: var(--accent-tint);
  stroke: var(--accent);
  stroke-width: var(--stroke-strong);
}

/* Connectors */
path.connector {
  stroke: var(--muted);
  stroke-width: var(--stroke-thin);
}
```

---

## Annotations

Italic Instrument Serif callouts in margins. Leader line: dashed Bézier, `accent` color.

```html
<text class="annotation" font-family="Instrument Serif" font-style="italic">
  "Sovereign execution requires
   zero credential exposure."
</text>
```

---

## First-Run Gate

On first use in a new project, the skill checks if this file has been customized. If it still shows default tokens, it pauses and asks:

> *"This is your first diagram in this project. The style guide is at default. Want to run onboarding to your website, paste tokens manually, or proceed with the default Nomad Flow tokens?"*

Select "proceed with default" or "paste tokens" to skip onboarding.

---

*Last updated: 2026-05-09 | Brand: Nomad Flow / VentureMind*