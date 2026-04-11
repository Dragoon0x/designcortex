---
name: designcortex
description: "DesignCortex is a perceptual frontend skill for Claude Code — exact CSS values, lookup tables, and formulas grounded in visual science so Claude stops writing averaged-out frontend code."
version: 1.0.0
---

# DesignCortex

## What This Is

A lookup table for your eyes. Every value here is testable, measurable, and the result of converging evidence from Material Design 3, Apple HIG, Ant Design, Radix, and years of production frontend work. When Claude writes frontend code without these values, it averages what it has seen. That average is wrong.

**Before:** `opacity: 0.5` on disabled buttons, `box-shadow: 0 4px 6px rgba(0,0,0,0.1)` everywhere, `letter-spacing: normal` at all sizes, hover states that are just `opacity: 0.8`.

**After:** Values that match how human eyes actually perceive contrast, depth, weight, and motion.

---

## §1 — Typography Precision

### Letter-Spacing by Size

Large type has open optical counters. Without negative tracking, it looks loose. Small type needs neutral or slightly positive tracking for legibility.

| Font Size | Letter-Spacing | Why |
|-----------|---------------|-----|
| 11-12px | `+0.03em` | Micro text needs air between glyphs |
| 13-14px | `+0.01em` | Small text, slight openness |
| 15-16px | `0` | Body text, neutral |
| 18-20px | `-0.01em` | Lead text, start tightening |
| 24-28px | `-0.015em` | Subheadings |
| 32-40px | `-0.02em` | Headings |
| 48-56px | `-0.025em` | Display small |
| 64-80px | `-0.03em` | Display large |
| 96px+ | `-0.035em` | Hero / statement |

**Rule:** If a font has built-in optical sizing (variable font with `opsz` axis), reduce these adjustments by ~40%.

### Line-Height by Size

As font size increases, line-height ratio decreases. Large text with 1.5 line-height looks like double-spacing.

| Font Size | Line-Height |
|-----------|------------|
| 12px | `1.65` |
| 14px | `1.55` |
| 16px | `1.5` |
| 18px | `1.45` |
| 20-24px | `1.35` |
| 28-32px | `1.25` |
| 40-48px | `1.15` |
| 56px+ | `1.1` |
| 80px+ | `1.05` |

**Rule:** Serif body text needs +0.05 to +0.1 more. Monospace needs +0.05 to +0.1 as well.

### Font-Weight Hierarchy

Use exactly 3 weights. Not 2 (too flat), not 5 (too noisy).

| Role | Weight | When |
|------|--------|------|
| Heavy | `700` | Page titles, primary headings, important numbers |
| Medium | `500` | Subheadings, button labels, active nav, labels |
| Regular | `400` | Body text, descriptions, secondary content |

Exceptions: `600` when `500` lacks contrast against `400` in a specific face. `300` only for display text 48px+ in typefaces designed for light weights. Never for UI text.

### Responsive Type Scale (clamp formulas)

```css
--font-size-body:    clamp(0.938rem, 0.875rem + 0.25vw, 1rem);
--font-size-lead:    clamp(1rem, 0.875rem + 0.5vw, 1.125rem);
--font-size-h3:      clamp(1.125rem, 0.875rem + 1vw, 1.5rem);
--font-size-h2:      clamp(1.25rem, 0.75rem + 2vw, 2rem);
--font-size-h1:      clamp(1.5rem, 0.5rem + 4vw, 3rem);
--font-size-display: clamp(2rem, -0.5rem + 8vw, 4.5rem);
```

### Text Rendering (always apply)

```css
body {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-rendering: optimizeLegibility;
}
```

### Paragraph Spacing

Space above headings > space below. This binds the heading to its content.

```css
h2 { margin-top: 2.5em; margin-bottom: 0.75em; }
h3 { margin-top: 2em; margin-bottom: 0.5em; }
p  { margin-bottom: 1.25em; }
```

### Optimal Reading Width

```css
.prose { max-width: 65ch; }
```

Not `680px`. Characters are the unit of reading comfort. `ch` scales with font-size.

---

## §2 — Color Precision

### State Overlay Opacities

MD3, Radix, and Ant Design converge within ±2% of these values.

| State | Opacity | Notes |
|-------|---------|-------|
| Hover | `0.08` | Below 0.06 invisible on most monitors |
| Focus | `0.12` | Must exceed hover for keyboard distinction |
| Pressed | `0.16` | Clearly different from hover |
| Dragging | `0.20` | Object is "lifted" |
| Selected (bg) | `0.08 - 0.12` | Persistent without noise |
| Disabled | `0.38` | Not 0.5 (competes with active). Not 0.3 (invisible on some monitors). |
| Scrim/Backdrop | `0.48` | Dims without blackout |
| Image text overlay | `0.55 - 0.65` | Passes WCAG for white text on any image |

Implementation:
```css
.interactive { position: relative; }
.interactive:hover::after {
  content: ""; position: absolute; inset: 0;
  background: currentColor; opacity: 0.08;
  pointer-events: none; border-radius: inherit;
}
.disabled { opacity: 0.38; pointer-events: none; }
```

### Dark Mode Text (halation prevention)

Pure white on dark = halation (glowing/bleeding text). Affects ~33% of population (astigmatism).

| Role | Light Mode | Dark Mode |
|------|-----------|-----------|
| Primary | `#0F172A` | `#F1F5F9` |
| Secondary | `#64748B` | `#94A3B8` |
| Disabled | `#94A3B8` | `#475569` |
| Placeholder | `#94A3B8` | `#64748B` |

### Dark Mode Surfaces (not pure black)

| Level | Hex | Usage |
|-------|-----|-------|
| Base | `#09090B` | Page background |
| Surface | `#0F0F12` | Cards |
| Elevated | `#18181B` | Dropdowns, popovers |
| Highest | `#27272A` | Selected, active |
| Border | `#27272A` | Default |
| Border-strong | `#3F3F46` | Emphasis |

Dark mode elevation = lighter surface (no shadows).

### Brand Color Dark Mode

Desaturate and lighten to prevent vibration:
```css
/* Light */ --brand: oklch(55% 0.24 264);
/* Dark */  --brand: oklch(68% 0.20 264);
```
Use `oklch()` for perceptually uniform manipulation.

---

## §3 — Spacing System

### Base-4 Scale

```css
--space-1: 4px;   --space-2: 8px;   --space-3: 12px;  --space-4: 16px;
--space-5: 20px;  --space-6: 24px;  --space-8: 32px;  --space-10: 40px;
--space-12: 48px; --space-16: 64px; --space-20: 80px; --space-24: 96px;
```

### Component Padding Lookup

| Component | Padding (x / y) | Total Height |
|-----------|-----------------|-------------|
| Button sm | `12 / 6` | 32px |
| Button md | `16 / 10` | 40px |
| Button lg | `24 / 14` | 48px |
| Input | `12 / 10` | 40px (matches button md) |
| Card | `24` all | — |
| Card compact | `16` all | — |
| Modal body | `24` all | — |
| Modal header | `24 / 16` | 56px |
| Dropdown item | `12 / 10` | 40px |
| Toast | `16` all | — |
| Table cell | `16 / 12` | — |
| Table cell compact | `12 / 8` | — |

### Section Spacing

| Context | Gap |
|---------|-----|
| Within field group | `8px` |
| Between fields | `20-24px` |
| Between sections | `32-40px` |
| Between page sections | `64-96px` |
| Hero to content | `80-120px` |

### Fluid Spacing

```css
--section-gap: clamp(3rem, 2rem + 4vw, 6rem);
--container-pad: clamp(1rem, 0.5rem + 2vw, 3rem);
```

---

## §4 — Shadow & Elevation

```css
--shadow-xs:  0 1px 2px 0 rgba(0,0,0,0.05);
--shadow-sm:  0 1px 3px 0 rgba(0,0,0,0.10), 0 1px 2px -1px rgba(0,0,0,0.10);
--shadow-md:  0 4px 6px -1px rgba(0,0,0,0.10), 0 2px 4px -2px rgba(0,0,0,0.10);
--shadow-lg:  0 10px 15px -3px rgba(0,0,0,0.10), 0 4px 6px -4px rgba(0,0,0,0.10);
--shadow-xl:  0 20px 25px -5px rgba(0,0,0,0.10), 0 8px 10px -6px rgba(0,0,0,0.10);
--shadow-2xl: 0 25px 50px -12px rgba(0,0,0,0.25);
```

| Element | Level |
|---------|-------|
| Card (resting) | `shadow-sm` or border (no shadow) |
| Card (hover) | `shadow-md` |
| Dropdown | `shadow-lg` |
| Modal | `shadow-xl` |
| Toast | `shadow-lg` |
| Sticky header (scrolled) | `shadow-sm` |

**Dark mode:** Shadows invisible. Use surface lightening instead.

---

## §5 — Motion & Timing

| Interaction | Duration | Easing |
|-------------|----------|--------|
| Button press | `100-150ms` | `ease-out` |
| Toggle | `200ms` | `ease-in-out` |
| Tooltip show | `150ms` | `ease-out` |
| Tooltip hide | `100ms` | `ease-in` |
| Dropdown open | `200ms` | `ease-out` |
| Dropdown close | `150ms` | `ease-in` |
| Modal enter | `250ms` | `cubic-bezier(0.16, 1, 0.3, 1)` |
| Modal exit | `200ms` | `cubic-bezier(0.4, 0, 1, 1)` |
| Page transition | `300ms` | `cubic-bezier(0.4, 0, 0.2, 1)` |
| Skeleton shimmer | `1500ms` | `linear` (infinite) |
| Stagger gap | `50ms` | — |

**Exit rule:** Exits are faster than enters. Always. Objects arriving settle. Objects leaving get out of the way.

```css
--ease-default: cubic-bezier(0.4, 0, 0.2, 1);
--ease-in:      cubic-bezier(0.4, 0, 1, 1);
--ease-out:     cubic-bezier(0, 0, 0.2, 1);
--ease-spring:  cubic-bezier(0.34, 1.56, 0.64, 1);
```

### Reduced Motion (mandatory)

```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
}
```

### Only Animate

✓ `transform`, `opacity`, `clip-path`, `background-color`
✗ `width`, `height`, `top`, `left`, `margin`, `padding`, `border-radius`, `box-shadow`

---

## §6 — Interactive States

### Focus Ring

```css
:focus-visible { outline: 2px solid var(--color-brand); outline-offset: 2px; }
:focus:not(:focus-visible) { outline: none; }
```

2px width. 2px offset. Brand color. Visible on all backgrounds.

### Button States

```css
.btn { transition: background-color 150ms ease-out, transform 100ms ease-out; }
.btn:hover { filter: brightness(0.92); }
.btn:active { transform: scale(0.97); filter: brightness(0.85); }
.btn:disabled { opacity: 0.38; pointer-events: none; }
```

### Input States

| State | Border | Background |
|-------|--------|-----------|
| Default | `#D4D4D8` | surface |
| Hover | `#A1A1AA` | surface |
| Focus | brand color + 2px ring | surface |
| Error | `#EF4444` | `#FEF2F2` |
| Disabled | border at 0.38 | muted |

### Touch Targets

| Context | Minimum |
|---------|---------|
| Mobile buttons | `44 × 44px` |
| Mobile list items | `48px` height |
| Desktop buttons | `32 × 32px` |

Extend tap area without visual change:
```css
.icon-btn { position: relative; width: 24px; height: 24px; }
.icon-btn::before { content: ""; position: absolute; inset: -10px; }
```

---

## §7 — Layout Formulas

### Container Widths

| Content | Max-Width |
|---------|-----------|
| Prose | `65ch` |
| Form single-col | `480-560px` |
| Form two-col | `720-800px` |
| Dashboard | `1200px` |
| Marketing | `1280px` |

### Z-Index System

```css
--z-dropdown: 100;  --z-sticky: 200;    --z-drawer: 300;
--z-modal-bg: 400;  --z-modal: 500;     --z-popover: 600;
--z-toast: 700;     --z-tooltip: 800;
```

Never use arbitrary values. Fix stacking context, don't escalate.

### Aspect Ratios

```css
.hero       { aspect-ratio: 21/9; }
.card-image { aspect-ratio: 3/2; }
.thumbnail  { aspect-ratio: 1/1; }
.video      { aspect-ratio: 16/9; }
```

### Fluid Clamp Formula

For value MIN→MAX across viewport VP_MIN→VP_MAX:
```
clamp(MIN, CALC + Xvw, MAX)
```
Example — 16px→48px across 320px→1280px:
```css
padding: clamp(1rem, 0.333rem + 2.08vw, 3rem);
```

---

## §8 — Anti-Slop Checklist

3+ matches = the output is slop. Start over.

**Typography:** `Inter` with no justification. `letter-spacing: normal` on 24px+ headings. Same weight for headings and body. `line-height: 1.5` on everything. No `max-width` on prose.

**Color:** Disabled at `0.5`. Hover is `opacity: 0.8`. Pure `#FFF` on dark. Pure `#000` dark bg. Full-saturation brand on dark.

**Spacing:** Mixed `16px` and `15px` in same view. Same spacing for sections and components. Heading margin-top ≤ margin-bottom.

**Shadow:** Same shadow on everything. Shadows on dark mode. Shadow + border on same element.

**Motion:** `transition: all 0.3s ease`. No `prefers-reduced-motion`. Same duration enter/exit. Animating `width`/`height`/`margin`.

**Layout:** `z-index: 9999`. No content max-width. Magic breakpoints. `px` fonts with no scaling.

---

## §9 — Quick Reference

### 5 Values Claude Gets Wrong

```
1. Disabled opacity:   0.38    not 0.5
2. Hover overlay:      0.08    not "opacity: 0.8"
3. Dark mode bg:       #09090B not #000000
4. Dark mode text:     #F1F5F9 not #FFFFFF
5. Heading tracking:   -0.02em not normal
```

### 5 CSS Values That Signal Quality

```
1. letter-spacing: -0.02em   (headings — optical awareness)
2. opacity: 0.38             (disabled — system knowledge)
3. outline-offset: 2px       (focus — accessibility care)
4. transition: 150ms ease-out (buttons — motion intent)
5. max-width: 65ch           (prose — reading science)
```

---

## §10 — Presets

```
Using DesignCortex with [preset], build [request].
```

| Preset | Feel | Primary | Fonts |
|--------|------|---------|-------|
| **Zinc** | Dev tool, dark | `#3B82F6` | Geist Sans + Mono |
| **Sand** | Editorial, warm | `#B45309` | Newsreader + Inter |
| **Carbon** | Luxury, dark | `#C9A96E` | Instrument Serif + Libre Franklin |
| **Chalk** | SaaS, clean | `#2563EB` | Plus Jakarta Sans |
| **Moss** | Organic, earthy | `#2D6A4F` | DM Sans + Mono |
| **Neon** | Bold, dark | `#A855F7` | Space Grotesk + Mono |

Custom config: answer 6 questions — theme, brand color, fonts, density, radius, separation method.

---

*DesignCortex by 0xDragoon — MIT License*
*Not opinions. Values. Testable, measurable, perceptually grounded.*
