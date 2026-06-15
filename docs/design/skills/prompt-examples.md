# Skill Prompt Examples
> Layer 2 · Skills · What to say to trigger each skill — copy, fill the [brackets], send

With skills you don't write structured prompt blocks anymore. You describe the task in plain language (Thai or English) and the right skill activates. These are example phrasings, organized by task.

**The only setup:** the first time in a conversation, the skill asks for your **DS context URL** — paste the public raw link. After that, it reuses it for the whole conversation.

---

## 🎨 ds-design — Generating UI

### New screen
```
Design a [screen name] for [web | mobile-ios | mobile-android | tablet].

It contains:
- [element 1]
- [element 2]
- [element 3]

Primary action: [main CTA]
States I need: default, loading, empty, error
```

**Filled example:**
```
Design a User Profile screen for mobile-ios.

It contains:
- An image AppBar with the user's cover photo
- Avatar overlapping the AppBar, name, and email
- An Edit Profile button
- A settings menu: account, notifications, privacy, sign out

Primary action: Edit Profile
States I need: default, loading
```

### Single component / variant
```
Create a [component] variant for [use case].
Base it on [existing DS component]. It should [how it differs].
States: default, hover, active, disabled.
```

**Filled example:**
```
Create a Card variant for featured promotions.
Base it on Card. It should have an accent border and a badge slot top-right.
States: default, hover, pressed.
```

### Empty / error / loading state
```
Design the [empty | error | loading] state for [screen/component].
It happens when [trigger].
The user should be able to [recovery action / next step].
Tone: [helpful | neutral | encouraging]. Language: [Thai | English | Lao].
```

**Filled example:**
```
Design the empty state for the Orders list.
It happens when a new user has no orders yet.
The user should be able to start shopping (primary CTA).
Tone: encouraging. Language: Thai.
```

### User flow (multi-screen)
```
Design the [flow name] flow for [platform].
Steps:
1. [step] — user [does what]
2. [step] — user [does what]
3. [step] — user [does what]
Success path: [...]. Error paths: [...].
```

**Filled example:**
```
Design the Checkout flow for web.
Steps:
1. Cart review — user confirms items
2. Shipping — user enters address
3. Payment — user pays (PromptPay)
4. Confirmation — user sees order summary
Success path: payment OK → confirmation. Error path: payment fails → retry on step 3.
```

### Responsive adaptation
```
Adapt the [screen] from [source platform] to [target platform].
Source Figma frame: [path/link].
Expected changes: [e.g. bottom nav → side nav, single → two column].
```

---

## 🔍 ds-enforce — Reviewing & Fixing

### Full consistency review
```
Review the [screen/design] for DS compliance.
[Paste the design, or describe it, or reference the Figma frame.]
```

**Filled example:**
```
Review the profile screen I just designed for DS compliance.
```

### Targeted fix
```
Fix these in [screen/component]:
- [element]: [what's wrong] → should be [correct token]
- [element]: [what's wrong] → should be [correct token]
Keep layout and copy unchanged.
```

**Filled example:**
```
Fix these in the profile screen:
- Sign-out row: uses a red hex → should be color.error.500
- Section gap: looks like 14px → should be space.6
Keep layout and copy unchanged.
```

### Cross-screen consistency
```
Check these screens look consistent with each other: [screen A], [screen B], [screen C].
Focus on: navigation, typography hierarchy, button placement, empty/loading/error styles, spacing.
```

### Accessibility check
```
Accessibility check on [screen/component] for [web | mobile].
Cover contrast, touch targets, color-as-sole-indicator, focus states, text sizing.
```

### Pre-handoff QA
```
Is the [feature] design ready for handoff? Check completeness, annotation,
DS compliance, responsive coverage, edge cases, and interactions.
```

---

## 📦 ds-handoff — Preparing for Developers

### Generate a handoff note
```
Write a handoff note for the [feature] design.
Figma file: [URL]. DS version: [vX.X.X].
[Mention any images and whether they're static or dynamic.]
```

**Filled example:**
```
Write a handoff note for the User Profile feature.
Figma file: https://figma.com/file/abc123. DS version: v1.2.0.
The AppBar cover image is dynamic (comes from the user API) — needs a fallback.
```

### Quick handoff for a single screen
```
Document the [screen] for the developer. Include components used, all states,
asset sources, and any non-obvious behavior.
```

---

## 💡 Tips that make skills work best

**Describe by component, not by color.**
- ❌ "a blue button at the bottom"
- ✅ "a Button variant=primary/lg at the bottom"

**Name your states up front.** If you don't say which states you need, you'll get default only and the developer will have to ask.

**Iterate freely in the same conversation.** No need to re-paste the URL or re-describe the DS:
```
"Make the AppBar taller."
"Now add a notification badge to the avatar."
"Switch the layout to two-column for tablet."
```

**Let the skill flag gaps.** If you ask for something the DS doesn't have, the skill tells you instead of inventing it. That's your cue to propose it via governance — not to force a workaround.

**Reference existing work:**
```
"Use the same Card layout as the Dashboard, but add a status badge."
```

---

## What you never need to do anymore

| Old way (pre-skills) | Now |
|----------------------|-----|
| Paste the DS Context Block every prompt | Skill fetches it from the URL |
| Copy a prompt template and fill every field | Just describe the task |
| List all tokens/constraints in the prompt | Skill loads them from the live DS |
| Remember which constraint applies | Skill enforces all of them |

The structured templates from the old `prompts/` library are gone because the skills do that structuring internally. These examples are just to show the *kind* of thing to say — your real requests can be as natural as talking to a colleague.
