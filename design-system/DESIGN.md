---
name: NomadFlow
description: VentureMind Nomad Flow design system tokens
version: 1.0.0
colors:
  surface:      "#0D0F14"
  background:   "#070809"
  surface-raised:  "#141620"
  surface-overlay: "#1C1F2E"
  on-surface:   "#F0ECE3"
  on-surface-muted: "#8A877D"
  primary:      "#E8A445"
  primary-hover:  "#C98A20"
  secondary:    "#4A7B5C"
  tertiary:     "#3B6B8C"
  error:        "#C94A4A"
  warning:      "#C9943A"
  success:      "#4A7B5C"
  # Scale variants (generated)
  primary-container: "#2A1F0A"
  on-primary-container: "#F5D98A"
  secondary-container: "#0D1F13"
  on-secondary-container: "#8FD4A4"
  tertiary-container: "#0D1F28"
  on-tertiary-container: "#8ACAEA"
  error-container: "#2A0D0D"
  on-error-container: "#F5AAAA"
typography:
  display-lg:
    fontFamily: "Outfit"
    fontSize: 56px
    fontWeight: 700
    lineHeight: 1.1
    letterSpacing: -0.02em
  display-md:
    fontFamily: "Outfit"
    fontSize: 44px
    fontWeight: 600
    lineHeight: 1.15
    letterSpacing: -0.01em
  headline-lg:
    fontFamily: "Outfit"
    fontSize: 32px
    fontWeight: 600
    lineHeight: 1.25
    letterSpacing: 0
  headline-md:
    fontFamily: "Outfit"
    fontSize: 24px
    fontWeight: 600
    lineHeight: 1.3
    letterSpacing: 0
  title-lg:
    fontFamily: "Cabinet Grotesk"
    fontSize: 20px
    fontWeight: 700
    lineHeight: 1.35
    letterSpacing: 0
  title-md:
    fontFamily: "Cabinet Grotesk"
    fontSize: 16px
    fontWeight: 500
    lineHeight: 1.4
    letterSpacing: 0.01em
  body-lg:
    fontFamily: "Cabinet Grotesk"
    fontSize: 16px
    fontWeight: 400
    lineHeight: 1.6
    letterSpacing: 0
  body-md:
    fontFamily: "Cabinet Grotesk"
    fontSize: 14px
    fontWeight: 400
    lineHeight: 1.55
    letterSpacing: 0
  label-sm:
    fontFamily: "Cabinet Grotesk"
    fontSize: 11px
    fontWeight: 500
    lineHeight: 1.4
    letterSpacing: 0.06em
    textTransform: uppercase
  label-md:
    fontFamily: "Cabinet Grotesk"
    fontSize: 13px
    fontWeight: 500
    lineHeight: 1.4
    letterSpacing: 0.03em
rounded:
  none: 0px
  sm: 6px
  md: 10px
  lg: 16px
  xl: 24px
  full: 9999px
spacing:
  unit: 4px
  xs: 4px
  sm: 8px
  md: 16px
  lg: 24px
  xl: 40px
  xxl: 64px
  container-padding: 24px
  card-gap: 16px
  section-margin: 48px
components:
  button-primary:
    background: "{primary}"
    color: "#070809"
    font: "{title-md}"
    borderRadius: "{rounded.md}"
    padding: "12px 24px"
  button-primary-hover:
    background: "{primary-hover}"
  button-ghost:
    background: transparent
    color: "{on-surface}"
    border: "1px solid {surface-raised}"
    borderRadius: "{rounded.md}"
    padding: "12px 24px"
  input-field:
    background: "{surface-raised}"
    color: "{on-surface}"
    border: "1px solid {surface-overlay}"
    borderRadius: "{rounded.md}"
    padding: "10px 16px"
    font: "{body-md}"
  card-standard:
    background: "{surface-raised}"
    borderRadius: "{rounded.lg}"
    padding: 24px
    border: "1px solid {surface-overlay}"
  card-elevated:
    background: "{surface-overlay}"
    borderRadius: "{rounded.lg}"
    padding: 24px
    boxShadow: "0 8px 32px rgba(0,0,0,0.4)"
  glass-card:
    background: "rgba(28,31,46,0.7)"
    backdropFilter: "blur(20px)"
    borderRadius: "{rounded.lg}"
    border: "1px solid rgba(240,236,227,0.08)"
    padding: 24px
motion:
  duration-fast: 150ms
  duration-normal: 250ms
  duration-slow: 400ms
  easing-standard: "cubic-bezier(0.4, 0, 0.2, 1)"
  easing-decelerate: "cubic-bezier(0, 0, 0.2, 1)"
  easing-accelerate: "cubic-bezier(0.4, 0, 1, 1)"
---

# VentureMind Nomad Flow — Design Language

**Version:** 1.0 | **Agent:** design-intelligence-agent | **Last updated:** 2026-05-09

## Design Philosophy

Nomad Flow is built for nomadic founders who move fast. The interface is calm, premium, and focused — never overwhelming. Every screen answers one question. Noise is banned.

**Core aesthetic:** Dark intelligence. Deep charcoal surfaces with warm amber accents. Glass-morphism where depth matters. Zero clutter.

## Color Rationale

| Token | Hex | Role |
|---|---|---|
| Background | `#070809` | Deepest base — never pure black |
| Surface | `#0D0F14` | Card backgrounds |
| Surface-raised | `#141620` | Elevated elements, inputs |
| Surface-overlay | `#1C1F2E` | Borders, dividers |
| On-surface | `#F0ECE3` | Primary text — warm off-white |
| On-surface-muted | `#8A877D` | Secondary text |
| Primary | `#E8A445` | Amber gold — CTAs, active states |
| Secondary | `#4A7B5C` | Success, positive signals |
| Tertiary | `#3B6B8C` | Info, links |
| Error | `#C94A4A` | Errors only |

**Banned:** Pure black backgrounds, neon blues/purples, Inter font, generic AI purple gradients.

## Typography Rules

- **Display/Headlines:** Outfit — geometric, premium, distinctive
- **Body/Labels:** Cabinet Grotesk — warm, readable, premium
- **Never:** Inter, Roboto, system-ui defaults
- **Line length:** Body max ~65 chars. Never full-width paragraphs.
- **Tracking:** Headlines slightly negative; labels slightly positive

## Spacing & Grid

- **Base unit:** 4px
- **Container padding:** 24px
- **Card gap:** 16px
- **Section margin:** 48px
- **Grid:** 12-column, fluid. Breakpoints: 640 / 1024 / 1440

## Motion Philosophy

- **Fast interactions:** 150ms — hover states, toggles
- **Normal transitions:** 250ms — panels, modals
- **Slow reveals:** 400ms — page transitions, data loading
- **Easing:** Always custom cubic-bezier. Never linear.
- **Perpetual micro-interactions:** Subtle on all interactive elements

## Component States

All interactive elements MUST have: default → hover → active → disabled → loading states. No element is complete without all five.

## Anti-Patterns (Explicitly Banned)

- Generic AI purple/blue gradients as backgrounds
- Card walls (too many cards on one screen)
- Inter font anywhere
- Pure black (`#000000`) backgrounds
- Linear animations
- Full-width text paragraphs
- Cluttered data tables without hierarchy