# DesignCortex Setup

Run once to configure your design system.

## Quick Setup (use a preset)

```
In Claude Code: Using DesignCortex with Zinc preset, set up my project.
```

This loads the Zinc preset tokens and applies all DesignCortex rules automatically.

## Custom Setup

Answer these 6 questions:

1. **Theme:** Light or dark default?
2. **Brand color:** Primary hex value?
3. **Typography:** Display font + body font?
4. **Density:** Spacious, comfortable, or compact?
5. **Border radius:** Sharp (0-2px), subtle (4-6px), rounded (8-12px), or pill (9999px)?
6. **Separation:** Borders, shadows, or background contrast?

```
In Claude Code:
Using DesignCortex, set up my project with:
- Dark theme
- Brand color #3B82F6
- Fonts: Geist Sans + Geist Mono
- Comfortable density
- Subtle radius (4-6px)
- Border separation
```

## Verification

After setup, test with a simple prompt:
```
Using DesignCortex, create a disabled button and a hover state.
```

Check that:
- Disabled uses `opacity: 0.38` (not 0.5)
- Hover uses an 8% overlay (not `opacity: 0.8`)
- Letter-spacing is negative on any heading text
- Focus ring uses `outline-offset: 2px`

If any of these are wrong, prefix your prompt with `Using DesignCortex,` to activate the skill.
