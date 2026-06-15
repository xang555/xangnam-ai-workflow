# Skills — Install & Setup
> Layer 2 · The three design skills that replace the old prompt library

Designers no longer copy-paste DS context blocks and prompt templates. Instead, three **skills** carry the workflow. Each skill fetches the live design system from a hosted URL, so output is always against the current DS.

---

## The three skills

| Skill | Triggers when you… | What it does |
|-------|--------------------|--------------|
| **ds-design** | ask to design/create/lay out/mock up any screen, component, variant, state, or flow | Generates UI using only DS tokens + components, all constraints satisfied. Handles custom patterns (image AppBars, exceptions) correctly. |
| **ds-enforce** | ask to review/audit/check a design, fix DS violations, or run an accessibility check | Reviews against the DS in 4 modes (consistency, targeted fix, cross-screen, a11y). Also pre-handoff QA. |
| **ds-handoff** | are ready to hand off to developers, or want a handoff note | Produces a complete handoff note: screens, states, components, **asset sources**, behaviors, edge cases. |

All three trigger automatically from natural phrasing (Thai or English) — a designer doesn't need to know the skill exists. They just say "ออกแบบหน้า profile" or "review this screen" and the right skill activates.

---

## Step 1 — Host the DS file at a public URL

The skills fetch `foundation/agent-context.md` over the web. It must be reachable at a **public raw URL**. Options:

| Option | How | Good for |
|--------|-----|----------|
| **Public GitHub repo (raw)** | Put `agent-context.md` in a public repo → use the `raw.githubusercontent.com/...` link | Most teams — versioned, easy to update |
| **GitHub Gist (public)** | Paste the file into a public Gist → use the raw link | Quick start, single file |
| **GitHub Pages / any static host** | Publish the file → use its URL | Teams with a docs site |

Copy the final raw URL — designers will paste it into the skill once per conversation.

> ⚠️ **The URL must be public.** `web_fetch` can't authenticate, so a **private** repo link will fail and the skill will keep asking for a valid URL. See the private-DS variant below if your DS can't be public.

---

## Step 2 — Install the skills in Claude.ai

For each `.skill` file (`ds-design.skill`, `ds-enforce.skill`, `ds-handoff.skill`):

1. Open Claude.ai → **Settings → Capabilities** (Skills)
2. Upload the `.skill` file
3. Repeat for all three

Once installed, they're available in every conversation and trigger on matching requests.

---

## Step 3 — First run (designer)

1. Start a normal request: *"Design a settings screen for mobile-ios."*
2. The skill asks for the DS context URL (first time in the conversation). Paste the public raw URL from Step 1.
3. The skill fetches it and proceeds. For the rest of that conversation, it reuses the loaded DS — no re-asking.

That's it. The DS URL is the only thing a designer prepares.

> 💡 Not sure what to say? See [`prompt-examples.md`](prompt-examples.md) for copy-and-fill example requests for all three skills.

---

## How the fetch gate works (so you know what to expect)

Every skill runs this gate before doing any work:

```
1. URL given in the conversation?  ──no──>  Ask for it (once)
            │ yes
            ▼
2. web_fetch the URL
            │
       success? ──no──>  Stop. Report the error. Ask for a valid public URL.
            │ yes               (never falls back to invented/remembered tokens)
            ▼
3. Use the fetched file as the single source of truth — design / review / handoff
            │
            ▼
4. Reuse it for the rest of the conversation (no re-fetch unless the DS changed)
```

This was tested: a missing URL prompts a question, a broken URL stops and re-asks, and a working URL makes the skill use exactly the fetched tokens (verified by feeding it a DS with distinct values and confirming the output used them, not memorized ones).

---

## Private-DS variant (if you can't host publicly)

If your design system must stay internal and can't be served at a public URL, the fetch approach won't work — `web_fetch` has no credentials. Two alternatives:

1. **Paste instead of fetch.** Keep the DS file internal; the designer pastes its contents into the conversation when the skill asks. The skill then uses the pasted content as the source of truth. (Ask for a skill variant that prompts for pasted content instead of a URL — it's a one-line change to the load protocol.)
2. **Publish a sanitized copy.** If only certain values are sensitive, host a public `agent-context.md` with tokens/components/constraints (which aren't usually secret) and keep rationale/internal notes in the private `design-system.md`.

Most teams find the token list itself isn't sensitive, so a public `agent-context.md` + private `design-system.md` is the common setup.

---

## Keeping skills current

Because skills fetch the hosted file, **you never re-package or re-upload skills when the DS changes** — you just update and re-publish `agent-context.md`. Re-upload a skill only when you change the skill's *behavior* (its instructions), not the DS data.
