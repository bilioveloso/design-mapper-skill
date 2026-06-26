# Contributing to design-mapper-skill

Contributions are welcome. All PRs are reviewed and merged by [@bilioveloso](https://github.com/bilioveloso).

## What you can contribute

- New rows in the Industry / Product Signals table
- New rows in the Routing Table
- New carryover context rules
- New example runs (Brief → Design Route output)
- Corrections to existing signal mappings

## Submission format

### New signal mapping

```
| [signal keywords] | [Category / Palette] |
```

Must include a note on why this signal maps to that category — add it as a comment in the PR.

### New example run

Must follow the exact `DESIGN ROUTE` block format:

```
### Brief: "[brief description]"

` ` `
╔══════════════════════════════════════╗
║         DESIGN ROUTE                 ║
╚══════════════════════════════════════╝
Brief:    ...
Platform: ...

Step 1 → color-combo-skill
  ...

Step 2 → design-effects-skill
  ...

Step 3 → font-pairing-skill
  ...

Step 4 → icon-system-skill  [or SKIP]
  ...

Confidence: HIGH / MEDIUM / LOW
Adjustments: ...
` ` `
```

## Rules

- Signal keywords must be common natural language terms — no jargon
- Each routing decision must be traceable to a signal in the Signal Detectors section
- Do not change existing signal → route mappings without opening an issue first
- Example runs must be realistic briefs, not toy examples

## Process

1. Fork the repo
2. Make your change to `SKILL.md`
3. Open a PR with the title format: `feat(signals): Add [industry/mood] mapping` or `feat(examples): Add [brief type] example`
4. Wait for review — @bilioveloso will approve or request changes