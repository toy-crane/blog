---
name: draft-plan
description: Create an implementation plan (artifacts/<feature>/plan.md) based on spec.md. **Use only for product features that are ambiguous, multi-file, or take over 30 minutes — NOT for meta-tooling (skills, rules, hooks, repo config), single-line fixes, or unambiguous changes.** Discover related skills and generate a TDD-based task list with vertical slicing and dependency ordering. Triggered by "/draft-plan", "create plan", "implementation plan", etc.
argument-hint: "feature name"
---

# Create Implementation Plan

## Step 1: Check Prerequisites

Extract the feature name from $ARGUMENTS.

Required (per-feature):
- `artifacts/<feature>/spec.md` -- If missing, output "Please run `/write-spec <feature>` first." and stop

Optional (per-feature):
- `artifacts/<feature>/wireframe.html`

## Step 2: Enter Plan Mode

Operate in read-only mode. Do not create, modify, or delete any project files.

The only file output from this skill is `artifacts/<feature>/plan.md`.

## Step 3: Codebase Exploration

Explore the existing code to understand the architecture and related patterns.

- Check project structure, existing components, and state management approach
- Identify files this feature will affect and their dependency relationships
- Map dependencies between components — what depends on what
- If similar existing functionality exists, reference its implementation
- Note risks and unknowns

## Step 4: Discover Skills

Scan `.claude/skills/` and select every skill that has even a slight connection to this feature.
When in doubt, include it — builders can ignore what they don't need.

**Always include** (regardless of feature):
- `test-driven-development` — any task that adds or modifies behavior. Drives RED → GREEN discipline and criterion-to-test mapping. See also `CLAUDE.md` → Testing for the project's success-criteria principle, stack, and placement rules.

## Step 5: Fill in the Blanks

Read the above inputs and find items that are needed for implementation but not yet decided.

- Only ask about decisions with high cost of change
- One question at a time, present 2-4 options

## Step 6: Generate Plan Document

Read each confirmed skill's SKILL.md. The plan must not contradict rules that builders will load during execution.

Read `references/plan-template.md` and write following its format.

### Task Writing Principles

#### Vertical Slicing

Each task must be a vertical slice delivering working, testable functionality through one complete path — not a horizontal layer.

Bad (horizontal slicing):
```
Task 1: Define all content collection schemas
Task 2: Build all layout components
Task 3: Build all pages
Task 4: Wire everything up
```

Good (vertical slicing):
```
Task 1: Reader can open a post page (collection entry + layout + page)
Task 2: Reader can browse the post list (index page + card component)
Task 3: Reader can filter posts by tag (tag page + filter logic)
```

#### Task Sizing

Target S (1-2 files) or M (3-5 files). Never L or larger.

Break a task down further when:
- Acceptance criteria need more than 3 bullets
- It touches 2 or more independent subsystems
- The title contains "and" (sign it is two tasks)

#### Acceptance

Each task's **Acceptance** section is a flat checklist of natural-language outcomes derived from the Success Criteria of the scenarios listed in **Covers**. One bullet per Success Criteria covered by this task. Use concrete values from the spec — paraphrasing is allowed but the outcome must remain externally observable.

**Each acceptance bullet must map 1:1 to a test case that proves it.** Pick the lowest boundary where the criterion is actually provable — if a mock would obscure what the criterion is about, don't mock there (see `CLAUDE.md` → Testing).

The task's **Covers** line names which scenarios are addressed and whether coverage is full or partial. If partial, note which subset (e.g. "happy path only", "validation only").

#### Verification

Each Acceptance bullet must name **how it will be verified** — a command, an MCP step, or a specific human review. The bullet is not done until a future reader can re-run that check independently. Pick the lowest provable boundary.

| Provable in | Use |
|---|---|
| Code (DOM, function, HTTP) | Vitest / `bunx astro check` / `bun run build` |
| Real browser, repeatable in CI | Playwright (`bun run test:e2e`, if configured) |
| Real browser, one-shot with evidence | Browser MCP (`mcp__claude-in-chrome__*`) |
| Cannot be automated (design judgment, screen-reader AT, cross-browser feel, perf threshold absent from tooling) | Human review — state the reviewer/role, the artifact, and the criterion. Save evidence (screenshot/video/note) to `artifacts/<feature>/evidence/`. |

`manual: visit X` as a placeholder for a check that *could* be automated is not allowed. A named human review *is* — the test is "can someone else re-run this check from your bullet alone?"

For the concrete shape of the Verification block, see `references/plan-template.md`.

#### Ordering

- Place test file generation first (colocated `<file>.test.ts(x)` next to the source — see `CLAUDE.md` → Testing). If prerequisite work is needed, place it before with a reason
- Order tasks starting with those that have the fewest dependencies
- Place high-risk tasks early (fail fast)
- Each task must leave the system in a working state

#### Checkpoint Discipline

Insert a checkpoint after every 2-3 tasks. A checkpoint verifies: all tests pass, build succeeds, and the vertical slice works end-to-end.

#### Wireframe Integration

- If wireframe.html exists, reflect component types in the task's implementation targets
- For components identified in the wireframe that don't exist in the project, check package registry for installability before implementing directly

#### Other

- Reflect codebase exploration results in the Affected Files section
- Task references include only external sources that the executor cannot find on their own (for skills, only name + keywords)

Save as `artifacts/<feature>/plan.md`.

## Step 7: Present for Human Review

Present the complete plan.md to the user. Ask for approval or revision requests. Apply any requested changes. Do not proceed until the user approves.

## Done


Inform the user whether to proceed with `/execute-plan <feature>`.
