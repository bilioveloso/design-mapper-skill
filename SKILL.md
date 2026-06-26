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
  version: "1.0.0"
  date: June 2026
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

## How to Use This Skill

When you receive a design brief:

1. **Detect signals** from the brief using the Signal Detectors below.
2. **Resolve the route** using the Routing Table.
3. **Emit the Design Route block** (template at the end).
4. **Load each skill in order**, reading the carryover notes before invoking each one.
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

## Fast Path

One-line route for when you need a category instantly. No full analysis — match the strongest signal and go. Use the full Routing Table when the brief has multiple signals or needs effects + font routing.

| Strongest signal | Category |
|---|---|
| "premium" / "luxury" / "exclusive" | **Luxury** |
| "aspirational" / "polished" / "startup" | **Luxury Facade** |
| "soft" / "calm" / "wellness" | **Soft Gradients** |
| "cyber" / "neon" / "futuristic" | **Otherworldly** |
| "bold" / "street" / "disruptive" | **Acid Contemporary** |
| "corporate" / "SaaS" / "B2B" | **Corporate** |
| "finance" / "bank" / "investment" | **Corporate / Finance Trust** |
| "medical" / "healthcare" / "clinical" | **Healthcare** |
| "gaming" / "esports" / "stream" | **Gaming** |
| "dark" / "moody" / "gothic" | **Gothic** |
| "eco" / "organic" / "natural" | **Nature** |
| "tropical" / "resort" / "holiday" | **Warm Tropical** |
| "vintage" / "retro" / "heritage" | **Retro** |
| "minimal" / "clean" / "whitespace" | **Minimalist** |
| "craft" / "artisan" / "handmade" | **Rustic** |
| "replace black" / "replace white" | **Refined Defaults** |
| spring / summer / autumn / winter | **Seasonal** |

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

## Conflict Resolution

When signals point in different directions, apply these tiebreakers in order.

### Rule 1: Explicit beats implicit
If the brief explicitly names a style ("it should feel very dark and moody"), that overrides industry inference. Industry signals are default assumptions; explicit style instructions take precedence.

### Rule 2: Mood overrides industry when 2+ mood signals present
- 1 mood signal + industry signal → blend (use industry category, apply mood as modifier)
- 2+ mood signals + industry signal → mood wins

*Example:* "B2B dashboard, but dark and moody" → 1 mood signal → stay Corporate, note dark mode. "B2B dashboard, but dark, edgy, and disruptive" → 2 mood signals → shift to Gothic / Iron Fog or Gaming / Shadow Protocol.

### Rule 3: Platform suppresses effects, not colors
Mobile or print signals affect only the design-effects routing step. They never override the color category.

### Rule 4: Accessibility-first overrides accent choices
If the brief includes "accessibility", "WCAG AA", "inclusive", or "government", flag any accent scoring below 4.5:1 on its primary surface and substitute the highest-contrast Supporting color instead.

### Rule 5: Luxury + playful = Luxury Facade
Never combine Luxury and Acid Contemporary. The conflict resolves to Luxury Facade, which holds the tension between premium and approachable.

### Rule 6: When truly ambiguous, emit two routes
If signals are roughly equal, emit Route A and Route B, flag `Confidence: MEDIUM`, and let the user choose.

### Common conflict patterns

| Conflict | Resolution |
|---|---|
| Corporate + Dark | Corporate palette, dark mode adaptation |
| Luxury + Playful | Luxury Facade / Caramel Air or Citrus Royal |
| Healthcare + Dark | Healthcare palette, "dim mode" only (max bg #1A2A3A) |
| Gaming + Corporate | Steel Platform Accent swapped for Cyber Arena neon |
| Minimalist + Warm | Warm Mono or Soft Prestige / Terracotta Bloom |
| Nature + Premium | Soft Prestige / Forest Moss × Vanilla Silk |
| Retro + Luxury | Luxury / Emerald Dynasty with desaturated period-correct secondaries |

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

### Brief: "Wellness app for millennials. Calming, modern, not generic spa vibes."

```
╔══════════════════════════════════════╗
║         DESIGN ROUTE                 ║
╚══════════════════════════════════════╝
Brief:    Wellness app, millennial audience, calming but modern — not generic spa
Platform: mobile

Step 1 → color-combo-skill
  Category:  Soft Gradients + Luxury Facade / Soft Prestige
  Palette:   Dreamy Periwinkle gradient + Arctic Teal × Cloud Pearl surfaces
  Note:      Soft cool palette — avoid warm glass; prefer clean card elevation

Step 2 → design-effects-skill
  Focus:     subtle elevation on cards, soft shadows, no glass morphism
  Suppress:  heavy blur, warm tones, decorative motion
  Note:      Rounded humanist font will complement the soft palette well

Step 3 → font-pairing-skill
  Archetype: rounded-humanist
  Priority:  body readability, Dynamic Type support
  Note:      Soft icon style to match

Step 4 → icon-system-skill
  Library:   Phosphor (regular weight) or Lucide
  Weight:    stroke 1.5px
  Style:     static

Confidence: HIGH
Adjustments: If client wants "not generic" to mean distinctive — shift palette toward Indigo Night × Wisteria Glow for more personality
```

---

### Brief: "Law firm rebrand. Prestigious, trustworthy, modernised — less stuffy than the old identity."

```
╔══════════════════════════════════════╗
║         DESIGN ROUTE                 ║
╚══════════════════════════════════════╝
Brief:    Law firm rebrand — prestigious, trustworthy, modernised
Platform: web + print

Step 1 → color-combo-skill
  Category:  Corporate / Legal Anchor
  Palette:   Legal Anchor
  Note:      Warm-neutral palette — editorial flat only; print-safe colours

Step 2 → design-effects-skill
  Focus:     flat, clean dividers, editorial white space
  Suppress:  glass morphism, motion, gradients in body
  Note:      High-contrast serif pairing will drive the "prestigious" signal

Step 3 → font-pairing-skill
  Archetype: editorial-authority
  Priority:  high-contrast display serif + humanist body
  Note:      No animated icons; minimal icon use

Step 4 → icon-system-skill   [SKIP — print + formal web context; no UI components]

Confidence: HIGH
Adjustments: If digital product (client portal) is in scope, reload icon-system for navigation
```

---

### Brief: "Premium cocktail brand. Wants to feel luxurious but also tropical and fun."

```
╔══════════════════════════════════════╗
║         DESIGN ROUTE                 ║
╚══════════════════════════════════════╝
Brief:    Premium cocktail brand — luxury + tropical, fun but not cheap
Platform: web + packaging

⚠️ Conflict detected: Luxury (premium) vs Warm Tropical (fun)
Applying Rule 5 variant: Luxury Facade resolves luxury/approachable tension

Step 1 → color-combo-skill
  Category:  Luxury Facade / Citrus Royal
  Palette:   Citrus Royal (bold premium) + Sunset Punch gradient as hero accent
  Note:      High-energy palette — glass morphism with warm shadows will amplify luxury feel

Step 2 → design-effects-skill
  Focus:     warm glass morphism, editorial layout, hero gradient sections
  Suppress:  cold shadows, corporate flatness
  Note:      Display serif with tropical warmth; no geometric sans

Step 3 → font-pairing-skill
  Archetype: display-editorial
  Priority:  bold display heading + light body
  Note:      Filled playful icons match the energy

Step 4 → icon-system-skill
  Library:   Phosphor (bold) or custom illustrated
  Weight:    bold filled
  Style:     static (packaging) / animated for web hero

Confidence: MEDIUM
Adjustments: If "luxury" is the stronger signal (high price point, exclusive distribution) → shift to Luxury / Emerald Dynasty instead
```

---

### Brief: "Public sector website. Must be accessible to everyone. Government services."

```
╔══════════════════════════════════════╗
║         DESIGN ROUTE                 ║
╚══════════════════════════════════════╝
Brief:    Government services website — must be WCAG AA compliant, inclusive
Platform: web

⚠️ Accessibility override active: all text-facing accents must meet 4.5:1 minimum

Step 1 → color-combo-skill
  Category:  Corporate / Finance Trust
  Palette:   Finance Trust (high-contrast navy + white)
  Note:      Override — verify every colour pair meets WCAG AA before applying.
             Accent (#005BBB on #FFFFFF = 7.2:1 ✅), Secondary on Primary (16.8:1 ✅)

Step 2 → design-effects-skill
  Focus:     flat, high-contrast, maximum legibility
  Suppress:  glass, animation, decorative gradients, anything that reduces contrast
  Note:      Accessible humanist sans only; no display faces

Step 3 → font-pairing-skill
  Archetype: accessible-government
  Priority:  body legibility, large print compatibility, Dynamic Type support
  Note:      Icons need text labels — never icon-only in this context

Step 4 → icon-system-skill
  Library:   Lucide or GOV.UK-style icons
  Weight:    stroke 2px
  Style:     static; every icon must have visible text label

Confidence: HIGH
Adjustments: None — signals are unambiguous
```

---

### Brief: "Indie game studio portfolio. Small team, pixel art games, quirky personality."

```
╔══════════════════════════════════════╗
║         DESIGN ROUTE                 ║
╚══════════════════════════════════════╝
Brief:    Indie game studio — pixel art, quirky, small team personality
Platform: web

Step 1 → color-combo-skill
  Category:  Retro / Vintage + Otherworldly accent
  Palette:   Avocado Dream base + Silver Pulse (#2BEE34) as accent surprise
  Note:      Retro-warm base with one neon accent — unexpected pairing suits indie quirk

Step 2 → design-effects-skill
  Focus:     flat with intentional pixel/retro texture; one neon glow on hero element
  Suppress:  corporate clean, glass morphism, smooth gradients
  Note:      Pixel or slab font; no humanist sans

Step 3 → font-pairing-skill
  Archetype: retro-pixel
  Priority:  display character + readable body
  Note:      Animated pixel-style icons match well

Step 4 → icon-system-skill
  Library:   custom pixel icons or Game Icons (game-icons.net)
  Weight:    bold, chunky
  Style:     animated pixel where possible

Confidence: MEDIUM
Adjustments: If portfolio is more commercial (seeking publisher deals), shift base to Minimalist / Bone & Carbon for professionalism
```

---

### Brief: "Sustainable fashion brand. Premium but ethical. Earth-conscious, not crunchy."

```
╔══════════════════════════════════════╗
║         DESIGN ROUTE                 ║
╚══════════════════════════════════════╝
Brief:    Sustainable fashion — premium + ethical, earth-conscious but not hippie
Platform: web + print

⚠️ Tension: Nature/Organic (eco) vs Luxury (premium)
Resolution: Luxury Facade / Soft Prestige resolves premium/organic tension

Step 1 → color-combo-skill
  Category:  Luxury Facade / Soft Prestige
  Palette:   Forest Moss × Vanilla Silk
  Note:      Organic warmth with premium restraint — flat editorial preferred

Step 2 → design-effects-skill
  Focus:     editorial flat, generous white space, linen/paper texture optionally
  Suppress:  glass, neon, tech-adjacent effects
  Note:      Serif with organic warmth; no geometric sans

Step 3 → font-pairing-skill
  Archetype: editorial-organic
  Priority:  display serif + humanist body
  Note:      Stroke icons, nature-adjacent

Step 4 → icon-system-skill
  Library:   Phosphor (light) or Feather
  Weight:    stroke 1.5px
  Style:     static

Confidence: HIGH
Adjustments: If price point is very high (€500+ garments) → upgrade to Luxury / Emerald Dynasty
```

---

### Brief: "Educational platform for children aged 6–10. Fun, safe, encouraging."

```
╔══════════════════════════════════════╗
║         DESIGN ROUTE                 ║
╚══════════════════════════════════════╝
Brief:    Children's education platform — fun, safe, encouraging, age 6-10
Platform: mobile + web (tablet-primary)

⚠️ Accessibility override: children's UI must meet WCAG AA minimum on all interactive elements

Step 1 → color-combo-skill
  Category:  Warm Tropical / Mango Shore
  Palette:   Mango Shore
  Note:      Bright, joyful, safe — large touch targets need high-contrast borders

Step 2 → design-effects-skill
  Focus:     soft elevation, friendly rounded corners, celebratory micro-animations
  Suppress:  glass morphism, dark shadows, complex gradients
  Note:      Rounded friendly font; large scale

Step 3 → font-pairing-skill
  Archetype: friendly-rounded
  Priority:  large body size (18px+), high legibility, rounded letterforms
  Note:      Filled colourful icons; animated for rewards and feedback

Step 4 → icon-system-skill
  Library:   Lordicon (animated) or custom illustrated
  Weight:    bold filled, high contrast
  Style:     animated for reward moments, static for navigation

Confidence: HIGH
Adjustments: If platform includes teacher/admin view → load a second route using Corporate / Ops Green for the admin UI
```

---

## Maintained by

[@bilioveloso](https://github.com/bilioveloso) — part of the modular design skill system.

**System repos:**
- [color-combo-skill](https://github.com/bilioveloso/color-combo-skill)
- [design-effects-skill](https://github.com/bilioveloso/design-effects-skill)
- [font-pairing-skill](https://github.com/bilioveloso/font-pairing-skill)
- [icon-system-skill](https://github.com/bilioveloso/icon-system-skill)
