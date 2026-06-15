# Token Pipeline & CI Enforcement
> Layer 4 · Automation · Make the DS enforce itself — not human discipline alone

**For:** Developer / DevOps
**Principle:** AI prompt audits = safety net · **Pipeline + CI = the primary gate**

---

## Why this layer exists

Manual prompt-based enforcement (Layer 3) is good, but has weak points:
- People forget to paste the context block
- AI can miss things (especially in large files)
- The DS changes and code doesn't follow (drift)

Production-grade = **tokens have a single source of truth, and CI blocks hardcoding automatically**

```
Figma Variables (source of truth)
       │  export (Tokens Studio / Figma REST API / MCP)
       ▼
  tokens.json  ←── committed to git, reviewed via PR
       │  Style Dictionary build
       ▼
┌──────┬──────────┬──────────────┐
│ CSS  │ TS/JS    │ Dart/Flutter │   ← generated; never edit by hand
│ vars │ tokens.ts│ theme.dart   │
└──────┴──────────┴──────────────┘
       │
       ▼
   CI lint: block hex/px hardcoding in PRs
```

---

## 1. Source of Truth: Figma Variables

Set up Figma Variables covering every token in `design-system.md`:

| Collection | Contents |
|-----------|----------|
| `Primitives` | raw values: color scales, spacing, radius, font sizes |
| `Semantic` | aliases: `color.action.default` → `primary.500`, `color.text.primary` → `neutral.900` |

**Rule:** Designers use Variables only in Figma — never fill with raw hex (Figma then tracks usage for you).

---

## 2. Export: Figma → tokens.json

Options (pick one):

| Method | Best for |
|--------|---------|
| **Tokens Studio plugin** + sync to a git repo | Teams where designers own tokens — push straight to the repo via PR |
| **Figma REST API** (`GET /v1/files/:key/variables/local`) + script | Teams wanting full automation (Variables API requires an Enterprise plan) |
| **Figma MCP + Claude Code** | Use prompt #5 in `prompts/figma-mcp.md` to pull and diff — good for small teams |

Standard output format (W3C Design Tokens / Style Dictionary format):

```json
{
  "color": {
    "primary": {
      "500": { "value": "#3B82F6", "type": "color" },
      "600": { "value": "#2563EB", "type": "color" }
    }
  },
  "space": {
    "4": { "value": "16px", "type": "dimension" }
  }
}
```

---

## 3. Build: Style Dictionary

```bash
npm install -D style-dictionary
```

`config.json`:

```json
{
  "source": ["tokens/**/*.json"],
  "platforms": {
    "css": {
      "transformGroup": "css",
      "buildPath": "src/styles/",
      "files": [{ "destination": "tokens.css", "format": "css/variables" }]
    },
    "ts": {
      "transformGroup": "js",
      "buildPath": "src/styles/",
      "files": [{ "destination": "tokens.ts", "format": "javascript/es6" }]
    },
    "flutter": {
      "transformGroup": "flutter",
      "buildPath": "lib/theme/",
      "files": [{ "destination": "tokens.g.dart", "format": "flutter/class.dart" }]
    }
  }
}
```

**Rules:**
- Generated files carry the header `// GENERATED — DO NOT EDIT. Source: tokens/*.json`
- Changing a token = change Figma Variables → export → PR — **never edit generated files directly**
- Tailwind: import the tokens into `tailwind.config` instead of redefining them

---

## 4. CI Gate: Block Hardcoding

### ESLint (React/Next.js)

```bash
npm install -D eslint-plugin-design-tokens   # or write a custom rule
```

Minimal custom rule concept — block hex literals in style props/styled-components:

```js
// eslint rule: no-hardcoded-design-values
// fail on: #[0-9a-fA-F]{3,8}, rgb(, rgba( in JSX style / css template literals
// fail on px values not in [4,8,12,16,20,24,32,40,48,64,80,96]
```

### Stylelint (CSS/SCSS)

```json
{
  "plugins": ["stylelint-declaration-strict-value"],
  "rules": {
    "scale-unlimited/declaration-strict-value": [
      ["/color/", "fill", "stroke", "background", "font-size", "border-radius", "box-shadow"],
      { "ignoreValues": ["transparent", "inherit", "currentColor", "none"] }
    ]
  }
}
```

→ forces those properties to be `var(--token)` only

### Flutter

```yaml
# analysis_options.yaml + custom_lint
# block: Color(0xFF...), EdgeInsets not using AppSpacing, TextStyle not using AppTypography
```

### GitHub Actions

```yaml
name: ds-compliance
on: [pull_request]
jobs:
  token-lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: npm ci
      - run: npm run lint:tokens        # the eslint + stylelint rules above
      - run: npm run build:tokens       # style-dictionary build
      - run: git diff --exit-code src/styles/  # fail if generated files are out of sync with tokens.json
```

---

## 5. Visual Regression (optional but recommended)

Catches DS drift that lint can't (layout/spacing off):

| Tool | Best for |
|------|---------|
| **Chromatic** (+ Storybook) | React — snapshot every component in every state, diff in PRs |
| **Playwright screenshots** | Teams already using Playwright MCP — `expect(page).toHaveScreenshot()` |
| **golden tests** | Flutter — `matchesGoldenFile()` |

Workflow: every new component needs a story/golden for every state (default/hover/disabled/loading) before merge.

---

## 6. Drift Detection Schedule

| Frequency | What | Who |
|-----------|------|-----|
| Every PR | CI token lint + generated-file sync check | Automated |
| Every sprint | Run the Token Audit prompt (#4 in `figma-mcp.md`) on changed files | Developer |
| Every DS release | Sync Figma Variables → tokens.json → regenerate all platforms | Maintainer |
| Every quarter | Review DS exceptions + deprecated components | DS Owner |

---

## Setup Checklist (once per repo)

- [ ] Figma Variables covering every token in `design-system.md` (Primitives + Semantic)
- [ ] Export method chosen and tokens.json in the repo
- [ ] Style Dictionary installed + configured for your platforms
- [ ] Lint rules installed (ESLint/Stylelint/custom_lint)
- [ ] GitHub Actions workflow added
- [ ] `CLAUDE.md` at the repo root (see `templates/CLAUDE.md.template`)
- [ ] (Optional) Storybook + Chromatic or golden tests
