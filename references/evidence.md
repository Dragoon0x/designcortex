# Evidence Sources

Every value in DesignCortex is grounded in at least one of these sources.

## Design Systems (convergent values)

| System | Source | Key Contribution |
|--------|--------|-----------------|
| Material Design 3 | Google | State opacities, elevation model, motion curves |
| Apple HIG | Apple | Touch targets (44px), typography scale, SF Pro metrics |
| Ant Design | Alibaba | Component spacing, form patterns, table density |
| Radix UI | WorkOS | State overlays, focus management, accessibility patterns |
| Carbon | IBM | Type scale ratios, spacing tokens, grid system |
| Primer | GitHub | Dark mode surfaces, color scales, component APIs |
| Lightning | Salesforce | Enterprise density, data table patterns |

## Research

| Topic | Finding | Source |
|-------|---------|--------|
| Halation | Pure white text on black causes glow/bleed for ~33% of users | Arditi & Cho, 2005 (Lighthouse International) |
| Optimal line length | 55-65 CPL produces fastest reading with highest comprehension | Dyson & Kipping, 1998 (Visible Language) |
| Disabled opacity | 38-40% converged across MD3, Workday, Salesforce after user testing | MD3 spec; Workday Canvas tokens |
| Touch targets | Below 44px increases error rate 20%+ on mobile | MIT Touch Lab; Apple HIG; WCAG 2.5.8 |
| Hover threshold | Below 6% lightness delta is imperceptible on consumer monitors | sRGB gamut testing; MD3 state layer spec |
| Letter-spacing | Negative tracking needed at display sizes due to optical counter expansion | Robert Bringhurst, Elements of Typographic Style |
| Line-height ratio | Decreasing ratio with size prevents appearance of double-spacing at display sizes | Butterick's Practical Typography |
| Animation duration | 200-500ms range matches human attention capture/release cycle | Nielsen Norman Group; MD3 motion spec |
| Exit < Enter | Faster exits reduce perceived latency and respect user intent to dismiss | MD3 motion spec; Apple HIG motion |
| Reduced motion | 20-25% of users report motion sensitivity | Vestibular Disorders Association; WCAG 2.3.3 |

## Convergence Methodology

When multiple design systems independently arrive at the same value, that value is more likely correct than any single system's opinion. DesignCortex prioritizes values where 3+ systems converge.

Example: Disabled opacity
- MD3: 0.38
- Radix: 0.35-0.40 range
- Ant Design: 0.25 (outlier — too low)
- Workday Canvas: 0.40
- Carbon: 0.25 (outlier — too low)

Convergence: 0.38 (MD3 + Workday, supported by Radix range). Ant and Carbon are outliers — their disabled states are too subtle on mid-range monitors.
