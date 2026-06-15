# Handoff Note — [Feature Name]
> Layer 5 · Template · Fill this in (or let the `ds-handoff` skill generate it, then add Figma links)

**Date:** YYYY-MM-DD
**Designer:** [name]
**DS Version:** v[X.X.X]
**Figma File:** [URL]

---

## Screens / Frames

| Frame Name | Figma Path | Description | Priority |
|-----------|------------|-------------|----------|
| [name] | [path] | [description] | P1/P2/P3 |

---

## States Required

| Screen | States |
|--------|--------|
| [name] | default / loading / empty / error |

---

## Component Usage

| Component | Variant | Notes |
|-----------|---------|-------|
| [name] | [variant] | [notes] |

---

## Assets
> Include for every image/icon beyond the standard icon set. **Defining the source is mandatory.**

| Asset | Source | Type | Spec |
|-------|--------|------|------|
| [name] | Figma export / [CDN URL] | static / dynamic | [ratio, format, @1x/2x/3x] |

> For dynamic assets: confirm a **fallback** is designed (e.g. AppBar image fails → variant=solid).

---

## Behaviors & Interactions
> Specify motion with DS tokens — never "fades in," but "fade in, duration.normal (200ms), easing.out."

### [interaction name]
- Trigger: [user action]
- Animation: [duration + easing from the DS]
- Description: [...]

---

## Edge Cases

- [case + how to handle]

---

## Questions / Decisions Pending

- [ ] [...]

---

## Notes for Developer

- [anything not obvious from Figma alone]
