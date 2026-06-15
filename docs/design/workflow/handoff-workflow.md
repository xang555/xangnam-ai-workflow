# Handoff Workflow Guide
> Layer 2 · Workflow · Design → Development Handoff

---

## Overview

A good handoff lets the developer implement correctly the first time — with no back-and-forth questions.

```
Design Done → Handoff Prep → Handoff Review → Dev Starts → QA Check → Done
```

---

## Designer Responsibilities (before handoff)

### Pre-handoff checklist

**Figma:**
- [ ] Every frame has a meaningful name (not "Frame 1")
- [ ] Components in Figma use the DS library (not detached)
- [ ] Annotations complete: component names, token values, states
- [ ] Every state has its own frame: default/loading/empty/error
- [ ] Prototype links exist for complex interactions

**Handoff Note:**
- [ ] Create `handoff-note.md` per the template below
- [ ] Specify the DS version used
- [ ] Specify behaviors not visible from Figma alone
- [ ] Specify edge cases

---

## Handoff Note Template

```markdown
# Handoff Note — [Feature Name]
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
| [screen name] | ✅ default ✅ loading ✅ empty ✅ error |

---

## Component Usage

| Component | Variant Used | Notes |
|-----------|--------------|-------|
| Button | Primary/Large | Main page CTA |
| [name] | [variant] | [notes] |

---

## Behaviors & Interactions

### [interaction name]
- Trigger: [user action]
- Animation: [duration + easing from the DS]
- Description: [explain]

---

## Edge Cases

- [Edge case 1 + how it should be handled]
- [Edge case 2]

---

## Questions / Decisions Pending

- [ ] [questions still awaiting an answer from PM/Tech]

---

## Notes for Developer

- [things to watch out for, or special context]
```

---

## Developer Responsibilities (after receiving the handoff)

### Step 1 — Read the handoff note first

**Don't** open Figma and start implementing — always read the handoff note first.

Verify:
- [ ] You understand the full scope
- [ ] The DS version matches the codebase
- [ ] Any questions before starting? Ask immediately

### Step 2 — Estimate and flag issues

After reading the handoff note, if there are problems:

```
Issues to flag back to the designer:
- DS version mismatch (Figma on v1.2 but code still on v1.1)
- A Figma component that doesn't exist in the codebase yet
- A designed state that's unclear
- A behavior that's technically infeasible under constraints
```

### Step 3 — Implement per the Developer Workflow

→ See `developer-workflow.md` for the step-by-step.

---

## QA Handoff Check

After the developer finishes, the designer performs design QA:

### Design QA Checklist

```
Open the implementation and Figma side by side

Visual:
- [ ] Layout matches
- [ ] Spacing/padding matches
- [ ] Typography (size/weight/color) matches
- [ ] Colors match
- [ ] Icons/images correct
- [ ] Border radius matches

States:
- [ ] Loading state correct
- [ ] Empty state correct
- [ ] Error state correct
- [ ] Disabled state correct
- [ ] Hover/Active states correct (web)

Responsive (web):
- [ ] Mobile (375px)
- [ ] Tablet (768px)
- [ ] Desktop (1280px)

Interactions:
- [ ] Animations/transitions match spec
- [ ] Navigation flow correct
```

### Reporting Design Bugs

If QA finds issues:

```markdown
## Design Bug Report

**Screen:** [screen name]
**Issue:** [describe the problem]
**Expected:** [per Figma/DS]
**Actual:** [as implemented]
**Figma reference:** [link to frame]
**Screenshot:** [attach]
**Severity:** [Critical/Major/Minor]
```

---

## Common Handoff Issues & Prevention

| Problem | Prevention |
|---------|-----------|
| Dev keeps asking about spacing/colors | Annotate token names on every element in Figma |
| Missing states | Run the states checklist before handoff |
| Animations don't match | Always specify duration+easing in the handoff note |
| DS version mismatch | Check the DS version before every handoff |
| Edge cases not designed | Run an edge-case review before handoff |
