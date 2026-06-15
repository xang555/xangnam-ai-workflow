# AI Agent Design Manual
> A guide for UX/UI teams on using AI agents in design work

**Version:** 1.2.0
**Last Updated:** 2026-06-11
**Maintained by:** Design Team (DS Owner: see `foundation/governance.md`)

---

## 📖 What is this manual?

This manual helps the UX/UI team use AI agents (Claude + Figma MCP) to design apps and web apps accurately and consistently against the team's custom design system.

**As of v1.2.0, the workflow is skill-centric.** Three installable skills carry the design-system knowledge. Designers no longer copy-paste context blocks or prompt templates — they just describe what they want, and the right skill activates and fetches the live design system from a hosted URL.

**Two-layer principle:**
- **Skills + hosted DS** — the AI always works against one live design system, fetched at runtime; output is DS-compliant by default
- **Automated gates** — token pipeline + CI lint block anything that slips past on the code side

---

## 🗂️ Manual Structure

```
ai-design-manual/
│
├── README.md                          🔵 you are here
│
├── foundation/                        🔵 Layer 1: Design System Contract
│   ├── design-system.md               ← human-readable DS, official baseline
│   ├── agent-context.md               ← THE hosted file the skills fetch (source of truth)
│   ├── governance.md                  ← ownership, semver, approval flow
│   └── changelog.md                   ← DS change history
│
├── skills/                            🟢 Layer 2: Skills (replaces the old prompt library)
│   ├── README.md                      ← install + how to host the DS URL + private-DS variant
│   ├── ds-design.skill                ← generate UI to the DS
│   ├── ds-enforce.skill               ← review + fix designs
│   └── ds-handoff.skill               ← prepare handoff notes
│
├── workflow/                          🟣 Layer 3: Step-by-step guides
│   ├── designer-workflow.md           ← what to prepare + step-by-step per skill
│   ├── developer-workflow.md          ← Claude Code + Figma MCP + CLAUDE.md
│   └── handoff-workflow.md            ← Design → Dev bridge
│
├── prompts/                           🟢 Developer prompts
│   └── figma-mcp.md                   ← Figma MCP commands (developers)
│
├── automation/                        🟠 Layer 4: Automated Enforcement
│   └── token-pipeline.md              ← Figma Variables → Style Dictionary → CI lint
│
└── templates/                         ⚪ Layer 5: Templates
    ├── CLAUDE.md.template             ← DS context for dev repos
    └── handoff-note.template.md       ← fillable handoff note
```

---

## 🚀 Getting Started

### For the team (one-time)
1. Fill in **DS Owner / Maintainer** in [`foundation/governance.md`](foundation/governance.md)
2. Verify baseline tokens in [`foundation/design-system.md`](foundation/design-system.md)
3. **Host** [`foundation/agent-context.md`](foundation/agent-context.md) at a public raw URL (see [`skills/README.md`](skills/README.md))
4. Set up **Figma Variables** + a published **DS Library file**
5. Set up the **token pipeline + CI** per [`automation/token-pipeline.md`](automation/token-pipeline.md)
6. Developers add **`CLAUDE.md`** to repos from [`templates/CLAUDE.md.template`](templates/CLAUDE.md.template)

### For Designers
1. Install the three skills (see [`skills/README.md`](skills/README.md))
2. Save the public DS URL
3. Follow [`workflow/designer-workflow.md`](workflow/designer-workflow.md) — describe the task, paste the URL when asked, review, hand off

### For Developers
1. Do **Phase 0** in [`workflow/developer-workflow.md`](workflow/developer-workflow.md) (CLAUDE.md + pipeline)
2. Use [`prompts/figma-mcp.md`](prompts/figma-mcp.md) for MCP implementation

---

## ⚡ Which skill for which task

| You want to… | Skill | Say something like |
|--------------|-------|--------------------|
| Design a screen / component / flow | **ds-design** | "Design a checkout screen for web" |
| Check or fix a design | **ds-enforce** | "Review this for DS compliance" |
| Check accessibility | **ds-enforce** | "Accessibility check on this screen" |
| Prepare for developers | **ds-handoff** | "Write a handoff note for this" |

Skills trigger automatically from natural phrasing (Thai or English). You don't invoke them by name.

---

## 📌 Golden Rules

| # | Rule | Do | Don't |
|---|------|----|----|
| 1 | One hosted DS | Update + re-publish `agent-context.md` | Keep private copies of tokens |
| 2 | Let skills fetch | Paste the DS URL when asked | Paste context blocks manually |
| 3 | Describe by component | "Button primary/md" | "the blue button" |
| 4 | Review before handoff | Run ds-enforce every time | Ship straight from generation |
| 5 | DS changes via governance | Proposal → approve → re-publish | Quietly change the DS alone |
| 6 | Figma uses Variables only | Bind every value; use library instances | Raw hex / detached components |
| 7 | Code passes CI | Token lint green before merge | Merge with hardcoded values |

---

## 🔄 When the Design System Changes

Through the **governance flow** ([`foundation/governance.md`](foundation/governance.md)):

```
Proposal → DS Owner approves → Maintainer executes:

1. design-system.md      ← edit source for humans
2. agent-context.md      ← sync + run Consistency Check
3. RE-PUBLISH agent-context.md  ← skills fetch the live file, so this is what updates them
4. changelog.md          ← record version + Action Required
5. Figma library         ← update + publish
6. Token pipeline        ← regenerate token files
7. CLAUDE.md in repos    ← sync (Claude Code loads these locally)
8. Notify the team
```

> ✅ Skills need **no** re-upload when the DS changes — they fetch the re-published file. Re-upload a skill only when you change its *behavior*.

---

## 🛠️ Tools

| Tool | Who | Purpose |
|------|-----|---------|
| Claude.ai + the 3 skills | Designer | Design, review, handoff — DS fetched live |
| Claude Code (+ CLAUDE.md) | Developer | Implement Figma designs; DS context auto-loaded |
| Figma MCP | Designer + Developer | Read/write Figma through AI |
| Figma (+ Variables, Code Connect) | Designer | Primary design tool; Variables = token source of truth |
| Style Dictionary | DevOps/Developer | Generate per-platform token files |
| ESLint/Stylelint + GitHub Actions | Automated | Block hardcoded values in PRs |
