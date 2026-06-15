# Prompt Library — Figma MCP (Developer)
> Layer 3 · Prompts · For Claude Code + Figma MCP

---

## How to use this file

Prompts in this file are for **Claude Code** connected to **Figma MCP**.
Always begin every session with the Session Setup.

---

## 0. Session Setup (do this every time)

> ✅ **If the repo already has a `CLAUDE.md`** (per `templates/CLAUDE.md.template`) — the DS context loads automatically. Use this short setup:

```
## Task Setup

### Figma Reference
- File URL: [https://www.figma.com/file/...]
- Target: [page/section to implement]
- Node ID: [if known — more precise than a name]

### Handoff Note
[paste it or give the path]

Confirm Figma MCP is connected and can read the file before starting.
Check that the DS version in the handoff note matches CLAUDE.md — if not, stop and report.
```

> ⚠️ **If the repo doesn't have a `CLAUDE.md` yet** — use the full setup below, then go set up CLAUDE.md (Phase 0 in `developer-workflow.md`):

```
## Developer Session Setup

### Design System Context
[paste the full DS Context Block from foundation/agent-context.md]

### Project Context
- Framework: [React 18 | Next.js 14 | Flutter 3.x]
- Styling: [Tailwind CSS | CSS Modules | Styled Components]
- Token file location: [src/styles/tokens.ts]
- Components directory: [src/components/]
- DS library version in code: v[X.X.X]

### Figma Setup
- File URL: [https://www.figma.com/file/...]
- Target: [page/section to implement]

Confirm Figma MCP is connected and can read the file before starting.
```

---

## 1. Read & Map Component

Use when: understanding the Figma design before implementing

```
Read Figma frame: [frame name or URL]

and build a mapping table:

| Figma Element | Figma Node ID | Existing Code Component | Action |
|--------------|--------------|-------------------------|--------|
| [name] | [node-id] | [path or N/A] | reuse / create / modify |

Then summarize:
1. Components reusable from the codebase as-is
2. Components that must be created
3. Components that must be modified
4. Tokens needed (compared against the DS Context)
5. Questions or ambiguities before implementing
```

---

## 2. Implement Screen from Figma

Use when: implementing a full screen from Figma

```
Implement [screen name] from Figma frame: [frame name/URL]

### Mapping Reference
[paste the mapping table from step 1]

### Implementation Requirements
- Use tokens from: [path/to/tokens.ts]
- Reuse components from: [path/to/components/]
- Create new components in: [path/to/components/feature/]
- No hardcoded color/spacing/font values
- Add comment `// Figma: [node-id]` at the top of every new component

### Code Style
- TypeScript strict mode
- Functional components + hooks
- [add your team's conventions]

### States Required
- [ ] Default
- [ ] Loading (skeleton)
- [ ] Empty
- [ ] Error

Implement section by section, top → bottom.
After each section, ask before continuing or adjusting.
```

---

## 3. Implement Single Component

Use when: implementing a single component from Figma

```
Implement component: [component name]
Figma node: [node-id or path]

### DS Reference
- DS Component: [name in the DS Component Index]
- Base component (if any): [path in codebase]

### Component Spec from Figma
Read the Figma node and extract:
1. Available props/variants
2. States (default/hover/active/disabled/loading)
3. Sizes if any
4. Token values everywhere

### Output
- TypeScript interface for props
- Component implementation
- CSS/styles using tokens
- Basic usage example
- Storybook stories (if the project has them)

### Comment format
// Figma: [node-id] - [component name]
// DS: [DS Component name] / [variant]
```

---

## 4. Token Audit

Use when: hunting hardcoded values in code

```
Audit file/directory: [path]

Search for these hardcoded values:
1. Hex colors (#xxxxxx or rgb/rgba)
2. Pixel values outside the spacing scale
   (not multiples of 4: 4,8,12,16,20,24,32,40,48,64,80,96)
3. Font sizes not on the DS typography scale
4. Font weights other than 400/500/600/700

Report in this format:
File: [path]
Line X: [code snippet] → should be [token name]

Then ask whether to fix these files now.
```

---

## 5. Sync Figma Tokens to Code

Use when: DS tokens were updated in Figma and need syncing to code

> 📌 If the repo has the token pipeline set up (`automation/token-pipeline.md`), sync primarily through the pipeline (tokens.json → Style Dictionary) — use this prompt for verifying/diffing, or for teams without the pipeline yet.

```
Read Figma variables/tokens from: [Figma file URL]
Collection: [variable collection name, e.g. "Primitives", "Semantic"]

Compare against the token file at: [path/to/tokens.ts]

Report:
1. Identical tokens (no action)
2. Differing tokens (value changed)
3. Tokens in Figma but not in code (must add)
4. Tokens in code but not in Figma (must review)

Then ask whether to update the token file to match Figma.
```

---

## 6. Verify Fidelity

Use when: checking whether code matches Figma

```
Compare the implementation in [file path]
against the Figma design: [frame name/URL]

Check all:
1. Does the layout structure match?
2. Does spacing/padding/gap match the DS tokens?
3. Does typography (size/weight/line-height/color) match?
4. Do colors match the intended tokens?
5. Does border radius match?
6. Does shadow match?
7. Are icons correct?
8. Does responsive behavior match the DS breakpoints?

Report:
✅ [what matches]
❌ [what differs] — Figma: [Figma value] → Code: [current value] → Fix: [correct token/value]

After reporting, ask whether to fix immediately.
```

---

## 7. Generate Figma Annotation Summary

Use when: a developer wants a structured spec from Figma

```
Read Figma frame: [frame name/URL]
and produce an implementation spec:

## [Screen Name] — Implementation Spec

### Layout
[describe the layout structure]

### Component Tree
[show the component hierarchy]

### Design Tokens Used
| Property | Token | Value |
|----------|-------|-------|
| Background | color.neutral.50 | #F9FAFB |
| [property] | [token] | [value] |

### States to Implement
| State | Trigger | Visual Change |
|-------|---------|---------------|
| Loading | API call | Show skeleton |
| [state] | [trigger] | [change] |

### Interactions
| Element | Trigger | Behavior |
|---------|---------|----------|
| [element] | [trigger] | [behavior] |

### Responsive Behavior
| Breakpoint | Layout Change |
|-----------|---------------|
| Mobile (< 768px) | [change] |
| [breakpoint] | [change] |
```

---

## Quick Reference: Figma MCP One-liners

```
# Read a frame's component tree
"Read Figma frame [name] and show the full component tree"

# Pull an element's token values
"Read Figma node [node-id] and show all token values"

# Find all components on a page
"List every component instance in frame [name] with variants"

# Check auto-layout
"Read all auto-layout settings of frame [name]"

# Pull states
"Find all frames that are states of [component name] in Figma"
```

---

## 8. Fallback: when MCP reads incompletely or unclearly

Use when: the component tree from MCP doesn't match what you see in Figma, or the frame is very complex

```
Figma MCP read frame [name] but the structure is incomplete.
Attached is a screenshot of this frame: [attach a PNG export from Figma]

Please:
1. Compare the screenshot against the structure MCP returned
2. Identify elements MCP missed or misread
3. For parts MCP can't see — use the screenshot visuals + DS Context to infer the closest tokens, and flag them for verification
```

**Tips to reduce MCP misreads:**
- Use a specific node ID instead of a frame name
- Ask the Designer to keep layers clean (auto-layout, meaningful layer names, no detached instances)
- Very large frame → read it section by section

---

## 9. Figma Code Connect (if your plan supports it)

Code Connect is a Figma feature that officially maps Figma components ↔ codebase components (via `.figma.tsx` files).
The result: MCP/Dev Mode shows your team's real component code instead of auto-generated code.

```
Set up Code Connect for component: [component name]

1. Read the Figma component: [node-id or library path]
2. Look at the existing component at: [path in codebase]
3. Create [ComponentName].figma.tsx mapping:
   - Figma variants → component props
   - Figma states → prop values
4. Verify the mapping covers every variant in the DS Component Index
```

> Once Code Connect covers every component — step "Read & Map Component" (#1) becomes much shorter, because the mapping comes straight from Figma.
