# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Purpose

This is a documentation-only repository containing a single process document: `README.md`, which defines the **AI Agent Development Workflow** — the organization-wide 7-phase process for AI-assisted development. It is not a codebase; there is no build system, test suite, or deployable artifact.

## Document Structure

`README.md` is the sole content file. It covers:

1. **Phase 1 — Ideation**: `/brainstorming` skill → spec document saved to `docs/superpowers/specs/YYYY-MM-DD-<topic>-design.md`
2. **Phase 2 — Design**: `DESIGN.md` creation (via getdesign.md if none exists) → Figma UX/UI design (mid-level detail, separate agent session using Figma MCP)
3. **Phase 3 — Planning**: `/writing-plans` skill → plan saved to `docs/superpowers/plans/YYYY-MM-DD-<topic>-implementation.md`
4. **Phase 4 — Implementation**: `/subagent-driven-development` + TDD (Red-Green-Refactor) + CodeGraph before every edit + Figma post-implementation verification; E2E tests for frontend after all component tasks
5. **Phase 5 — Review**: `/code-review` skill → type check → human review → final Figma comparison
6. **Phase 6 — Automated Security Review**: CI/CD via Hermes + Anthropic Cybersecurity Skills; GitHub issues with severity labels; production deployment blocked until all issues resolved
7. **Phase 7 — Delivery**: Merge → staging smoke test → production → monitor

## Key Concepts to Preserve When Editing

- **Gates are mandatory**: Spec → Plan → Implementation transitions require explicit human approval. Never soften or remove gate language.
- **Model routing**: Opus/Sonnet for Phases 1–3 and 5; GLM/DeepSeek for Phase 4 (mechanical execution); any model for Phase 6.
- **Figma detail level**: Phase 2 designs are explicitly "mid-level" — not wireframes, not pixel-perfect exhaustive variants.
- **Security severity labels**: `security:critical`, `security:high`, `security:medium`, `security:low`, `security:accepted-risk` — these exact labels are referenced in Phase 6.
- **Appendix B** contains the canonical `~/.claude/CLAUDE.md` template. Changes here affect every project that copies this template.

## File Conventions

- All flows and diagrams must use Mermaid syntax — no ASCII art.
- The document uses `---` horizontal rules between major sections.
- Table formatting uses `|---|---|` style separators.
