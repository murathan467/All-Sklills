# AGENTS.md — Flutter & Dart Orchestration Rules
**(Graphify + Addy Skills + Ponytail + Official Flutter/Dart Skills + UI/UX Pro Max)**

This repository uses a 4-phase agent architecture tailored for **Flutter & Dart** applications. Follow the strict hierarchy and conflict-resolution rules below for every task.

---

## 1. System Hierarchy & Phase Routing

1. **Discovery & Context (ALWAYS FIRST):** Governed by **`graphify`**
2. **Specification, Design Tokens & Planning:** Governed by **`addyosmani/agent-skills`** + **`ui-ux-pro-max`**
3. **Code Implementation (Flutter & Dart):** Governed by **`flutter-*` / `dart-*` skills** + **`ponytail` (`full` mode with Widget Override)**
4. **Verification, Testing & Review:** Governed by **`dart-run-static-analysis`** + **`addyosmani/agent-skills`** + **`ponytail-review`**

---

## 2. Phase 1: Context & Codebase Discovery (`graphify`)

Before using raw `grep`, `find`, or reading multiple `.dart` files sequentially:

- **Check for Graph:** If `graphify-out/graph.json` exists, ALWAYS query the knowledge graph first:
  - Concept & relationship lookup: `graphify query "<question>"`
  - Symbol connections & callers: `graphify explain "<WidgetOrClassName>"`
  - Dependency/call path between two modules: `graphify path "<ScreenOrBloc>" "<RepositoryOrService>"`
- **Read Only What Matches:** Open only the exact files and line ranges returned by `graphify`.
- **Stale Graph Guard:** After creating new `.dart` files, widgets, or classes, run `graphify update .` before relying on graph queries in subsequent steps.

---

## 3. Phase 2: Spec, Design & Flutter Skill Routing

Always invoke lifecycle and Flutter/Dart skills explicitly by their skill name:

- **Underspecified requirements** -> Activate `interview-me` or `spec-driven-development`
- **Multi-file implementation plan** -> Activate `planning-and-task-breakdown`
- **Visual design system, color palettes, typography (`design-system/MASTER.md`)** -> Activate `ui-ux-pro-max` ONLY for visual token decisions; map those tokens into Flutter's `ThemeData` / `ColorScheme` / `ThemeExtension`.
- **Layered architecture (UI / Logic / Data, Repository, Bloc/Cubit/Riverpod/Notifier)** -> Activate `flutter-apply-architecture-best-practices`
- **Responsive & adaptive layouts (`LayoutBuilder`, `MediaQuery`, mobile/tablet/desktop)** -> Activate `flutter-build-responsive-layout`
- **Fixing layout errors (`RenderFlex overflowed`, `unbounded height/width`)** -> Activate `flutter-fix-layout-issues`
- **Declarative navigation & deep linking (`go_router`)** -> Activate `flutter-setup-declarative-routing`
- **Dart 3 `sealed class` states, switch expressions & destructuring** -> Activate `dart-use-pattern-matching`
- **Widget tests (`WidgetTester`) & static analysis (`dart analyze`)** -> Activate `flutter-add-widget-test` and `dart-run-static-analysis`

---

## 4. Phase 3: Implementation & Conflict Resolution (`flutter` vs `ponytail`)

### Rule A — `const` Widget Decomposition Overrides Ponytail Collapsing
- **Conflict:** Ponytail instructs to inline single-caller functions/classes into one place.
- **Resolution:** In Flutter (`.dart`), inlining sub-widgets into a single giant `build()` method or converting `StatelessWidget` classes into `_buildSection()` helper methods **destroys Flutter's element tree caching and `const` rebuild optimization**.
  1. **NEVER** replace extracted `StatelessWidget` classes (with `const` constructors) with helper methods or inline giant `build()` trees.
  2. **NEVER** collapse UI (`Widget`), State Management (`Bloc`/`Cubit`/`Notifier` + `sealed class` States), and Data (`Repository`/`DataSource`) layers into one class.
  3. Apply Ponytail's minimalism *inside* method bodies and business logic.

### Rule B — Design System Boundary (`ui-ux-pro-max` vs Flutter Theme)
- Use `ui-ux-pro-max` to select colors, contrast ratios, spacing, and typography; implement them exclusively via Flutter's `Theme.of(context).colorScheme` and `textTheme` (never hardcode hex colors inside widget trees).

### Rule C — Reuse via Graphify
- Before creating a new custom widget, extension, or formatter, run `graphify query` to check if an existing reusable widget or utility already exists in `lib/`.

---

## 5. Phase 4: Verification & Pre-Merge Gates

1. Run `git diff --stat` and verify adherence to `incremental-implementation`.
2. Run `dart analyze` (via `dart-run-static-analysis`) and `flutter test` (`debugging-and-error-recovery` if anything fails).
3. Execute `code-review-and-quality` and `ponytail-review` self-checks before final output.

