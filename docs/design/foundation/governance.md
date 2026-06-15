# DS Governance
> Layer 1 · Foundation · Who owns the DS, how it changes, how it's versioned

**For:** anyone proposing a DS change (Designer, Developer, PM)

---

## 1. Roles & Ownership

| Role | Who | Responsibility |
|------|-----|----------------|
| **DS Owner** | [name — 1 person] | Approves every DS change, keeps foundation files consistent |
| **DS Maintainer** | [names — 1-2 people] | Updates foundation files, Figma library, and token pipeline after approval |
| **Contributor** | everyone on the team | Proposes new tokens/components via proposals |

> Small teams (1-3 people): the DS Owner and Maintainer can be the same person, but **a name must be written down** — "everyone owns it" = nobody owns it.

---

## 2. Semantic Versioning Policy

The DS uses semver: `MAJOR.MINOR.PATCH`

| Level | When | Example | Impact |
|-------|------|---------|--------|
| **PATCH** (1.1.0 → 1.1.1) | Change an existing token value, no renames | primary.500 changes from #3B82F6 → #2F6FE4 | Old designs/code still work — just regenerate tokens |
| **MINOR** (1.1.0 → 1.2.0) | Add something new without touching the old | New component, new token, new variant | Old prompts still work — new items are optional |
| **MAJOR** (1.1.0 → 2.0.0) | Breaking change | Rename a component/token, remove a token, change structure | **Every prompt, CLAUDE.md, Figma library, and codebase must be updated** |

**Deprecation rule:** before removing anything (MAJOR), it must be deprecated in a MINOR release at least one version earlier
— move it to the Deprecated section in `agent-context.md` and name its replacement.

---

## 3. Change Approval Flow

```
Contributor proposes → DS Owner reviews → Approve/Reject → Maintainer executes → Sync everywhere
```

### Step 1 — Proposal

Use this template (post in the team channel or open an issue):

```markdown
## DS Change Proposal

**Type:** [new token | new component | change value | rename | remove]
**Semver impact:** [patch | minor | major]
**What:** [describe the proposal]
**Why:** [a real use case the current DS doesn't support]
**Checked for duplicates:** [reviewed the Component Index + tokens — nothing similar exists]
**Affected:** [which prompts/Figma/code are impacted]
```

### Step 2 — Review Criteria (DS Owner)

- [ ] Does something similar already exist in the DS? (if yes → reject, use the existing item)
- [ ] Does the use case occur in ≥ 2 places? (one-off → reject; handle as a documented local exception)
- [ ] Is it consistent with the existing token scales? (e.g. spacing must be a multiple of 4)
- [ ] Does it satisfy CONSTRAINT-01 through 08?

### Step 3 — Execute (Maintainer) — in this order only

1. `foundation/design-system.md` — edit/add
2. `foundation/agent-context.md` — sync the Context Block + Component Index + run the Consistency Check
3. **Re-publish `agent-context.md`** at its hosted URL — the design skills fetch this file live, so re-publishing is what makes them current. No skill re-upload needed.
4. `foundation/changelog.md` — record a new entry with Action Required
5. Figma library — update components/variables, then publish
6. Token pipeline — regenerate token files (see `automation/token-pipeline.md`)
7. `CLAUDE.md` in every repo — sync the DS Context Block (see `templates/CLAUDE.md.template`). Developers load this locally via Claude Code, so it doesn't auto-update like the hosted file.
8. Notify the team — new version + any breaking changes

> ⚠️ **Never skip a step** — most "AI didn't follow the DS" problems come from foundation files being out of sync.
> 💡 The split to remember: **designers' skills fetch the hosted file** (step 3 updates them); **developers' Claude Code loads CLAUDE.md locally** (step 7 updates them).

---

## 4. Version Pinning Rules

Every artifact must declare which DS version it uses:

| Artifact | Where |
|----------|-------|
| Figma file | Cover page + handoff note |
| Every prompt | DS Context Block (`DS Version:` field) |
| Codebase | `CLAUDE.md` + `package.json` (if tokens are a package) |
| Handoff note | Header |
| PR | PR description template |

**Version mismatch = blocker:** if Figma uses v1.2 but code is still on v1.1, the developer must flag it before implementing. Never guess. (See `workflow/handoff-workflow.md`.)

---

## 5. Exception Process

Sometimes you need to step outside the DS (e.g. a special marketing page):

1. Get DS Owner approval first
2. Document the exception in code/Figma: `// DS-EXCEPTION: [reason] [approved-by] [date]`
3. Review exceptions quarterly — if one is reused in ≥ 2 places, promote it into the DS

**Never:** an undocumented exception is just an ordinary DS violation.
