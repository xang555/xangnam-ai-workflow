# Design System Changelog
> Layer 1 · Foundation · Record every change

---

## How to use this changelog

Every time the DS changes, add a new entry **at the top** in this format
(changes must be approved per `governance.md` first):

```markdown
## v[X.X.X] — YYYY-MM-DD
### 🆕 Added
### 🔄 Changed
### ⚠️ Breaking Changes
### 🗑️ Deprecated
```

**Semver:** PATCH = change an existing token value · MINOR = add new items · MAJOR = breaking change.

**Breaking Change** = anything that requires updating old prompts/CLAUDE.md, Figma files, or code.

---

## v1.2.0 — 2026-06-11

### 🆕 Added
- **Skill-centric workflow.** Three installable Claude.ai skills replace the old copy-paste prompt library:
  - `ds-design` — generate UI to the DS
  - `ds-enforce` — review + fix designs (consistency, targeted fix, cross-screen, accessibility)
  - `ds-handoff` — produce handoff notes
- `skills/README.md` — skill install steps, how to host the DS file at a public URL, and a private-DS (paste-instead-of-fetch) variant
- **Hosted DS model.** `agent-context.md` is now the single hosted source of truth the skills fetch at runtime via a public URL
- `templates/handoff-note.template.md` — extracted fillable handoff note
- Governance Step 3 now includes **re-publishing** the hosted `agent-context.md`

### 🔄 Changed
- `foundation/agent-context.md` — reframed as the hosted source of truth (header explains the fetch model); token content unchanged from v1.1.0
- `README.md` — rewritten around skills; added a "which skill for which task" table; golden rules updated (skills fetch the DS instead of pasting context blocks)
- `workflow/designer-workflow.md` — rewritten as a skill-driven, step-by-step guide with an explicit "what to prepare" section
- `workflow/developer-workflow.md` — added a note clarifying skills are designer-side; developers use Claude Code + CLAUDE.md
- DS change flow extended to re-publish the hosted file (skills) separately from syncing CLAUDE.md (developers)

### 🗑️ Removed (from the manual — content now lives inside the skills)
- `prompts/design-gen.md` → content is in the `ds-design` skill
- `prompts/ds-enforce.md` → content is in the `ds-enforce` skill
- `prompts/review.md` → content is in the `ds-enforce` skill
- `prompts/figma-mcp.md` **kept** — developers still need MCP prompts

### ⚠️ Breaking Changes
- None to the DS contract (tokens/components/constraints unchanged). This is a workflow change.

**Action Required:**
- [ ] Host `agent-context.md` at a public raw URL (see `skills/README.md`)
- [ ] Install the three skills in Claude.ai for each designer
- [ ] Share the public DS URL with the design team
- [ ] If the DS must stay private, set up the paste-instead-of-fetch variant (see `skills/README.md`)
- [ ] Developers: no change — keep using CLAUDE.md

---

## v1.1.0 — 2026-06-10

### 🆕 Added
- `foundation/governance.md`, `automation/token-pipeline.md`, `templates/CLAUDE.md.template`
- Color token `color.neutral.200`; Typography/Radius/Shadow/Motion sections in the Context Block
- Contrast notes; Thai/Lao font fallback guidance
- MCP fallback (screenshot) and Code Connect prompts; Developer Phase 0 (repo setup)

### 🔄 Changed
- Token values became the official baseline (no placeholders)
- All 8 constraints present in the Context Block (previously missing 04/05)
- Spacing scale completed through space.24
- Session setup relies on CLAUDE.md instead of pasting the context block

### ⚠️ Breaking Changes
- None to designs/code — but Action Required items applied

---

## v1.0.0 — 2026-06-09

### 🆕 Added
- Initial design system release: color/spacing/typography/radius/shadow/motion tokens
- Components (atoms, molecules, organisms), grid system, breakpoints
- 8 Hard Constraints (CONSTRAINT-01 through CONSTRAINT-08)

### ⚠️ Breaking Changes
- None (initial release)

---

## Template for the next entry

```markdown
## v1.X.X — YYYY-MM-DD

### 🆕 Added
### 🔄 Changed
### ⚠️ Breaking Changes
### 🗑️ Deprecated

**Action Required:**
- [ ] Update `agent-context.md` + run Consistency Check
- [ ] RE-PUBLISH `agent-context.md` at the hosted URL
- [ ] Publish the new Figma library version
- [ ] Regenerate token files (token pipeline)
- [ ] Sync `CLAUDE.md` in every repo
- [ ] Notify the team
```
