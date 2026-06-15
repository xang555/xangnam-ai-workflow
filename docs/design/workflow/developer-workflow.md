# Developer Workflow Guide
> Layer 2 · Workflow · For Developers using Claude Code + Figma MCP

---

## Overview

```
Receive Figma design → Read Intent → Map Components → Implement → Verify Fidelity → CI Gate → PR Ready
```

Claude Code + Figma MCP makes design implementation as accurate as possible — but the DS Context must be loaded **automatically** via `CLAUDE.md`, not pasted by hand every time.

> 📌 **Skills vs CLAUDE.md.** The three design skills (ds-design, ds-enforce, ds-handoff) are for **designers in Claude.ai**. Developers don't use those skills — you use **Claude Code with a `CLAUDE.md`** that carries the same DS context locally. Same design system, two delivery mechanisms: designers' skills fetch the hosted file; your CLAUDE.md is loaded from the repo.

---

## Phase 0: Repo Setup (once per repo)

> ⚠️ **This is the most important step in this workflow** — a correctly set-up repo makes every later session safe automatically.

- [ ] Create `CLAUDE.md` at the repo root from `templates/CLAUDE.md.template` — fill in the Project Context completely
- [ ] Set up the token pipeline + CI lint per `automation/token-pipeline.md`
- [ ] (If your Figma plan supports it) set up **Figma Code Connect** — officially maps Figma components ↔ code components, so the MCP instantly knows which codebase component each node is, with no manual mapping table
- [ ] Verify Figma MCP is connected in Claude Code (`claude mcp list`)

**Result:** whenever Claude Code opens in this repo, the DS context + constraints load automatically.

---

## Prerequisites (per session)

- [ ] Figma file URL and the frame/screen to implement
- [ ] Handoff note from the designer (if missing — ask before starting)
- [ ] DS version in the handoff note **matches** the version in `CLAUDE.md` — if not, flag it back to the designer immediately. Never guess.

---

## Phase 1: Setup Claude Code Session

### Step 1.1 — Start the session (much shorter — CLAUDE.md does the work)

```
## Task
Implement [screen/component name] from the Figma design

### Figma Reference
- File URL: [https://figma.com/file/...]
- Target Frame: [frame name, e.g. "Mobile / Profile Screen"]
- Node ID: [if known — more precise than the frame name]

### Handoff Note
[paste the handoff note or its file path]

Steps:
1. Read the Figma frame and extract the component structure
2. Map to existing components in the codebase first
3. Create new components only where none exist
4. Use token values — no hardcoding (see the constraints in CLAUDE.md)
```

> 💡 If the repo doesn't have a `CLAUDE.md` yet (e.g. a legacy repo), paste the DS Context Block from `foundation/agent-context.md` yourself — then go finish Phase 0.

### Step 1.2 — Have Claude read Figma first

Claude Code reads the design through Figma MCP — wait for the result, then verify Claude understood the structure correctly:

```
Ask Claude: "Explain the component tree of this frame
and map it to the existing components in this project."
```

Verify:
- ✅ Claude names components matching the DS
- ✅ Claude identifies reusable existing components
- ✅ Claude identifies gaps (what still needs to be created)

**Fallback when MCP misreads or reads incompletely:** export the frame as a PNG and attach it in the prompt alongside the MCP read — Claude compares the visual against the structure simultaneously. This is more accurate than relying on MCP alone, especially for complex frames or absolute positioning.

---

## Phase 2: Implementation

### Step 2.1 — Component mapping before implementing

**Have Claude build a mapping table before writing code:**

```
Produce a mapping table in this format:

| Figma Element | Figma Node ID | Existing Component | Status |
|---------------|--------------|--------------------|--------|
| [Figma name] | [node-id] | [component path] | reuse/create/modify |
```

> 💡 If Code Connect is set up (Phase 0), most of the mapping comes straight from Figma — you just review it.

For any component marked `create`, ask Claude:
- What's the closest thing in the DS?
- Should this be a new component, or a variant of an existing one?

### Step 2.2 — Implement component by component

**Prompt pattern:**

```
Implement [ComponentName] from Figma node [node-id]

Requirements:
- Extends [existing component] (if reusing)
- Props: [list props from Figma variants]
- States: [default | hover | active | disabled | loading]

(token rules and comment format are already in CLAUDE.md — no need to repeat)
```

### Step 2.3 — Token Verification

After each component, have Claude check:

```
Audit this component:
1. Any hardcoded values? (hex, px values that aren't tokens)
2. Do token names match design-system.md?
3. Do responsive breakpoints match the DS?

Fix any hardcoded values to tokens immediately.
```

> Note: CI lint will catch this again at PR time (see `automation/token-pipeline.md`) — but catching it yourself first is faster.

---

## Phase 3: Verify Fidelity

### Step 3.1 — Visual Comparison

After implementation is complete:

```
Ask Claude: "Compare the implementation against the Figma design:
1. Does spacing/padding match?
2. Does typography (size, weight, color) match?
3. Do colors match the intended tokens?
4. What differs from the design?"
```

**Recommended:** if you use Playwright MCP — have Claude render the component, screenshot it, and compare directly against the Figma export (a real pixel-level check, not just code reading).

### Step 3.2 — Fidelity Checklist

- [ ] Layout structure matches Figma
- [ ] Spacing/padding follows DS tokens
- [ ] Typography (size/weight/color) matches
- [ ] Colors from tokens (no hardcoding)
- [ ] Border radius / shadow on scale (CONSTRAINT-04, 05)
- [ ] Interactive states complete (hover/active/disabled)
- [ ] Loading state present (if async)
- [ ] Empty state present (if list/data)
- [ ] Error state present (if form/fetch)
- [ ] Responsive per DS breakpoints
- [ ] Figma node ID commented in code
- [ ] Storybook story / golden test for every state (if the project has visual regression set up)

### Step 3.3 — Fix Loop

If fidelity issues are found:

```
Tell Claude what to fix:

"Issues found:
1. [Element] — spacing should be space.4 (16px) but is currently 14px
2. [Element] — should use color.neutral.500 but is currently #666
Fix both."
```

---

## Phase 4: CI Gate & PR

### Step 4.1 — Check the designer's handoff note

- [ ] Every listed screen/state is implemented
- [ ] Custom behaviors (animations/interactions) implemented
- [ ] Outstanding questions for the designer answered

### Step 4.2 — Local CI check before push

```bash
npm run lint:tokens     # token lint must pass
npm run build:tokens    # generated files must be in sync
```

### Step 4.3 — PR Description Template

```markdown
## Implements: [Feature Name]

### Figma Reference
- File: [Figma URL]
- Frames: [frame names implemented]
- DS Version: v[X.X.X]

### Components
- 🆕 Created: [list new components]
- ♻️ Reused: [list existing components]
- 🔄 Modified: [list modified components]

### Token Usage
- ✅ No hardcoded values (CI token-lint passed)
- ✅ All colors from DS tokens
- ✅ All spacing from DS scale

### States Implemented
- [x] Default
- [x] Loading
- [x] Empty
- [x] Error

### Screenshots
[add side-by-side screenshots against Figma]
```

---

## Figma MCP — Reference Commands

→ See the full set in `prompts/figma-mcp.md`

```
# Read a frame's component tree
"Read Figma frame [name] and show the full component tree"

# Pull token values of an element
"Read Figma node [node-id] and show all token values"

# Sync tokens
"Read Figma variables from the DS library, compare against tokens/*.json, and report the diff"

# Verify design fidelity
"Compare the code in [file path] against Figma frame [frame]"
```

---

## Tips & Best Practices

### ✅ Do this

**Set up CLAUDE.md before anything else** — this is the difference between "relying on discipline" and "the system enforcing itself"

**Map before implementing** — knowing what's reusable saves more time than implementing everything fresh

**Comment Figma references:**
```typescript
// Figma: Profile/Card/UserCard [node: 123:456]
// DS: Card/Elevated + Avatar/MD + Typography/Body
export const UserCard = ({ ... }) => {
```

**Attach screenshots for complex frames** — MCP structure + visual together is the most accurate

### ❌ Avoid this

- Don't hardcode anything that has a token (CI will block it anyway)
- Don't create new components without mapping against the DS first
- Don't implement without reading the designer's handoff note
- Don't implement when the handoff DS version ≠ the CLAUDE.md version
- Don't edit generated token files directly
- Don't merge without passing the fidelity checklist + CI

---

## Common Issues & Solutions

| Problem | Cause | Fix |
|---------|-------|-----|
| Claude uses wrong component names | Repo has no CLAUDE.md / incomplete context | Finish Phase 0 |
| Layout differs from Figma | Figma MCP read the frame incompletely | Use a specific node ID + attach a screenshot |
| Token exists in the DS but Claude hardcodes | CLAUDE.md token list missing a category | Sync CLAUDE.md with the latest agent-context.md |
| New component is inconsistent | DS components weren't referenced | Have Claude compare against the DS before creating |
| Responsive is off | Breakpoints not specified | Breakpoints are in CLAUDE.md — check it's synced |
| DS version mismatch | Figma library and code on different versions | Flag the designer; don't implement until aligned |
