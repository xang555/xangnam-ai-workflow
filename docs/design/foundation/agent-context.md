# Agent Context — DS Source of Truth (Hosted)
> Layer 1 · Foundation · The single file the skills fetch at runtime

**DS Version:** 1.2.0

---

## 📡 This file is the hosted source of truth

The three design skills (`ds-design`, `ds-enforce`, `ds-handoff`) do **not** embed the design system. Instead, they fetch **this file** from a public URL at the start of every task. That means:

- There is exactly **one** copy of the DS that everyone works against — this one.
- When the DS changes, you update this file and **publish it** — every skill is instantly current, with nothing to re-sync.
- A designer always works against the live version, never a stale copy.

**To make this work, host this file at a public raw URL** (see `skills/README.md` for hosting options). Designers give that URL to the skill once per conversation; the skill fetches it before doing any work.

> ⚠️ Keep this file **machine-friendly and complete**. If a token category is missing here, the skills can't enforce it. This file and `design-system.md` must stay 100% in sync.

---

## Color Tokens (token names only — never raw hex in output)

```
color.primary.50=#EFF6FF | 100=#DBEAFE | 500=#3B82F6 (default) | 600=#2563EB (hover) | 900=#1E3A8A
color.error.500=#EF4444 | color.warning.500=#F59E0B | color.success.500=#22C55E
color.neutral.0=#FFFFFF | 50=#F9FAFB | 100=#F3F4F6 | 200=#E5E7EB | 500=#6B7280 | 900=#111827 | 1000=#000000
```

Usage: primary.500 = default interactive · neutral.900 = body text · neutral.500 = secondary text · neutral.100 = default border · neutral.200 = strong border · error/warning/success = semantic only.

Contrast notes (WCAG AA): neutral.500 on neutral.0 = 4.6:1 (don't use below 12px); primary.500 on neutral.0 = 3.7:1 (large text / UI only, never body); white on primary.500 = 3.7:1 (buttons need semibold+ at ≥14px).

## Spacing Tokens (base-4 — only these values)

```
space.1=4px, space.2=8px, space.3=12px, space.4=16px, space.5=20px,
space.6=24px, space.8=32px, space.10=40px, space.12=48px,
space.16=64px, space.20=80px, space.24=96px
```

Common: component padding sm = space.2/3 · md = space.3/4 · section gap = space.6/8 · page margin mobile = space.4, tablet = space.10, desktop = space.20.

## Typography Tokens

```
font.family: display/body="Inter", mono="JetBrains Mono"
  (Thai/Lao fallback: "Inter","Noto Sans Thai","Noto Sans Lao",sans-serif)
font.size: xs=12, sm=14, base=16, lg=18, xl=20, 2xl=24, 3xl=30, 4xl=36 (px)
font.weight: regular=400, medium=500, semibold=600, bold=700
line.height: tight=1.25, normal=1.5, relaxed=1.75
```

Common: page title = 3xl/bold · section title = xl/semibold · card title = lg/semibold · body = base/regular · caption = sm/regular/neutral.500. Thai/Lao text must use line.height ≥ normal (1.5), never tight.

## Radius Tokens

```
radius.none=0, sm=4px, md=8px (default), lg=12px, xl=16px, 2xl=24px, full=9999px (pills/avatars)
```

## Shadow Tokens

```
shadow.sm (subtle/border replacement) | shadow.md (cards) | shadow.lg (dropdowns/popovers) | shadow.xl (modals)
```

## Motion Tokens

```
duration: instant=0, fast=100ms, normal=200ms (default), slow=300ms, slower=500ms
easing: default | in | out | spring
```

## Component Index (use these exact names)

```
Atoms:
- Button [variant: primary|secondary|ghost|danger|link] [size: sm|md|lg] [state: default|hover|active|disabled|loading]
- Input [type: text|password|search|number] [size: sm|md|lg] [state: default|focus|error|disabled]
- Checkbox [state: unchecked|checked|indeterminate|disabled]
- Radio [state: unselected|selected|disabled]
- Toggle [state: off|on|disabled]
- Badge [variant: default|primary|success|warning|error] [size: sm|md]
- Avatar [size: xs|sm|md|lg|xl] [type: image|initials|icon]
- Icon [size: 16|20|24|32]
- Divider [orientation: horizontal|vertical]
- Spinner [size: sm|md|lg]
- Tag/Chip [variant: default|primary|removable]

Molecules:
- FormField [= Label + Input + HelperText]
- SearchBar [= Input(search) + Button]
- Dropdown / Select
- DatePicker
- Tooltip [position: top|bottom|left|right]
- Toast [variant: info|success|warning|error]
- Modal [size: sm|md|lg|fullscreen]
- Drawer [position: left|right|bottom]
- Card [variant: default|elevated|outlined]
- ListItem [variant: default|with-icon|with-avatar|with-action]
- NavigationItem [state: default|active|disabled]

Organisms:
- AppBar [variant: solid|image]  (image: mandatory scrim color.neutral.1000@40%, text=neutral.0)
- Navbar
- Sidebar / NavigationDrawer
- DataTable [features: sort|filter|pagination]
- Form [layout: single-column|two-column]
- EmptyState [type: no-data|error|no-results|no-access]
- PageHeader [= title + breadcrumb + actions]

[Add new components here when approved via governance.md]
```

## Layout

```
Web grid: 12 col · gutter 24/16/12px (desktop/tablet/mobile) · margin 80/40/16px · max width 1280px
Mobile grid: 4 col · gutter 16px · margin 16px
Breakpoints: xs<480, sm 480-767, md 768-1023, lg 1024-1279, xl 1280-1535, 2xl≥1536
Density: compact=space.2 · default=space.4 · comfortable=space.6
```

## Hard Constraints (all 8 — never violate)

```
CONSTRAINT-01: No color outside the tokens
CONSTRAINT-02: No hardcoded spacing/font-size/font-weight
CONSTRAINT-03: No new component if one exists in the DS
CONSTRAINT-04: No shadow outside the elevation scale
CONSTRAINT-05: No border-radius outside the radius scale
CONSTRAINT-06: Every interactive element has complete states (hover/active/disabled)
CONSTRAINT-07: Touch target ≥ 44x44px (mobile)
CONSTRAINT-08: Color contrast WCAG AA (4.5:1 text, 3:1 large text)
```

---

## 🔄 When this file changes

Every change goes through `governance.md`. After editing this file:

1. Keep it in sync with `design-system.md` (run the Consistency Check below)
2. **Re-publish** so the hosted URL serves the new version
3. Bump the DS Version at the top
4. Record it in `changelog.md`
5. Skills need **no** re-syncing — they fetch this file live. (Developer `CLAUDE.md` files still need syncing, since Claude Code loads those locally.)

### Consistency Check (run after every edit)
- [ ] All 8 constraints present, matching `design-system.md`
- [ ] Every token value matches `design-system.md`
- [ ] Component Index matches `design-system.md`
- [ ] File re-published at the hosted URL

## 🚫 Deprecated Components

```
[none yet]
```
