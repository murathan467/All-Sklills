# AGENTS.md — React & Next.js Orchestration Rules
**(Graphify + Addy Skills + Ponytail + Vercel React/Next.js Skills + UI/UX Pro Max)**

This repository uses a 4-phase agent architecture tailored for **React 19 & Next.js (App Router / TypeScript)**. Follow the strict hierarchy and conflict-resolution rules below for every task.

---

## 1. System Hierarchy & Phase Routing

1. **Discovery & Context (ALWAYS FIRST):** Governed by **`graphify`**
2. **Specification, Design Tokens & Planning:** Governed by **`addyosmani/agent-skills`** + **`ui-ux-pro-max`**
3. **Code Implementation (React/Next.js & Diff Discipline):** Governed by **`vercel-react-best-practices`** + **`frontend-ui-engineering`** + **`ponytail` (`full` mode)**
4. **Verification, Testing & Review:** Governed by **`web-design-guidelines`** + **`browser-testing-with-devtools`** + **`addyosmani/agent-skills`**

---

## 2. Phase 1: Context & Codebase Discovery (`graphify`)

Before using raw `grep`, `find`, or reading multiple `.ts`/`.tsx` files sequentially:

- **Check for Graph:** If `graphify-out/graph.json` exists, ALWAYS query the knowledge graph first:
  - Concept & relationship lookup: `graphify query "<question>"`
  - Symbol connections & callers: `graphify explain "<ComponentOrHookName>"`
  - Dependency/call path between two modules: `graphify path "<PageOrComponent>" "<ApiOrAction>"`
- **Read Only What Matches:** Open only the exact files and line ranges returned by `graphify`.
- **Stale Graph Guard:** After creating new `.ts`/`.tsx` files, components, hooks, or Server Actions, run `graphify update .` before relying on graph queries in subsequent steps.

---

## 3. Phase 2: Spec, Design & React/Next.js Skill Routing

Always invoke lifecycle and Web skills explicitly by their skill name:

- **Underspecified requirements** -> Activate `interview-me` or `spec-driven-development`
- **Multi-file implementation plan** -> Activate `planning-and-task-breakdown`
- **API / Server Action / Route Handler contracts** -> Activate `api-and-interface-design`
- **Visual design system, color palettes, typography (`design-system/MASTER.md`)** -> Activate `ui-ux-pro-max`
- **React 19 & Next.js App Router performance (RSC, waterfalls, bundle size, caching)** -> Activate `vercel-react-best-practices`
- **Reusable component APIs & avoiding boolean prop proliferation** -> Activate `vercel-composition-patterns`
- **Production DOM/CSS engineering, responsive layouts & WCAG accessibility** -> Activate `frontend-ui-engineering`
- **UI/UX compliance audit & runtime browser verification** -> Activate `web-design-guidelines` + `browser-testing-with-devtools`

---

## 4. Phase 3: Implementation & Conflict Resolution (`vercel` + `frontend-ui-engineering` vs `ponytail`)

### Rule A — Server vs. Client Component Boundary Overrides Ponytail Collapsing
- **Conflict:** Ponytail instructs to merge single-caller components into a single file/function.
- **Resolution:** In Next.js App Router, **NEVER** collapse a Client Component (`'use client'` leaf for interactivity/state) into its parent Server Component (`page.tsx` / `layout.tsx`), as doing so forces the entire route tree into the client JavaScript bundle and breaks async Server Component data fetching. Keep `'use client'` boundaries pushed down to the smallest interactive leaf components (`vercel-react-best-practices`).

### Rule B — Clear Split Between `ui-ux-pro-max`, `frontend-ui-engineering`, and `vercel-react-best-practices`
- **`ui-ux-pro-max`:** Owns aesthetic direction, color tokens, typography pairings, and `design-system/MASTER.md`.
- **`frontend-ui-engineering`:** Owns semantic HTML, CSS/Tailwind layout architecture, keyboard navigation, ARIA states, and loading/empty/error UI states.
- **`vercel-react-best-practices` & `vercel-composition-patterns`:** Own React 19 / Next.js execution model (`Suspense`, parallel `Promise.all` fetching, Server Actions, compound components, zero unnecessary `useEffect`).

### Rule C — Reuse via Graphify
- Before creating any new UI primitive, custom hook (`use*`), or utility in `lib/`/`components/`, run `graphify query` to reuse existing project components and utilities.

---

## 5. Phase 4: Verification & Pre-Merge Gates

1. Run `git diff --stat` and verify adherence to `incremental-implementation`.
2. Run `npm run build` / `tsc --noEmit` / test suites (`debugging-and-error-recovery` if anything fails).
3. Audit UI changes with `web-design-guidelines` and `code-review-and-quality` + `ponytail-review`.

