---
name: design-mapper-skill
description: >-
  Intelligent design brief router. Takes any design request and emits a structured
  routing plan — which skills to load, in what order, with what focus parameters.
  Always load this skill FIRST when receiving a design brief. It is the entry point
  for the entire modular design system.
  Trigger on: "design a", "create UI for", "branding for", "what should it look like",
  "color scheme for", "design brief", "visual identity", "/design", intent=design_brief.
license: MIT
metadata:
  author: bilioveloso
  version: "1.1.0"
  date: June 2026
  auto_mode: true
trigger_keywords:
  - design a
  - design brief
  - create UI
  - branding for
  - visual identity
  - what should it look like
  - how should it look
  - UI for
  - app design
  - website design
  - /design
  - /mapper
---

# Design Mapper Skill

**Entry point for the modular design system.** Load this first on any design brief.
It reads the brief, detects signals, and emits a routing plan. The agent then loads
each linked skill in the specified order, carrying context forward.

## The Skill Network

```
design-mapper-skill  ← you are here (always load first)
        │
        ├─► color-combo-skill      (palette, mood, roles)
        ├─► design-effects-skill   (morphism, motion, elevation, effects)
        ├─► font-pairing-skill     (typeface, scale, pairing archetype)
        └─► icon-system-skill      (library, weight, style, animated)
```

Skills are independent but the mapper sequences them and passes carryover context
so each skill builds on the choices made before it.

---

## On First Load

When this skill is first loaded in a new session, before processing any design brief:

1. **Announce the system** in one sentence: "I've loaded design-mapper-skill — the entry point for a modular design system."
2. **Offer the companion skills** — ask the user once:

   > "The full system includes 4 companion skills. Want me to install them now?
   > - **color-combo-skill** — 161 palettes, WCAG contrast ratios, CSS custom property templates
   > - **design-effects-skill** — glass morphism, animation, page-type recipes
   > - **font-pairing-skill** — typeface pairing, type scale, loading strategy
   > - **icon-system-skill** — icon library selection, optical sizing, currentColor"

3. **If the user says yes**, run each command:
   ```bash
   npx skills add bilioveloso/color-combo-skill
   npx skills add bilioveloso/design-effects-skill
   npx skills add bilioveloso/font-pairing-skill
   npx skills add bilioveloso/icon-system-skill
   ```
4. **If the user says no**, proceed. The mapper will fetch companion skills from the web when routing — installation is optional.

**Only ask once per session.** If a design brief was already given before the skill loaded, skip the prompt and go straight to routing.

---

## Auto Mode (default on)

After emitting the DESIGN ROUTE block, **automatically load and apply each non-skipped skill in order.** Do not wait for the user to trigger each one manually.

**Source priority for each skill:**
1. Installed locally → read from `~/.claude/skills/<skill-name>/SKILL.md`
2. Not installed → fetch from the raw URL below

| Skill | Raw URL |
|---|---|
| color-combo-skill | `https://raw.githubusercontent.com/bilioveloso/color-combo-skill/main/SKILL.md` |
| design-effects-skill | `https://raw.githubusercontent.com/bilioveloso/design-effects-skill/main/SKILL.md` |
| font-pairing-skill | `https://raw.githubusercontent.com/bilioveloso/font-pairing-skill/main/SKILL.md` |
| icon-system-skill | `https://raw.githubusercontent.com/bilioveloso/icon-system-skill/main/SKILL.md` |

Pass the carryover context from the route when applying each skill (see Carryover Context Rules below).

**To disable auto mode:** If the user says "just route, don't apply" or "route only" — emit the DESIGN ROUTE block and stop. The user can then trigger each skill manually.

---

## How to Use This Skill

When you receive a design brief:

1. **Detect signals** from the brief using the Signal Detectors below.
2. **Resolve the route** using the Routing Table.
3. **Emit the Design Route block** (template at the end).
4. **Auto mode:** load each non-skipped skill in order, reading the carryover notes before each one.
5. **Skip any skill** that has a skip condition (listed per skill below).

Do not ask clarifying questions before emitting the route. Emit the best route you
can from the brief, then offer to adjust.

---

## Signal Detectors

Read the brief for these signals. Multiple signals compound — use the strongest match.

### Industry / Product Signals

| Signal in brief | Primary route |
|---|---|
| dashboard, analytics, metrics, data viz, admin panel | → Corporate / Slate Command → design-effects: flat |
| SaaS, B2B, enterprise, platform, workspace | → Corporate / Slate Command or Steel Platform |
| fintech, banking, finance, investment, wealth | → Corporate / Finance Trust |
| legal, compliance, regulatory, law firm | → Corporate / Legal Anchor |
| devops, infrastructure, monitoring, ops, uptime | → Corporate / Ops Green |
| healthcare, medical, clinical, hospital, pharma | → Healthcare / Clinical Trust |
| wellness, mental health, meditation, therapy | → Healthcare / Wellness Sage |
| gaming, esports, competitive, streaming, twitch | → Gaming / Cyber Arena or Shadow Protocol |
| fashion, luxury brand, jewelry, watch | → Luxury / Golden Obsession or Emerald Dynasty |
| startup, product launch, consumer app | → Luxury Facade / Caramel Air or Blue Horizon |
| e-commerce, retail, shop, marketplace | → Luxury Facade / Citrus Royal or Coastal Contrast |
| restaurant, food, beverage, café, bakery | → Rustic / Amber Workshop |
| craft brewery, spirits, wine, bar | → Rustic / Smoked Oak OR Gothic / Bone & Wine |
| travel, resort, hotel, hospitality | → Warm Tropical / Palm & Sand or Mango Shore |
| eco, sustainable, green, organic, botanical | → Nature / Forest Floor or Deep Canopy |
| music, event, concert, festival | → Acid Contemporary / Ink Volt or Crimson Grid |
| photography, portfolio, creative studio | → Minimalist / Grey Scale Plus or Bone & Carbon |
| science fiction, tech startup, futuristic | → Otherworldly / Vulcanico or Silver Pulse |
| vintage brand, retro revival, heritage | → Retro / Mustard Record or Rust & Cream |
| beauty, cosmetics, skincare, perfume | → Luxury Facade / Soft Prestige OR Gothic / Midnight Bloom |
| kids, education, learning, playful | → Warm Tropical / Mango Shore (high energy) |

### Mood / Aesthetic Signals (override or confirm industry route)

| Signal in brief | Modifier |
|---|---|
| premium, exclusive, expensive, luxury, opulent | → confirm or upgrade to Luxury |
| minimal, clean, whitespace, editorial | → override to Minimalist |
| dark, moody, dramatic, night, noir | → override to Gothic / Dark Romance |
| cyber, neon, futuristic, digital, tech | → override to Otherworldly |
| warm, cozy, artisan, handmade | → override to Rustic / Craft |
| bold, disruptive, street, urban, aggressive | → override to Acid Contemporary |
| soft, calm, airy, gentle, wellness | → override to Soft Gradients or Luxury Facade / Soft Prestige |
| nostalgic, vintage, retro, aged | → override to Retro / Vintage |
| natural, earthy, organic, botanical | → override to Nature / Organic |

### Platform / Context Signals

| Signal | Effect on routing |
|---|---|
| mobile app, iOS, Android | → design-effects: suppress heavy glass, prefer subtle elevation; font-pairing: prefer Dynamic Type–friendly scales |
| web app, SaaS, dashboard | → design-effects: glass OK, elevation-heavy layouts OK |
| landing page, marketing site | → design-effects: hero gradient + scroll motion focus |
| print, poster, packaging | → skip design-effects (motion irrelevant); font-pairing: display-weight emphasis |
| dark mode required | → apply dark variant of chosen palette; design-effects: glass morphism preferred |
| accessibility priority | → override accent choices that fail WCAG AAA; flag in route |
| already has brand colors | → skip color-combo; start at design-effects |
| already has fonts chosen | → skip font-pairing |
| icon-free context (text content, doc) | → skip icon-system |

---

## Skip Conditions

Load only what's needed. Skip a skill when:

| Skill | Skip when |
|---|---|
| color-combo-skill | Brief already specifies exact brand colors |
| design-effects-skill | Platform is print/poster/packaging; or brief is purely typographic |
| font-pairing-skill | Brief already specifies exact typefaces |
| icon-system-skill | No UI components involved (branding only, editorial, print) |

---

## Carryover Context Rules

Each skill builds on the last. Pass these carryover notes when loading each subsequent skill.

### color-combo → design-effects
- **Warm palette** (Rustic, Tropical, Retro) → prefer soft elevation, warm shadows, no cold glass
- **Dark palette** (Gothic, Gaming, Otherworldly) → glass morphism strongly preferred; high-contrast borders
- **Corporate palette** → flat or subtle elevation only; no glass; clean dividers
- **Minimalist palette** → zero decoration; solid fills only; no gradients in components

### color-combo → font-pairing
- **Luxury palette** → serif display or high-contrast didone pairing
- **Minimalist palette** → geometric sans, mono optionally for accents
- **Rustic / Craft palette** → slab serif or humanist serif
- **Corporate palette** → humanist sans (Inter, Source Sans), no decorative
- **Gothic palette** → high-contrast serif or editorial condensed
- **Otherworldly / Gaming** → geometric sans or display mono; avoid humanist

### design-effects → icon-system
- **Glass morphism active** → icons need heavier weight (stroke 2px+) to show through blur
- **Flat design** → icons should be stroke-only, light (1.5px), consistent weight
- **Dark background** → prefer filled or outlined icons with high contrast; avoid 1px strokes
- **Animated effects active** → consider animated icons (Lordicon / Lottie) for key interactive moments

---

## Routing Table (Full)

Quick-reference for common brief types. Resolve from top; first match wins.

| Brief type | color-combo | design-effects | font-pairing | icon-system |
|---|---|---|---|---|
| B2B SaaS dashboard | Corporate / Slate Command | flat, subtle elevation | humanist sans | stroke, medium weight |
| Fintech / banking | Corporate / Finance Trust | flat, no glass | humanist sans, tabular nums | stroke, minimal |
| Healthcare app | Healthcare / Clinical Trust | flat, clean dividers | humanist sans (Inter) | stroke, accessible |
| Wellness / mental health | Healthcare / Wellness Sage | soft elevation, no motion | rounded humanist | soft filled icons |
| Esports team | Gaming / Cyber Arena | glass, neon glow | geometric display | animated, bold filled |
| Luxury brand site | Luxury / Golden Obsession | subtle glass, editorial | didone or high-contrast serif | none or minimal |
| Fashion startup | Luxury Facade / Caramel Air | soft glass, warm shadows | modern humanist serif | minimal stroke |
| Eco / organic brand | Nature / Forest Floor | soft elevation, no glass | slab serif or humanist | nature-themed stroke |
| Coffee / craft brand | Rustic / Amber Workshop | textured, warm shadows | slab serif | hand-drawn or bold stroke |
| Music / event | Acid Contemporary / Ink Volt | flat, bold color blocks | condensed display | minimal or animated |
| Dark editorial / niche | Gothic / Velvet Crypt | glass, dark shadows | editorial serif | none or minimal |
| Resort / travel | Warm Tropical / Mango Shore | warm glass, blur OK | rounded humanist | filled, playful |
| Mobile consumer app | Luxury Facade / Blue Horizon | subtle elevation, no heavy glass | rounded humanist | filled icons, 24px |
| Portfolio / studio | Minimalist / Bone & Carbon | flat, zero decoration | geometric sans + mono | stroke, ultra-light |
| Retro brand | Retro / Mustard Record | flat, textured optionally | slab serif or vintage display | hand-drawn or none |

---

## Output Format

After resolving the route, always emit this block verbatim before loading any skill:

```
╔══════════════════════════════════════╗
║         DESIGN ROUTE                 ║
╚══════════════════════════════════════╝
Brief:    [one-line brief summary]
Platform: [web / mobile / print / unknown]

Step 1 → color-combo-skill
  Category:  [category name]
  Palette:   [palette name, or "agent selects"]
  Note:      [carryover note for next skill]

Step 2 → design-effects-skill
  Focus:     [effect family or "flat"]
  Suppress:  [what to avoid]
  Note:      [carryover note for next skill]

Step 3 → font-pairing-skill
  Archetype: [pairing archetype name]
  Priority:  [display / body / mono emphasis]
  Note:      [carryover note for next skill]

Step 4 → icon-system-skill   [or: SKIP — reason]
  Library:   [preferred library]
  Weight:    [stroke px or filled]
  Style:     [animated / static]

Confidence: [HIGH / MEDIUM / LOW — how well signals matched]
Adjustments: [anything ambiguous that the user should confirm]
```

Then, in auto mode, proceed to load and apply each non-skipped skill immediately.

---

## Example Runs

### Brief: "Design a dashboard for a B2B logistics platform. Dark mode. Needs to feel trustworthy and data-forward."

```
╔══════════════════════════════════════╗
║         DESIGN ROUTE                 ║
╚══════════════════════════════════════╝
Brief:    B2B logistics dashboard, dark mode, trustworthy + data-forward
Platform: web

Step 1 → color-combo-skill
  Category:  Corporate / Enterprise
  Palette:   Slate Command (dark variant)
  Note:      Dark palette — glass morphism preferred in effects step

Step 2 → design-effects-skill
  Focus:     subtle glass morphism on data cards, clean elevation
  Suppress:  warm shadows, decorative motion, heavy gradients
  Note:      Heavy elevation context — icons need 2px+ stroke weight

Step 3 → font-pairing-skill
  Archetype: corporate-clean
  Priority:  body + tabular numerals for data columns
  Note:      Humanist sans only; no serifs in this context

Step 4 → icon-system-skill
  Library:   Lucide or Phosphor
  Weight:    stroke 2px
  Style:     static (dashboard context)

Confidence: HIGH
Adjustments: None — signals aligned well
```

---

### Brief: "Branding for a boutique gin distillery. Should feel premium but a bit dark and mysterious."

```
╔══════════════════════════════════════╗
║         DESIGN ROUTE                 ║
╚══════════════════════════════════════╝
Brief:    Boutique gin distillery — premium, dark, mysterious
Platform: print + web

Step 1 → color-combo-skill
  Category:  Gothic / Dark Romance
  Palette:   Bone & Wine
  Note:      Dark warm palette — glass morphism optional; prefer editorial flat

Step 2 → design-effects-skill
  Focus:     editorial flat with subtle texture; dark shadows
  Suppress:  bright glass, animated motion, neon
  Note:      Minimal icons; serif-heavy type will dominate

Step 3 → font-pairing-skill
  Archetype: editorial-dark
  Priority:  high-contrast display serif + light body
  Note:      Refined palette — icon use should be minimal or none

Step 4 → icon-system-skill   [SKIP — branding/print context; no UI components]

Confidence: HIGH
Adjustments: If web presence includes e-commerce, reload icon-system for cart/nav icons
```

---

### Brief: "Gaming peripheral brand targeting competitive players. Think RGB, dark, aggressive."

```
╔══════════════════════════════════════╗
║         DESIGN ROUTE                 ║
╚══════════════════════════════════════╝
Brief:    Gaming peripheral brand — RGB, dark, competitive, aggressive
Platform: web

Step 1 → color-combo-skill
  Category:  Gaming / Esports
  Palette:   Cyber Arena
  Note:      Dark neon palette — heavy glass morphism in effects step

Step 2 → design-effects-skill
  Focus:     glass morphism, neon glow, RGB border animation
  Suppress:  warm shadows, serif type integration, soft gradients
  Note:      Heavy animated context — icons should be animated/filled

Step 3 → font-pairing-skill
  Archetype: gaming-display
  Priority:  geometric display (headings) + sans body
  Note:      High-contrast bold; no humanist warmth

Step 4 → icon-system-skill
  Library:   Lordicon (animated) or Phosphor (bold filled)
  Weight:    bold filled
  Style:     animated for hero elements, static for nav

Confidence: HIGH
Adjustments: None
```

---

## Maintained by

[@bilioveloso](https://github.com/bilioveloso) — part of the modular design skill system.

**System repos:**
- [color-combo-skill](https://github.com/bilioveloso/color-combo-skill)
- [design-effects-skill](https://github.com/bilioveloso/design-effects-skill)
- [font-pairing-skill](https://github.com/bilioveloso/font-pairing-skill)
- [icon-system-skill](https://github.com/bilioveloso/icon-system-skill)
