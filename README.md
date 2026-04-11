# DesignCortex

**Install once. Claude now writes `letter-spacing: -0.02em` on 32px headings instead of leaving it at default.**

That specificity — defensible, tested, perceptually grounded — is what separates production frontend code from AI slop. DesignCortex embeds those rules directly into Claude Code, so every session uses the CSS values a senior frontend engineer carries in their head.

Now Claude knows:

- Disabled at **0.38 opacity, not 0.5** — MD3 and Workday both converged here; 0.5 competes with active elements
- Hover states need a **0.08 overlay**, not `opacity: 0.8` on the element — one is a state layer, the other destroys the element
- Dark mode text: **#F1F5F9, not #FFFFFF** — pure white causes halation for ~33% of users (astigmatism)
- Headings at 32px+: **-0.02em letter-spacing** — optical counters open up at large sizes
- Line-height at 48px: **1.15, not 1.5** — large text with 1.5 looks double-spaced
- Exits faster than enters: **200ms out, 250ms in** — objects leaving should get out of the way

Ten lookup table sections. Six preset design systems. One anti-slop checklist. One `git clone`.

---

## Quick Start

**Step 1 — Install:**

```bash
# Global — every project
git clone https://github.com/0xDragoon/designcortex ~/.claude/skills/designcortex

# Per-project — this repo only
git clone https://github.com/0xDragoon/designcortex .claude/skills/designcortex
```

**Step 2 — Use it:**

```
Using DesignCortex, build a settings page with a form and sidebar navigation.
```

Or with a preset:

```
Using DesignCortex with Zinc preset, build a dashboard with KPI cards and a data table.
```

That's it.

---

## What's Inside

### SKILL.md — 10 Sections of Exact Values

| Section | What It Covers |
|---------|---------------|
| §1 Typography | Letter-spacing by size, line-height by size, weight hierarchy, responsive clamp() formulas, text rendering, paragraph spacing, reading width |
| §2 Color | State overlay opacities (hover/focus/pressed/disabled), dark mode text, dark mode surfaces, brand color adaptation |
| §3 Spacing | Base-4 scale, component padding lookup table, section spacing, gap vs margin decision tree, fluid spacing |
| §4 Shadow | 6-level shadow scale, element-to-shadow mapping, dark mode shadow strategy, border vs shadow decision |
| §5 Motion | Duration by interaction type, named easing curves, exit rule, reduced motion, animatable properties |
| §6 States | Focus ring spec, button state CSS, input state table, touch target sizes, tap area extension |
| §7 Layout | Container widths by content type, z-index system, aspect ratios, breakpoints, clamp formula |
| §8 Anti-Slop | 25-point checklist to detect averaged-out AI output |
| §9 Quick Ref | The 5 values Claude gets wrong most, the 5 values that signal quality |
| §10 Presets | 6 design system configurations |

### Presets

| Preset | Feel | Primary | Fonts |
|--------|------|---------|-------|
| **Zinc** | Developer tool, dark | `#3B82F6` | Geist Sans + Geist Mono |
| **Sand** | Warm editorial, light | `#B45309` | Newsreader + Inter |
| **Carbon** | Premium luxury, dark | `#C9A96E` | Instrument Serif + Libre Franklin |
| **Chalk** | Clean SaaS, light | `#2563EB` | Plus Jakarta Sans + JetBrains Mono |
| **Moss** | Organic earthy, light | `#2D6A4F` | DM Sans + DM Mono |
| **Neon** | Bold high-energy, dark | `#A855F7` | Space Grotesk + Space Mono |

---

## How It's Different from PencilPlaybook

PencilPlaybook gives Claude perceptual values for **Pencil.dev** (a design tool with MCP commands, scaffold scripts, canvas workflows).

DesignCortex gives Claude perceptual values for **frontend code output** (CSS properties, React components, HTML structure). Same ideology — exact, defensible, testable values. Completely different application layer.

PencilPlaybook says: "Set disabled opacity to 40% in the Pencil design tool."
DesignCortex says: "Write `opacity: 0.38` in the CSS. Here's the implementation pattern."

They complement each other. Use PencilPlaybook for design tool work. Use DesignCortex for code.

---

## File Structure

```
designcortex/
├── SKILL.md                          # The core — all lookup tables in one file
├── .claude/skills/designcortex/
│   └── SKILL.md                      # Same file, Claude Code auto-discovery path
├── presets/
│   ├── zinc.json                     # Dev tool dark
│   ├── sand.json                     # Editorial warm
│   ├── carbon.json                   # Luxury dark
│   ├── chalk.json                    # SaaS clean
│   ├── moss.json                     # Organic earthy
│   └── neon.json                     # Bold dark
├── references/
│   └── evidence.md                   # Source evidence for every value
├── setup.md                          # Setup wizard instructions
├── CHANGELOG.md
├── LICENSE                           # MIT
└── README.md
```

---

## The Values Claude Gets Wrong Most Often

| What | Claude Default | DesignCortex |
|------|---------------|-------------|
| Disabled opacity | `0.5` | `0.38` |
| Hover state | `opacity: 0.8` on element | `0.08` overlay via pseudo-element |
| Dark background | `#000000` | `#09090B` |
| Dark text | `#FFFFFF` | `#F1F5F9` |
| Heading tracking | `normal` | `-0.02em` at 32px |
| Line-height 48px | `1.5` | `1.15` |
| Transition | `all 0.3s ease` | `[property] 150ms ease-out` |
| Focus ring | browser default | `2px solid brand, offset 2px` |
| Modal exit | same as enter | 50ms faster, ease-in |
| Reduced motion | not handled | always handled |

---

## Contributing

Issues and PRs welcome. Useful contributions:

- Additional evidence for existing values (research papers, design system audits)
- Values from design systems not yet surveyed (Shopify Polaris, Atlassian, Spectrum)
- New presets with distinct aesthetic personalities
- Edge case documentation (RTL typography, CJK line-height, variable font specifics)

Every value must be defensible. "I think it looks better" is not evidence. "MD3 uses 0.38, Workday uses 0.40, user testing showed 0.5 competes with active states" is evidence.

---

## License

MIT — see [LICENSE](LICENSE).

---

**By [Dragoon0x](https://github.com/Dragoon0x). Star if your Claude output improves. Read the evidence. Ship with intention.**
