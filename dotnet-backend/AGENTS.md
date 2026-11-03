# AGENTS.md — .NET / C# Backend Orchestration Rules
**(Graphify + Addy Skills + Ponytail + .NET/EF Core Skills)**

This repository uses a 4-phase agent architecture tailored for **.NET / ASP.NET Core / EF Core** backends. Follow the strict hierarchy and conflict-resolution rules below for every task.

---

## 1. System Hierarchy & Phase Routing

1. **Discovery & Context (ALWAYS FIRST):** Governed by **`graphify`**
2. **Specification & Planning:** Governed by **`addyosmani/agent-skills`**
3. **Code Implementation (Diff Discipline + .NET Standards):** Governed by **`ponytail` (`full` mode)** + **`.NET Skills`**
4. **Verification, Testing & Review:** Governed by **`addyosmani/agent-skills`** + **`ponytail-review`**

---

## 2. Phase 1: Context & Codebase Discovery (`graphify`)

Before using raw `grep`, `find`, or reading multiple `.cs` files sequentially:

- **Check for Graph:** If `graphify-out/graph.json` exists, ALWAYS query the knowledge graph first:
  - Concept & relationship lookup: `graphify query "<question>"`
  - Symbol connections & callers: `graphify explain "<ClassNameOrMethod>"`
  - Dependency/call path between two modules: `graphify path "<ControllerOrEndpoint>" "<RepositoryOrDbContext>"`
- **Read Only What Matches:** Open only the exact files and line ranges returned by `graphify`. Never read `GRAPH_REPORT.md` or `graph.json` in full unless performing a broad architectural audit.
- **Stale Graph Guard:** After creating new `.cs` files, classes, interfaces, or endpoints, run `graphify update .` before relying on graph queries in subsequent steps.

---

## 3. Phase 2: Spec, Planning & .NET Skill Routing

Always invoke lifecycle and stack skills explicitly by their skill name:

- **Underspecified requirements** -> Activate `interview-me` or `spec-driven-development`
- **Multi-file implementation plan** -> Activate `planning-and-task-breakdown`
- **REST / Minimal API / OpenAPI contract design** -> Activate `api-and-interface-design` + `dotnet-webapi`
- **C# 12+ language features, records, ValueObjects, `Span<T>`, nullability** -> Activate `modern-csharp-coding-standards`
- **EF Core queries, migrations, `DbContext` lifetime, N+1 prevention** -> Activate `efcore-patterns` + `optimizing-ef-core-queries`
- **Async/await, `Channel<T>`, background workers, thread safety** -> Activate `csharp-concurrency-patterns`
- **`IServiceCollection` registration & `IValidateOptions` config** -> Activate `dependency-injection-patterns`
- **Hot-path memory/CPU optimization** -> Activate `analyzing-dotnet-performance`

---

## 4. Phase 3: Implementation & Conflict Resolution (`ponytail` vs `.NET Architecture`)

During code generation, apply **`ponytail` (`full` level)** rules with the following **mandatory .NET overrides**:

### Rule A — Clean Architecture / Vertical Slice / CQRS Override Ponytail Collapsing
- **Conflict:** Ponytail instructs to delete single-use wrappers, interfaces, and type conversions.
- **Resolution:** In .NET backends using Clean/Onion Architecture, Vertical Slices, or CQRS (MediatR `ICommand`/`IQuery`/`IRequestHandler`/FluentValidation), **NEVER** collapse Controllers/Endpoints, Command/Query Handlers, Domain Entities, and Persistence Repositories into a single class/method. **NEVER** expose EF Core Entities directly in API responses—always preserve Request/Response `record` DTOs. Apply Ponytail's minimalism *inside* each handler/method body.

### Rule B — EF Core Read vs. Write Discipline (`efcore-patterns` + `optimizing-ef-core-queries`)
- Always use `.AsNoTracking()` (or project directly via `.Select(...)` to a `record` DTO) on read-only queries.
- Always pass `CancellationToken` through every async Controller/Endpoint, Handler, and EF Core / HTTP call (`ToListAsync(ct)`, `SaveChangesAsync(ct)`).

### Rule C — Reuse via Graphify (No Duplicate Extensions/Mappers)
- Before writing any extension method, mapper, middleware, or validator, run `graphify query` to check if an equivalent already exists in the solution.

### Rule D — Testing Resolution (`test-driven-development` vs `ponytail`)
- **Domain Logic, CQRS Handlers, Validators & Bug Fixes:** `test-driven-development` takes precedence (Red-Green-Refactor).
- **Simple DI Wiring (`Program.cs`) & Trivial DTOs:** `ponytail` takes precedence (do not write boilerplate unit tests for auto-properties).

---

## 5. Phase 4: Verification & Pre-Merge Gates

Before marking any multi-file task complete:

1. Run `git diff --stat` and verify adherence to `incremental-implementation` (small, verifiable slice, zero unrelated formatting changes).
2. Run `dotnet build` and `dotnet test` (`debugging-and-error-recovery` if anything fails).
3. For auth, user input, SQL/EF queries, or external APIs, enforce `security-and-hardening` checks.
4. Execute `code-review-and-quality` and `ponytail-review` self-checks before final output.

