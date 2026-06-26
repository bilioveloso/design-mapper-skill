# Design Mapper Skill

Intelligent design brief router — the entry point for the modular design skill system.

Reads any design brief and emits a structured **Design Route**: which skills to load, in what order, with what focus parameters and carryover context.

## Always load this first

```
design-mapper-skill  ← start here
        │
        ├─► color-combo-skill      (palette, mood, roles)
        ├─► design-effects-skill   (morphism, motion, elevation)
        ├─► font-pairing-skill     (typeface, scale, pairing)
        └─► icon-system-skill      (library, weight, animated)
```

## What it does

Given a brief like *"Premium cocktail brand. Feels luxurious but also tropical and fun."*, the mapper:

1. **Detects signals** — industry keywords, mood adjectives, platform constraints
2. **Resolves conflicts** — explicit rules for when signals point in different directions
3. **Emits a Design Route block** — structured output specifying category, palette, effects family, font archetype, icon style
4. **Passes carryover context** — each skill's choices inform the next (warm palette → warm shadows → slab serif → organic icons)

## The Design Route output format

```
╔══════════════════════════════════════╗
║         DESIGN ROUTE                 ║
╚══════════════════════════════════════╝
Brief:    [summary]
Platform: [web / mobile / print]

Step 1 → color-combo-skill
  Category:  ...
  Palette:   ...

Step 2 → design-effects-skill
  Focus:     ...
  Suppress:  ...

Step 3 → font-pairing-skill
  Archetype: ...

Step 4 → icon-system-skill  [or: SKIP — reason]
  Library:   ...
  Weight:    ...

Confidence: HIGH / MEDIUM / LOW
Adjustments: ...
```

## What's inside SKILL.md

- **Signal Detectors** — industry, mood, and platform keyword tables
- **Fast Path** — one-line category lookup for quick routing
- **Skip Conditions** — when not to load each skill
- **Conflict Resolution** — 6 tiebreaker rules + common conflict patterns
- **Carryover Context Rules** — what to pass between skills to keep decisions coherent
- **Routing Table** — 18 common brief types mapped to full routes
- **10 example runs** — including edge cases: conflicting signals, accessibility-first, multi-audience

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for how to submit new signal mappings and example runs.

## License

MIT — see [LICENSE](LICENSE).

## Maintained by

[@bilioveloso](https://github.com/bilioveloso)

**Full skill system:**
[color-combo-skill](https://github.com/bilioveloso/color-combo-skill) ·
[design-effects-skill](https://github.com/bilioveloso/design-effects-skill) ·
[font-pairing-skill](https://github.com/bilioveloso/font-pairing-skill) ·
[icon-system-skill](https://github.com/bilioveloso/icon-system-skill)