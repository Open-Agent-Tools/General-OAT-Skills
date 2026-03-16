---
name: qa
description: >-
  Run a comprehensive multi-agent QA audit of any project. Auto-detects
  language and framework, then launches parallel agents to audit code quality,
  architecture, error handling, test coverage, CI/CD, documentation, and
  security. Use for pre-release validation, periodic quality reviews, or
  when the user asks for a full project audit.
disable-model-invocation: true
allowed-tools:
  - Agent
  - Bash
  - Read
  - Grep
  - Glob
  - Write
argument-hint: "[optional: focus area — e.g. \"tests\", \"security\", \"docs\", \"ci\"]"
user-invocable: true
---

# Multi-Agent QA Audit

Run a comprehensive quality audit of the current project. Auto-detect the
language(s), framework(s), and tooling, then launch all audit agents in
parallel for maximum speed.

## Scope

If $ARGUMENTS is provided, narrow the audit to that area (e.g. "tests" →
only test coverage, "security" → only security audit, "docs" → only
documentation accuracy). Otherwise audit the full project.

## Step 0 — Project Detection

Before any audit work, detect the project stack by reading the project root.
Identify:

1. **Languages** — check for: `Cargo.toml` (Rust), `package.json` (JS/TS),
   `pyproject.toml`/`setup.py`/`requirements.txt` (Python),
   `go.mod` (Go), `pom.xml`/`build.gradle` (Java/Kotlin),
   `*.csproj`/`*.sln` (C#/.NET), `Gemfile` (Ruby), `mix.exs` (Elixir),
   `Package.swift` (Swift), `CMakeLists.txt`/`Makefile` (C/C++).
2. **Frameworks** — check for framework indicators in config files and
   imports (React, Vue, Angular, Django, Flask, FastAPI, Rails, Spring,
   Next.js, Nuxt, SvelteKit, Tauri, Electron, etc.).
3. **Build/lint/test tooling** — identify available commands (npm/yarn/pnpm,
   cargo, uv/pip/poetry, go, make, gradle/maven, dotnet, mix, etc.).
4. **CI system** — check for `.github/workflows/`, `.gitlab-ci.yml`,
   `Jenkinsfile`, `.circleci/`, `bitbucket-pipelines.yml`, etc.
5. **Project conventions** — read `CLAUDE.md`, `.editorconfig`,
   linter configs, and any style guides present.

Store the detected stack context. All subsequent agents receive this context
so they can tailor their checks to the actual languages and frameworks in use.

## Step 1 — Build, Lint & Test (run first, blocking)

Based on the detected tooling, run the project's build, lint, and test
commands in parallel using the Bash tool. Adapt to what exists — skip
commands that don't apply. Examples by language:

| Language   | Build                  | Lint                        | Test                |
|------------|------------------------|-----------------------------|---------------------|
| Rust       | `cargo build`          | `cargo clippy -- -D warnings` | `cargo test`     |
| TypeScript | `npx tsc --noEmit`     | `npm run lint` (if exists)  | `npm test`          |
| Python     | `uv build` or skip     | `ruff check .` or `flake8`  | `pytest`            |
| Go         | `go build ./...`       | `golangci-lint run`         | `go test ./...`     |
| Java       | `./gradlew build`      | `./gradlew check`           | `./gradlew test`    |
| C#/.NET    | `dotnet build`         | `dotnet format --verify-no-changes` | `dotnet test` |
| Ruby       | skip                   | `rubocop`                   | `bundle exec rspec` |
| Elixir     | `mix compile`          | `mix credo`                 | `mix test`          |

If any fail, report failures but continue with remaining audit steps.

## Step 2 — Launch parallel audit agents

After Step 1 completes, launch ALL of the following agents concurrently
using the Agent tool. Pass the detected project stack context to each agent.

Each agent should report findings as a structured list of issues with file
paths and line numbers. **If an agent finds no issues in its area, return a
single line: "No findings." — do not elaborate.**

### Agent 1: Code Quality & Style

subagent_type: Explore

Audit the codebase for language-specific quality and style issues. Adapt
checks to the detected language(s):

**Universal checks (all languages):**
1. Search for TODO, FIXME, HACK, XXX comments — list them with file:line.
2. Check for dead code: unused imports, unreachable code, unused variables/
   functions. Use language-appropriate detection.
3. Check for inconsistent naming conventions within the project (e.g.,
   mixing camelCase and snake_case in the same language context).
4. Flag files exceeding 500 lines — candidates for splitting.
5. Check for magic numbers/strings that should be named constants.
6. Check for code duplication — flag blocks of 10+ near-identical lines.

**Language-specific checks:**
- **Python**: type hints on public functions, f-string vs format(), PEP 8
  compliance, `__all__` exports in `__init__.py`, proper use of
  `@dataclass`/`@attrs`/Pydantic models.
- **TypeScript/JavaScript**: `any` type usage, missing return types on
  exported functions, `==` vs `===`, console.log left in production code,
  proper async/await (no floating promises).
- **Rust**: `unwrap()`/`expect()` outside tests, `clone()` that could be
  avoided, unused `Result` values, proper use of `thiserror`/`anyhow`.
- **Go**: exported functions without doc comments, error values ignored
  (assigned to `_`), context.Background() misuse, proper error wrapping.
- **Java/Kotlin**: raw types, unchecked casts, mutable static fields,
  missing `@Override`, null-safety issues.
- **Ruby**: method length, class length, ABC complexity metrics.
- **C#**: nullable reference type warnings, async void methods, missing
  `ConfigureAwait`, `IDisposable` not disposed.

Report each violation with: rule violated, file:line, and what to fix.

### Agent 2: Architecture & Module Structure

subagent_type: Explore

Audit the project's architecture and module organization:

1. **Dependency direction** — map the import graph between top-level
   modules/packages. Flag any circular dependencies.
2. **Layer violations** — if the project has clear layers (e.g., API →
   service → data, or controller → model → view), check that dependencies
   flow in one direction. Flag any layer that imports from the wrong
   direction.
3. **Module cohesion** — flag modules that seem to do unrelated things
   (e.g., a utils file with 20+ functions spanning multiple domains).
4. **Coupling** — flag files that import from more than 10 other project
   modules (high fan-in suggests poor abstraction boundaries).
5. **Configuration** — check that config, secrets, and environment variables
   are loaded in one place, not scattered across the codebase.
6. **Entry points** — verify the project has clear entry points (main
   functions, app factories, route registrations) and they are documented.
7. **Separation of concerns** — flag any file that mixes I/O with business
   logic, or UI rendering with data fetching (unless it is the project's
   intentional pattern).

Report each issue with: the architectural concern, affected files, and
suggested refactoring.

### Agent 3: Error Handling & Robustness

subagent_type: Explore

Audit error handling patterns across the codebase:

**Universal checks:**
1. Check for bare/empty exception catches that swallow errors silently.
2. Check for overly broad exception catches (`except Exception`,
   `catch (Exception e)`, `catch {}`, `rescue => e`).
3. Check that errors include enough context for debugging (error messages
   should say what went wrong, not just "error occurred").
4. Verify that external API calls, file I/O, and network operations have
   error handling.
5. Check for resource leaks — opened files/connections/locks not properly
   closed (missing `with`, `defer`, `using`, `try-with-resources`, etc.).

**Language-specific checks:**
- **Python**: bare `except:`, `except Exception` without re-raise,
  missing `finally` for cleanup, `assert` used for validation in
  production code.
- **TypeScript/JavaScript**: `.catch()` missing on promises, `try/catch`
  with empty catch block, unhandled rejection patterns.
- **Rust**: `unwrap()` and `expect()` in non-test code, `panic!()` in
  library code, error types that don't implement `std::error::Error`.
- **Go**: `_` used to ignore errors, `panic()` in library code, errors
  not wrapped with `fmt.Errorf("...: %w", err)`.
- **Java/Kotlin**: catching `Throwable`, `e.printStackTrace()` instead
  of logging, checked exceptions swallowed.

Report each issue with: severity (critical/warning), file:line, the
problematic pattern, and the recommended fix.

### Agent 4: Test Coverage & Quality

subagent_type: Explore

Audit what is tested versus what should be tested:

1. **Discover test files** — find all test files in the project. List the
   count and location of tests by module/package.
2. **Identify untested code** — for each public module, check if a
   corresponding test file/module exists. Flag public functions, classes,
   or endpoints that have no tests.
3. **Test quality** — read a sample of test files and check for:
   - Tests that don't actually assert anything meaningful.
   - Tests with hardcoded values that don't test edge cases.
   - Flaky patterns: sleep-based waits, time-dependent assertions,
     order-dependent tests, shared mutable state between tests.
   - Tests that test implementation details rather than behavior.
4. **Critical paths** — identify the project's most critical code paths
   (entry points, API endpoints, core business logic, data
   transformations) and verify they have tests.
5. **Test infrastructure** — check for proper test fixtures, factories,
   or builders. Flag test files that duplicate setup code.
6. **Coverage gaps** — if a coverage config exists (`.coveragerc`,
   `jest.config`, `tarpaulin.toml`, etc.), note the configured thresholds
   and any excluded paths.

Report untested code with: file:line reference and suggested test case
description.

### Agent 5: CI/CD & Workflow Correctness

subagent_type: Explore

Read all CI/CD configuration files and audit for correctness:

1. **Pipeline completeness** — does CI run: build, lint, type-check, test,
   and security checks? Flag any missing stage.
2. **Environment consistency** — are language/runtime versions pinned and
   consistent across all jobs? Flag version mismatches.
3. **Secret handling** — check that no secrets, tokens, or credentials are
   hardcoded in CI files. Verify secrets are referenced via environment
   variables or secret stores.
4. **Caching** — check if dependency caching is configured (speeds up CI).
   Flag missing cache configuration for common patterns (node_modules,
   .cargo, .venv, pip cache, Maven/Gradle cache).
5. **Matrix testing** — for libraries, check if CI tests against multiple
   language/runtime versions and operating systems.
6. **Release workflow** — if a release/deploy workflow exists, verify it
   has appropriate gates (tests must pass, manual approval for production).
7. **Branch protection** — check if the CI config implies branch
   protection rules (required checks, review requirements).
8. **Action/plugin versions** — for GitHub Actions, check that action
   versions are pinned to SHA or major version (not `@master`).

If no CI configuration exists, report that as a finding and recommend
a basic CI setup for the detected language.

Report each issue with: CI file, job/step name, and what's wrong.

### Agent 6: Documentation Accuracy

subagent_type: Explore

Audit all documentation against the actual codebase:

1. **README.md** — verify:
   - Project description matches what the code actually does.
   - Install/setup instructions work (commands exist, dependencies listed).
   - Usage examples match the current API/CLI interface.
   - Badge URLs and links are not broken (check file references, not URLs).
   - Nothing described as "coming soon" that already exists, or described
     as existing that has been removed.
2. **CLAUDE.md** — if it exists, verify every stated convention, architecture
   description, and file layout claim against the actual code. Flag any
   stale or inaccurate sections.
3. **docs/ directory** — if it exists, check each doc for accuracy of
   file paths, API descriptions, and architecture diagrams.
4. **API documentation** — for libraries, check that public API docstrings/
   comments match the actual function signatures and behavior.
5. **Changelog/release notes** — if they exist, verify the latest entries
   match recent git history.
6. **Configuration docs** — verify that documented config options match
   what the code actually reads.

Report inaccuracies with: doc file, section, what's wrong, and what the
correct information should be.

### Agent 7: Security & Dependency Audit

subagent_type: Explore

Audit for security issues and dependency health:

**Code security (OWASP-informed):**
1. Search for hardcoded secrets, API keys, tokens, passwords, or
   connection strings in the codebase (check common patterns: `password=`,
   `api_key=`, `secret=`, `token=`, `-----BEGIN`).
2. Check for SQL injection vulnerabilities — raw string concatenation in
   database queries instead of parameterized queries.
3. Check for command injection — unsanitized user input passed to shell
   commands (`os.system`, `subprocess` with `shell=True`, `exec`, etc.).
4. Check for path traversal — user-controlled input used in file paths
   without sanitization.
5. Check for XSS vectors — if web framework, check for unescaped user
   input in templates/responses.
6. Check for insecure deserialization — `pickle.loads`, `yaml.load`
   (without SafeLoader), `eval()`, `JSON.parse` on untrusted input
   without validation.

**Dependency health:**
7. Check for pinned dependency versions vs floating (recommend pinning
   for applications, ranges for libraries).
8. Look for known-outdated or deprecated dependencies by checking for
   deprecation notices in config files or comments.
9. Check for vendored/copied code that should be a dependency.
10. Verify `.gitignore` excludes secrets files (`.env`, `*.pem`,
    `credentials.json`, etc.).

Report each issue with: severity (critical/high/medium/low), file:line,
the vulnerability type, and remediation steps.

### Agent 8: Project Standards & Configuration

subagent_type: Explore

Audit project configuration and standards compliance:

1. **CLAUDE.md compliance** — if `CLAUDE.md` exists, read every rule and
   verify the codebase follows it. Report each violation with the exact
   rule quoted and the violating file:line.
2. **Linter/formatter config** — verify linter and formatter configs exist
   and are consistent. Flag conflicting rules between tools.
3. **Editor config** — check `.editorconfig` if it exists. Verify the
   codebase follows its indentation, line-ending, and charset rules.
4. **Git hygiene** — check `.gitignore` for completeness. Flag any
   build artifacts, IDE files, or OS files that should be ignored but
   aren't. Check for large binary files tracked in git.
5. **License** — verify a LICENSE file exists and is referenced in
   package metadata (package.json, Cargo.toml, pyproject.toml, etc.).
6. **Required files** — check for standard project files: README.md,
   LICENSE, .gitignore. Flag any that are missing.
7. **Dependency lockfile** — verify a lockfile exists if the project uses
   a package manager (package-lock.json, yarn.lock, Cargo.lock,
   poetry.lock, uv.lock, go.sum, Gemfile.lock, mix.lock).

Report each issue with: what standard is violated and suggested fix.

---

**Each of Agents 1–8 must produce:**
1. A findings list with file:line references. Omit any area that is all
   clear — one line "No findings." is sufficient for a clean area.
2. If there are actionable improvements: specific recommendations with
   file:line references.

## Step 3 — Synthesize and write QA_Report.md

After all agents complete, write the consolidated QA report to
`QA_Report.md` in the project root using the Write tool. Print a brief
summary to stdout.

**Terseness rule:** Every section below should contain ONLY items that
need remediation. If a section has no findings, write one line:
`All clear.` Do not list what was checked, do not explain why things
are fine.

### 3.1 Build & Test Summary
Pass/fail for each check. If all pass: `Build & tests: all pass.`

### 3.2 Code Quality Issues
Grouped by language (Agent 1). Include: rule violated, file:line, fix.

### 3.3 Architecture Issues
Findings from Agent 2. Circular dependencies, layer violations, coupling
problems.

### 3.4 Error Handling Issues
Findings from Agent 3, grouped by severity (critical first).

### 3.5 Test Coverage Gaps
From Agent 4. List untested code with suggested test case descriptions.

### 3.6 CI/CD Issues
From Agent 5. Flag any workflow correctness issues.

### 3.7 Security Issues
From Agent 7, grouped by severity. Critical and high severity issues first.

### 3.8 Documentation Issues
From Agent 6. List only inaccurate or stale documentation with specific
corrections.

### 3.9 Project Standards Violations
From Agent 8. CLAUDE.md compliance, config issues, missing files.

### 3.10 Top Recommendations
Top 10 highest-impact improvements, ordered by priority. Weight security
issues, error handling gaps, and test coverage most heavily. Each item:
one sentence describing the fix + file:line reference.

Use file:line references throughout. No filler. Every sentence in the
report should describe something that needs to change.
