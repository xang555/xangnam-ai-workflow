# Designer Workflow Guide
> Layer 3 · Workflow · For Designers using Figma + Claude.ai skills

This guide is built around the three skills (`ds-design`, `ds-enforce`, `ds-handoff`). The skills carry the design-system knowledge and fetch the live DS, so your job is to **prepare good inputs, drive the skills, and verify the output** — not to memorize tokens or paste context blocks.

---

## What you must prepare (once, and per project)

### One-time (per designer)
- [ ] The three skills installed in Claude.ai (see `skills/README.md`)
- [ ] The **public DS context URL** saved somewhere handy (you'll paste it into the skill once per conversation)

### One-time (per team — usually the DS Owner/Maintainer)
- [ ] `agent-context.md` hosted at a public raw URL and kept current
- [ ] A **Figma DS Library file** (separate from product files) with master components + Variables, published
- [ ] Product files have that library enabled in their Assets panel

### Per design session
- [ ] Know the **platform** (web / mobile-ios / mobile-android / tablet)
- [ ] Know the **feature** and its requirements
- [ ] Have the DS URL ready to paste when the skill asks

That's the whole prep. No prompt templates, no context blocks.

> 💡 For copy-and-fill example requests for each skill, see [`../skills/prompt-examples.md`](../skills/prompt-examples.md).

---

## The big picture

```
Prepare ──> Design (ds-design) ──> Review (ds-enforce) ──> Handoff (ds-handoff) ──> Dev
 inputs        generate UI            check + fix           write the note
```

Each arrow is a skill. You move left to right, verifying at each step.

---

## Step-by-step

### Phase 1 — Design with `ds-design`

**Step 1.1 — Make the request in plain language.**
Just describe what you want. The skill triggers automatically.

> "Design a User Profile screen for mobile-ios. It has an image AppBar with the user's cover photo, an avatar, name, email, an Edit Profile button, and a settings menu (account, notifications, privacy, sign out)."

**Step 1.2 — Provide the DS URL when asked.**
The first time in a conversation, the skill asks for your DS context URL. Paste the public raw link. It fetches and confirms.
(If it also asks for the platform and you didn't say, answer — it affects layout, grid, and touch targets.)

**Step 1.3 — Read the output.**
The skill returns a layout structure with component names, token values on every element, the required states, and developer annotations. Because it fetched the live DS, every value is a real token.

**Step 1.4 — Iterate in the same conversation.**
Follow-ups don't need the URL again — the skill reuses what it loaded.
> "Make the AppBar taller and move the Edit button into the AppBar."

> 💡 **Be specific, by component.** "Use Button variant=primary/md" beats "a blue button." The skill speaks the DS's language; meet it there.

---

### Phase 2 — Review with `ds-enforce`

Never ship a design straight from generation. Run a review.

**Step 2.1 — Ask for a consistency review.**
> "Review the profile screen for DS compliance."

The skill reports `✅ PASS / ❌ FAIL / ⚠️ WARNING` per check (colors, spacing, typography, components, names, states, constraints), with priority fixes.

**Step 2.2 — Fix what it finds.**
For targeted issues, ask for the fix directly:
> "Fix: the sign-out row should use color.error.500, and the section gap should be space.6."

The skill changes only those values, keeping layout and copy intact.

**Step 2.3 — Run an accessibility pass** (especially mobile):
> "Accessibility check on this screen."

Covers contrast, touch targets, color-as-sole-indicator, focus states, text sizing.

---

### Phase 3 — Annotate in Figma

The skills produce the spec; you apply it in Figma so it's handoff-ready.

- [ ] Use **instances from the DS Library** — never detach, never rebuild components in the product file
- [ ] **Bind every value to Figma Variables** (fills, spacing, radius) — no raw hex
- [ ] Name frames meaningfully ("Profile / Default", not "Frame 1")
- [ ] Use **auto-layout** (MCP reads structure from it)
- [ ] Make each state its own frame: default / loading / empty / error
- [ ] Annotate component name + variant + token values on each element
- [ ] For images, set **export settings** (1x/2x/3x) and name the layer (e.g. `img-appbar-bg`)

---

### Phase 4 — Handoff with `ds-handoff`

**Step 4.1 — Generate the handoff note.**
> "Write a handoff note for the profile screen."

The skill produces the full note: screens/frames, states, component usage, **assets** (with source: static export vs dynamic URL), behaviors (with DS motion tokens), edge cases, and notes for the developer.

**Step 4.2 — Fill in the Figma-specific blanks.**
Add frame paths/links and the Figma file URL (the skill can't see your Figma). Use `templates/handoff-note.template.md` if you prefer to start from the blank form.

**Step 4.3 — Confirm asset sources.**
The most common handoff failure is an undefined image source. For every image, confirm static vs dynamic — and if dynamic, that a fallback (variant=solid) is designed.

**Step 4.4 — Send to the developer** along with the Figma link.

---

## When the DS doesn't have what you need

If a design needs a token or component the DS lacks, the skill will tell you rather than invent one. Don't force it. Instead:

1. Use the closest existing component for now
2. Write a **DS Change Proposal** (`foundation/governance.md`) → DS Owner approves
3. Maintainer updates `design-system.md` + `agent-context.md`, then **re-publishes** the hosted file
4. The skill picks up the new token/component on its next fetch — nothing else to update

See `references/custom-patterns.md` inside the `ds-design` skill for how custom-but-governed patterns (like image AppBars) work: the custom part gets a spec, everything around it stays on tokens.

---

## Common scenarios

**A — New screen from scratch:** ds-design (with URL) → ds-enforce review → annotate in Figma → ds-handoff.

**B — A new pattern emerges mid-design:** design with the closest existing component → propose via governance → after approval + re-publish, regenerate with ds-design.

**C — Checking a batch of screens look consistent:** ds-enforce cross-screen review across the frames.

---

## What changed from the old (prompt-template) workflow

| Old way | New way |
|---------|---------|
| Copy DS Context Block from agent-context.md | Skill fetches it from the URL |
| Paste a prompt template per task | Just describe the task; skill structures it |
| Keep your own copy of tokens in mind | Skill always uses the live hosted DS |
| Manually remember constraints | Skill enforces all 8 automatically |

The discipline that remains is the same: be specific, review before handoff, and route new patterns through governance.
