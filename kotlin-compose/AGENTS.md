# AGENTS.md — Kotlin & Jetpack Compose / KMP Orchestration Rules
**(Graphify + Addy Skills + Ponytail + Compose Skill Kit + Kotlin Tooling + UI/UX Pro Max)**

This repository uses a 4-phase agent architecture tailored for **Android Jetpack Compose & Kotlin Multiplatform (KMP)**. Follow the strict hierarchy and conflict-resolution rules below for every task.

---

## 1. System Hierarchy & Phase Routing

1. **Discovery & Context (ALWAYS FIRST):** Governed by **`graphify`**
2. **Specification, Design Tokens & Planning:** Governed by **`addyosmani/agent-skills`** + **`ui-ux-pro-max`**
3. **Code Implementation (Compose & Kotlin):** Governed by **`compose` (entry tree)** + **`ponytail` (`lite` body mode)**
4. **Verification, Testing & Review:** Governed by **`addyosmani/agent-skills`** + **`ponytail-review`**

---

## 2. Phase 1: Context & Codebase Discovery (`graphify`)

Before using raw `grep`, `find`, or reading multiple `.kt` files sequentially:

- **Check for Graph:** If `graphify-out/graph.json` exists, ALWAYS query the knowledge graph first:
  - Concept & relationship lookup: `graphify query "<question>"`
  - Symbol connections & callers: `graphify explain "<ClassNameOrComposable>"`
  - Dependency/call path between two modules: `graphify path "<ScreenOrViewModel>" "<RepositoryOrDao>"`
- **Read Only What Matches:** Open only the exact files and line ranges returned by `graphify`.
- **Stale Graph Guard:** After creating new `.kt` files, classes, or composables, run `graphify update .` before relying on graph queries in subsequent steps.

---

## 3. Phase 2: Spec, Design & Compose Skill Routing

Always invoke lifecycle and Compose skills explicitly by their skill name:

- **ANY Jetpack Compose or KMP task (MANDATORY ENTRY POINT):** ALWAYS load `compose` first. Let `compose` route to the exact sub-skill:
  - `@Composable` UI, Material 3, adaptive `WindowSizeClass`, animations, stability/recomposition -> `compose-ui`
  - New screen/sheet/dialog slice, `Contract.kt`, `ViewModel`, `SavedStateHandle`, Navigation 3 -> `compose-feature`
  - Module graph, MVI `BaseViewModel`, error tiers, Koin/Hilt DI, coroutines -> `compose-architecture`
  - Repositories, DTO -> Domain -> UiModel mappers, Ktor, Room, DataStore, Paging 3 -> `compose-data`
  - Gradle setup, `libs.versions.toml`, `build-logic` convention plugins -> `compose-project`
  - `commonMain` vs `androidMain`/`iosMain` `expect`/`actual` splits -> `compose-platform`
- **Visual design system, color palettes, typography (`design-system/MASTER.md`):** Activate `ui-ux-pro-max` ONLY for visual token decisions; implement those tokens using `compose-ui`.
- **Special Tooling Migrations (Manual/On-Demand ONLY):**
  - Upgrading project to AGP 9.0+ -> `kotlin-tooling-agp9-migration`
  - Converting `.java` files to `.kt` -> `kotlin-tooling-java-to-kotlin`
  - Fixing slow Kotlin/Native or iOS XCFramework builds -> `kotlin-tooling-native-build-performance`

---

## 4. Phase 3: Implementation & Conflict Resolution (`compose-skill` vs `ponytail`)

### Rule A — `compose-skill` Architecture Overrides Ponytail Collapsing
- **Conflict:** Ponytail instructs to inline single-caller functions, remove wrappers, and avoid type conversions.
- **Resolution:** In `.kt` files, **`compose-skill` rules strictly override Ponytail**:
  1. **NEVER** collapse `Route` (stateful / `ViewModel`-bound) and `Screen` (stateless / `@Preview`-ready) composables into a single function.
  2. **NEVER** inline MVI `State`, `Event`, and `Effect` contracts (`Contract.kt`).
  3. **NEVER** bypass `DTO` -> `Domain` -> `UiModel` mapping boundaries defined in `compose-data` (keeps network/Room types out of Compose stability).
  4. Apply Ponytail (`lite` mode) **only inside function bodies** to avoid dead code, speculative config flags, and duplicate helper logic.

### Rule B — Gradle & Tooling Boundary (`compose-project` vs `kotlin-tooling-*`)
- Default all day-to-day Gradle module creation, `libs.versions.toml` updates, and convention plugin edits to `compose-project`.
- Do NOT load `kotlin-tooling-agp9-migration` unless explicitly migrating an older project to AGP 9.

### Rule C — Reuse via Graphify
- Before writing any mapper, extension function, or custom composable component, run `graphify query` to reuse existing project utilities and design-system components.

---

## 5. Phase 4: Verification & Pre-Merge Gates

1. Run `git diff --stat` and verify adherence to `incremental-implementation`.
2. Run Gradle test/lint tasks (`debugging-and-error-recovery` if anything fails).
3. Execute `code-review-and-quality` and `ponytail-review` self-checks before final output.

