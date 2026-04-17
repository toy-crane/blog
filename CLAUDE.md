## Site

- Blog project (Astro)

## Workflow Phases

Define → Sketch → Plan → Build

### Spec-Driven Development

- Use for product features that are ambiguous, multi-file, or take over 30 minutes.
- Skip for single-line fixes, unambiguous changes, or meta-tooling (skills, rules, hooks, repo config).

| Phase   | Skill / Command            |
|---------|----------------------------|
| Specify | `/write-spec` skill        |
| Sketch  | `/sketch-wireframe` skill  |
| Plan    | `/draft-plan` skill        |
| Build   | `/execute-plan` skill      |

Each phase has a human review gate. Do not advance until the current phase is validated.

## Development Workflow

- Package manager: `bun`
- Framework: Astro (with React islands via shadcn/ui)

### Commit Rules
- Commit per logical unit of work; do not mix unrelated changes
- Conventional Commits: `feat:`, `fix:`, `docs:`, `refactor:`, `test:`, `chore:`

### Question Rules
- Always use the AskUserQuestion tool for multiple-choice questions

## Testing

### Principle
**Define success criteria. Loop until verified.**

Every change needs measurable success criteria — concrete input → observable outcome. Each criterion must have a test that proves it. Without a passing test, the criterion is not met.

Pick the lowest boundary where the criterion is actually provable. If a mock would obscure what the criterion is about (content collection schema, routing, real DOM), don't mock there — let the test hit the real dependency.

### Stack & file placement
- **Vitest** (jsdom, `@testing-library/react` for islands) — colocated `<file>.test.ts(x)` next to the source (e.g. `src/components/post-card.test.tsx`, `src/lib/slugify.test.ts`)
- **Playwright** (optional, add when needed) — `e2e/*.spec.ts` (not colocated; flows span pages)

### Naming
Test names describe the behavior in natural language — do not use scenario IDs.

### Commands
| Command | Scope |
|---------|-------|
| `bun run test` | Vitest |
| `bun run test:watch` | Vitest watch mode |
| `bunx astro check` | Astro type + content schema check |
| `bun run build` | Production build |
| `bun run dev` | Dev server |
| `bun run test:e2e` | Playwright (if configured) |

## Architecture — Astro

### Layout
- `src/pages/` — route files (`.astro`, `.md`, `.mdx`)
- `src/layouts/` — shared layouts
- `src/components/` — reusable components (`.astro` + React islands via shadcn)
- `src/content/` — content collections with Zod schemas (`src/content/config.ts`)
- `src/lib/` — framework-agnostic helpers
- `src/styles/` — global CSS / Tailwind entry

### Rules
- Prefer `.astro` components; reach for React islands only when interactivity requires it
- Island hydration: default to `client:visible` or `client:idle`; use `client:load` only when the island must run immediately
- Content collections are the source of truth for posts/data — define schemas in `src/content/config.ts` and use `getCollection()` / `getEntry()`
- Shared logic belongs in `src/lib/`; never duplicate across components

### shadcn/ui
- Follow the rules in `.claude/skills/shadcn/rules/` when writing React islands (see also `.claude/rules/shadcn-guard.md`)
- Install/update components via `bunx --bun shadcn@latest add <component>` — never edit `src/components/ui/*` source files directly

## Boundary

### Ask-first
- Adding new external dependencies
- Switching the shadcn preset or base (radix ↔ base)
- Changing the deployment target / adapter
