# Design System Reference
> Layer 1 · Foundation · Update this file whenever the DS changes

**DS Version:** 1.2.0
**Updated:** 2026-06-11

> ⚠️ This file is written for **humans** — full detail and rationale.
> For AI, use `agent-context.md` instead.
>
> 📌 All values in this file are the **official baseline the team uses** — not examples.
> Any change must go through the approval flow in `governance.md`,
> then update: this file → `agent-context.md` → `changelog.md` → token pipeline → `CLAUDE.md` in every repo.

---

## 1. Design Tokens

### 1.1 Colors

```
color.primary.50   = #EFF6FF
color.primary.100  = #DBEAFE
color.primary.500  = #3B82F6   ← default
color.primary.600  = #2563EB   ← hover
color.primary.900  = #1E3A8A

color.error.500    = #EF4444
color.warning.500  = #F59E0B
color.success.500  = #22C55E

color.neutral.0    = #FFFFFF
color.neutral.50   = #F9FAFB
color.neutral.100  = #F3F4F6
color.neutral.200  = #E5E7EB
color.neutral.500  = #6B7280
color.neutral.900  = #111827
color.neutral.1000 = #000000
```

**Color usage rules:**
- Use only the tokens above — never hardcode hex in Figma or code
- Primary 500 = default interactive elements
- Neutral 900 = body text, Neutral 500 = secondary text
- Neutral 100 = default border, Neutral 200 = strong border
- Error/Warning/Success are for semantic contexts only

**Contrast notes (WCAG AA):**
- neutral.900 on neutral.0 = 17.4:1 ✅
- neutral.500 on neutral.0 = 4.6:1 ✅ (barely passes — never use with fonts smaller than 12px)
- primary.500 on neutral.0 = 3.7:1 ⚠️ large text (≥18px) and UI components only — never body text
- neutral.0 on primary.500 = 3.7:1 ⚠️ primary buttons must use font.weight.semibold or heavier at size ≥ 14px

---

### 1.2 Typography

```
font.family.display  = "Inter"          ← headings
font.family.body     = "Inter"          ← body text
font.family.mono     = "JetBrains Mono" ← code

font.size.xs    = 12px  / 0.75rem
font.size.sm    = 14px  / 0.875rem
font.size.base  = 16px  / 1rem
font.size.lg    = 18px  / 1.125rem
font.size.xl    = 20px  / 1.25rem
font.size.2xl   = 24px  / 1.5rem
font.size.3xl   = 30px  / 1.875rem
font.size.4xl   = 36px  / 2.25rem

font.weight.regular  = 400
font.weight.medium   = 500
font.weight.semibold = 600
font.weight.bold     = 700

line.height.tight   = 1.25
line.height.normal  = 1.5
line.height.relaxed = 1.75
```

**Thai/Lao note:** Inter covers Latin only — for Thai/Lao UI, set a font fallback stack:
`"Inter", "Noto Sans Thai", "Noto Sans Lao", sans-serif`
and verify line-height against upper/lower vowel marks (Thai/Lao always need line-height ≥ normal=1.5; never use tight for body text).

---

### 1.3 Spacing

```
Base-4 system:
space.1  = 4px
space.2  = 8px
space.3  = 12px
space.4  = 16px
space.5  = 20px
space.6  = 24px
space.8  = 32px
space.10 = 40px
space.12 = 48px
space.16 = 64px
space.20 = 80px
space.24 = 96px
```

**Rule:** Use spacing from this scale only — never use other values such as 13px or 22px.

---

### 1.4 Border Radius

```
radius.none  = 0px
radius.sm    = 4px
radius.md    = 8px    ← default
radius.lg    = 12px
radius.xl    = 16px
radius.2xl   = 24px
radius.full  = 9999px ← pills, avatars
```

---

### 1.5 Shadow / Elevation

```
shadow.sm  = 0 1px 2px rgba(0,0,0,0.05)
shadow.md  = 0 4px 6px rgba(0,0,0,0.07)
shadow.lg  = 0 10px 15px rgba(0,0,0,0.10)
shadow.xl  = 0 20px 25px rgba(0,0,0,0.10)
```

**Usage:** sm = subtle border replacement, md = cards, lg = dropdowns/popovers, xl = modals

---

### 1.6 Motion / Animation

```
duration.instant = 0ms
duration.fast    = 100ms
duration.normal  = 200ms  ← default
duration.slow    = 300ms
duration.slower  = 500ms

easing.default  = cubic-bezier(0.4, 0, 0.2, 1)
easing.in       = cubic-bezier(0.4, 0, 1, 1)
easing.out      = cubic-bezier(0, 0, 0.2, 1)
easing.spring   = cubic-bezier(0.34, 1.56, 0.64, 1)
```

---

## 2. Components

> Add new components here when created, with variants and DO/DON'T.
> Every new component must be approved per `governance.md` before entering the DS.

### Template for adding a new Component

```markdown
### ComponentName

**Figma Node:** [Frame path in Figma]
**Purpose:** [why this component exists]

**Variants:**
- size: sm | md | lg
- state: default | hover | active | disabled | loading
- variant: primary | secondary | ghost | danger

**Props/Rules:**
- Must always have a label (no icon-only without a tooltip)
- Minimum touch target: 44x44px (mobile)

**DO:**
- ✅ Use ButtonPrimary for the main CTA only (1 per page)

**DON'T:**
- ❌ Never place 2 ButtonPrimary on the same page
```

---

### Button

**Figma Node:** `Design System / Components / Button`
**Purpose:** Interactive action trigger

**Variants:**
- `variant`: primary | secondary | ghost | danger | link
- `size`: sm | md | lg
- `state`: default | hover | active | disabled | loading
- `iconPosition`: none | left | right | only

**DO:**
- ✅ ButtonPrimary = main CTA only (1 per viewport)
- ✅ Use the loading state while awaiting async actions

**DON'T:**
- ❌ Never 2 ButtonPrimary on the same page
- ❌ Never use colors outside the defined variants

---

### Input

**Figma Node:** `Design System / Components / Input`

**Variants:**
- `type`: text | password | search | number
- `size`: sm | md | lg
- `state`: default | focus | error | disabled | readonly
- `hasLabel`: true | false
- `hasHelper`: true | false

**DO:**
- ✅ Every input must have a label (visible or aria-label)
- ✅ Use the error state with helper text explaining the problem

**DON'T:**
- ❌ Never use a placeholder in place of a label
- ❌ Never hide helper text when an error is present

---

### [Add more components here — use the template above]

---

## 3. Layout Rules

### Grid System

```
Web:
- Columns: 12
- Gutter: 24px (desktop), 16px (tablet), 12px (mobile)
- Margin: 80px (desktop), 40px (tablet), 16px (mobile)
- Max content width: 1280px

Mobile App:
- Columns: 4
- Gutter: 16px
- Margin: 16px
```

### Breakpoints

```
xs:  < 480px    ← small mobile
sm:  480-767px  ← mobile
md:  768-1023px ← tablet
lg:  1024-1279px ← small desktop
xl:  1280-1535px ← desktop
2xl: ≥ 1536px   ← large desktop
```

### Density

```
compact:      space.2 (8px) gaps — data tables, dense lists
default:      space.4 (16px) gaps — standard UI
comfortable:  space.6 (24px) gaps — marketing pages, onboarding
```

---

## 4. Hard Constraints

> These rules must **never** be violated, under any circumstances.
> All 8 must appear in full in the DS Context Block of `agent-context.md` at all times.

```
CONSTRAINT-01: No color values outside the tokens
CONSTRAINT-02: No hardcoded font-size or spacing in Figma
CONSTRAINT-03: No new components when one already exists in the DS
CONSTRAINT-04: No shadows outside the elevation scale
CONSTRAINT-05: No border-radius outside the radius scale
CONSTRAINT-06: Every interactive element must have complete states (hover, active, disabled)
CONSTRAINT-07: Minimum touch target 44x44px on mobile
CONSTRAINT-08: Color contrast must pass WCAG AA (4.5:1 text, 3:1 large text)
```
