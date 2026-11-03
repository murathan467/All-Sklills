# AGENTS.md — Antigravity Orchestration Rules (Graphify + Addy Skills + Ponytail)

This repository uses three complementary agent systems. Follow the strict hierarchy and conflict-resolution rules below for every task.

---

## 1. System Hierarchy & Phase Routing

Each phase of work is governed by a specific toolset. Do not mix conflicting rules across phases:

1. **Discovery & Context (ALWAYS FIRST):** Governed by **`graphify`**
2. **Specification & Planning:** Governed by **`addyosmani/agent-skills`**
3. **Code Implementation (Diff Discipline):** Governed by **`ponytail`** (default: `full` mode, NOT `ultra`)
4. **Verification, Testing & Review:** Governed by **`addyosmani/agent-skills`**

---

## 2. Phase 1: Context & Codebase Discovery (`graphify`)

Before using raw `grep`, `find`, or reading multiple source files sequentially:

- **Check for Graph:** If `graphify-out/graph.json` exists, ALWAYS query the knowledge graph first:
  - Concept & relationship lookup: `graphify query "<question>"`
  - Symbol connections & callers: `graphify explain "<NodeName>"`
  - Dependency/call path between two modules: `graphify path "<NodeA>" "<NodeB>"`
- **Read Only What Matches:** Open only the exact files and line ranges returned by `graphify`. Never read `GRAPH_REPORT.md` or `graph.json` in full unless performing a broad architectural audit.
- **Stale Graph Guard:** After creating new files, classes, or public functions, ensure the graph is updated (`graphify .` or via active `graphify watch` / git hooks) before relying on graph queries in subsequent steps.

---

## 3. Phase 2: Spec & Planning (`addyosmani/agent-skills`)

In Antigravity (`agy`), legacy slash command wrappers (`/plan`, `/build`) may conflict with built-in commands. Always invoke lifecycle skills explicitly by their skill name:

- Underspecified requirements -> Activate `interview-me` or `spec-driven-development`
- Multi-file implementation plan -> Activate `planning-and-task-breakdown`
- API / contract changes -> Activate `api-and-interface-design`
- UI / frontend / mobile views -> Activate `frontend-ui-engineering`

---

## 4. Phase 3: Implementation & Conflict Resolution (`ponytail` vs `agent-skills`)

During code generation, apply **`ponytail` (`full` level)** rules with the following explicit overrides to prevent clashes with `addyosmani/agent-skills`:

### Rule A — Architectural Boundaries Override YAGNI

- **Ponytail Rule:** Do not invent speculative wrappers, helper classes, interfaces, or config layers; keep diffs minimal and inline single-use logic.
- **Override:** If the repository already enforces a structured architecture (e.g., Clean/Onion Architecture, CQRS Commands/Queries/Handlers, Repository pattern, or MVVM/Bloc state boundaries), **respect the existing layer boundaries**. Do not bypass established architectural layers just to save lines of code. Apply Ponytail's minimalism _inside_ each required layer.

### Rule B — Reuse via Graphify (No Duplicate Helpers)

- Before writing any utility, mapper, validation, or helper function, run `graphify query` to check if an equivalent already exists in the codebase. Reuse existing project utilities.

### Rule C — Testing Resolution (`test-driven-development` vs `ponytail`)

- **Core Business Logic, Bug Fixes & API Contracts:** `addyosmani/agent-skills` (`test-driven-development`) takes precedence. Follow Red-Green-Refactor and verify edge cases.
- **Simple Wiring, UI Layout Tweaks & Config Changes:** `ponytail` takes precedence. Do not write redundant boilerplate unit tests for trivial pass-through code or framework getters.

### Rule D — Skill Deduplication

- While `ponytail` is active, **do not load** `code-simplification` from `addyosmani/agent-skills`. Ponytail already enforces YAGNI, small diffs, and anti-bloat rules during coding; loading both wastes context tokens.

---

## 5. Phase 4: Verification & Pre-Merge Gates (`addyosmani/agent-skills`)

Before marking any multi-file task complete:

1. Run `git diff --stat` and verify the change adheres to `incremental-implementation` (small, verifiable slice, zero unrelated formatting changes).
2. Run relevant test suites and build checks (`debugging-and-error-recovery` if anything fails).
3. For auth, user input, database queries, or external APIs, enforce `security-and-hardening` checks.
4. Execute `code-review-and-quality` self-check before final output.
