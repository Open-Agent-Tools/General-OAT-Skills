---
name: publish-rust
description: Verify and release a Rust crate or workspace. Bumps version, commits, tags, pushes. CI-driven by default; --local publishes directly.
argument-hint: major|minor|hotfix [--rc] [--local] [--no-watch]
allowed-tools: Bash, Read, Edit, Write, Glob, Grep
---

# /publish-rust

Generic Rust release skill. Runs a full verification suite locally, bumps the version, commits, creates a signed semver tag, and pushes to trigger the project's release CI. With `--local`, publishes to crates.io directly instead of relying on CI.

## Input

`$ARGUMENTS` is one of:

- `major` — bump first digit (`X.0.0`)
- `minor` — bump middle digit (`0.X.0`)
- `hotfix` — bump last digit (`0.0.X`)

Optional flags:

- `--rc` — tag a pre-release (`X.Y.Z-rc.N`). Increments N if already on an RC; drops the suffix if `--rc` is omitted. Pre-releases can be cut from any branch.
- `--local` — skip CI path; run `cargo publish` directly for each crate. Requires `CARGO_REGISTRY_TOKEN` in the environment.
- `--no-watch` — push the tag and exit without streaming the CI workflow (CI-driven mode only).

If `$ARGUMENTS` is empty or doesn't match `major|minor|hotfix`, print usage and stop.

## Config (optional)

If `.claude/publish-rust.toml` exists in the project root, read it. Supported keys (all optional):

```toml
# Branch required for stable releases (not --rc). Default: "main".
release_branch = "main"

# Tag format. Default: "v{version}".
tag_format = "v{version}"

# Override the default verification gates. If present, these REPLACE the defaults.
# Each entry is a shell command; all must pass. Env vars via "env =".
[[verify]]
name = "fmt"
cmd = "cargo fmt --all -- --check"

[[verify]]
name = "clippy"
cmd = "cargo clippy --workspace --all-features --exclude mycrate-foo -- -D warnings"
env = { RUSTFLAGS = "-D warnings" }

[[verify]]
name = "test"
cmd = "cargo test --workspace --all-features --exclude mycrate-foo"

[[verify]]
name = "doc"
cmd = "cargo doc --workspace --no-deps"
env = { RUSTDOCFLAGS = "-D warnings" }

# --local only: crates to publish, in topological order. Default: auto-detect from
# workspace Cargo.toml, skipping members with publish = false.
[local]
publish_order = ["mycrate-core", "mycrate-adapters", "mycrate-cli"]
inter_publish_sleep_secs = 20
```

Defaults if no config:

- `release_branch = "main"`
- `tag_format = "v{version}"`
- Verify suite (exactly this, in order):
  1. `cargo fmt --all -- --check`
  2. `cargo clippy --workspace --all-targets -- -D warnings`
  3. `cargo test --workspace`
  4. `cargo doc --workspace --no-deps` with `RUSTDOCFLAGS=-D warnings`
  5. For each published workspace member: `cargo publish --dry-run -p <crate> --allow-dirty`. For `publish = false` members: skip. If a dry-run fails because an upstream workspace crate isn't on crates.io, fall back to `cargo package --list -p <crate> --allow-dirty >/dev/null` and print a warning.

## Instructions

Emit short status lines (one sentence each) between steps. On any abort, print why and stop — never auto-fix, never proceed on warnings treated as errors.

### 1. Parse arguments

Parse the bump kind (`major`/`minor`/`hotfix`), `--rc`, `--local`, `--no-watch`. If invalid, print usage and stop.

### 2. Detect project structure

Read the workspace `Cargo.toml`:

- **Single crate**: `[package] version = "X.Y.Z"` directly. Bump this.
- **Workspace with shared version**: `[workspace.package] version = "X.Y.Z"`. Bump this; member crates use `version.workspace = true`.
- **Workspace with per-member versions**: each member has its own `[package] version`. Bump every member (fail if they're not all identical — the skill requires workspace-wide version parity).

Read optional `.claude/publish-rust.toml` for overrides.

### 3. Compute the new version

Let `current = X.Y.Z[-rc.N]` and `bump = major|minor|hotfix`.

- If current has `-rc.N` suffix and `--rc` is passed:
  - If bump matches the base version already being RC'd (same `X.Y.Z`), increment N.
  - Otherwise: start a new RC at the bumped base with `-rc.1`.
- If current has `-rc.N` suffix and `--rc` is NOT passed, and bump position matches the RC'd base: drop the suffix, tag the stable version as-is. (E.g. `0.8.0-rc.2` + `minor` → `0.8.0`.)
- If current is clean (no RC) and `--rc` is passed: bump + append `-rc.1`.
- If current is clean and `--rc` is not passed: plain bump.

New tag = `tag_format.replace("{version}", new_version)`.

### 4. Preconditions (all abort on failure, no auto-fix)

Run these checks in order. Print each as "→ checking X ... OK" or "→ checking X ... FAIL: <reason>" and stop on the first failure.

1. **git available and inside a git repo** (`git rev-parse --is-inside-work-tree`).
2. **Working tree clean** (`git status --porcelain` empty).
3. **On the release branch** (`git branch --show-current` == `release_branch`), unless `--rc` is passed (RCs allowed from any branch).
4. **Branch up-to-date with remote**: `git fetch origin <branch>`, then compare `HEAD` with `origin/<branch>`. Must be equal (neither ahead nor behind).
5. **Tag doesn't exist**: `git rev-parse <new_tag>` fails locally AND `git ls-remote --tags origin <new_tag>` is empty.
6. **CHANGELOG.md entry** (skip if file doesn't exist): grep for `^## \[v?<new_version>\]` in `CHANGELOG.md`. If the file exists and has no such heading, abort with the exact heading the user needs to add.
7. **CI release workflow exists** (CI-driven mode only, warn-only): check for any `.github/workflows/*.yml` with `on: push: tags:` matching `v*`. If none, print a warning but continue. Skip this check entirely with `--local`.
8. **`cargo` available**: `command -v cargo`.
9. **For `--local`**: `CARGO_REGISTRY_TOKEN` set in the environment. If not, abort.

### 5. Run verification gates

Run each gate from config (or the defaults listed above) in order. Stream stdout/stderr to the user. On the first failure, abort and print:

```
Verify gate failed: <name>
Command: <cmd>
Exit: <code>

Fix the underlying issue and re-run /publish-rust <args>. The skill has not modified any files.
```

Do not catch or retry. Do not continue on failure.

### 6. Bump versions and commit

Edit the version in the right location(s) per step 2. Leave every other line untouched.

Stage only the edited `Cargo.toml` files plus `Cargo.lock` (run `cargo check --workspace` first to update the lockfile, then stage it). Do not `git add -A`.

Commit with:

```
chore: release <new_tag>
```

Respect the user's git config for `commit.gpgsign` (don't pass flags either way).

### 7. Create the tag

Create an annotated tag:

```
git tag -a <new_tag> -m "Release <new_tag>"
```

If `git config tag.gpgsign` is `true`, use `-s` instead of `-a`. Otherwise `-a`. Never force-overwrite an existing tag — preconditions already verified it doesn't exist.

### 8. Push

Push the bump commit and tag:

```
git push origin <release_branch>
git push origin <new_tag>
```

On push failure (e.g., branch protection rejects direct push), abort. Do not bypass. Print the remote's error message verbatim.

### 9a. CI-driven: watch the release workflow (default)

Unless `--no-watch` is passed:

1. Find the workflow run triggered by the tag push: `gh run list --event=push --branch=<new_tag> --limit=1` (or poll `gh run list --workflow=release.yml --limit=1` if the workflow file was found in step 4). Wait up to 60s for the run to appear.
2. Stream it: `gh run watch <run_id> --exit-status`.
3. On failure: print the failed job's log tail with `gh run view <run_id> --log-failed | tail -80`. Stop.
4. On success, go to step 10.

With `--no-watch`: print the workflow run URL and the `gh run watch <id>` command the user can run, then stop.

### 9b. --local: publish to crates.io directly

Topologically sort the workspace members to publish:

- Use `config.local.publish_order` if set.
- Otherwise, run `cargo metadata --format-version 1 --no-deps` and do a topological sort of workspace members by their dependency graph (`package.dependencies` filtered to workspace-local crates). Skip members with `publish = false` (read from `cargo metadata` or `Cargo.toml` directly).

For each crate in order:

1. `cargo publish -p <crate>` (no `--dry-run`; dry-run was step 5).
2. Sleep `config.local.inter_publish_sleep_secs` seconds (default 20). Skip sleep after the last crate.
3. On failure, abort. Crates already published cannot be undone; print a clear message listing which crates made it and which didn't.

### 10. Verify crates.io indexed

For each published crate (CI-driven and --local both):

Poll `https://crates.io/api/v1/crates/<crate>/versions` (with reqwest/curl) up to 10 times, 10s apart (100s max per crate). Stop polling when the new version appears in the returned list.

If polling times out for any crate, print a warning (not an error) — indexing occasionally takes longer than 100s. The release is done; the index just hasn't caught up yet.

### 11. Report

Print a final summary:

```
Released <new_tag>

Commit:   <short_sha>
Tag:      <new_tag>
Pushed:   <branch> + tag to origin
CI:       <success/skipped/--no-watch>
crates.io: <list of indexed crates, or "check manually">

GitHub release: <https URL if gh release view succeeds, else "pending">
```

Then stop.

## Error message template

Any abort should print in this shape:

```
[publish-rust] aborted: <one-line summary>

Reason: <specific details>
State:  <what changed so far, e.g. "no changes made" / "commit created but tag not pushed">
Next:   <what the user should do, e.g. "fix X and re-run" / "manually revert commit with `git reset --hard HEAD~1`">
```

Never leave the repo in a weird state silently.

## Invariants

- **No auto-fix.** Preconditions and verify gates are warn/feedback + stop. Never commit fmt fixes, never auto-generate changelog entries, never stash uncommitted work.
- **Atomic push.** The bump commit and the tag are created and pushed together; if the push fails, tell the user exactly what state the local repo is in (commit created, tag created, but nothing pushed) so they can decide whether to `git reset --hard HEAD~1` + `git tag -d <tag>` or retry the push.
- **Respect branch protection.** Never use `--force` on any push. Never bypass git hooks. If the push is rejected, the skill's job is to tell the user why — not to work around it.
- **Opinionated by default, override via config.** The default gates cover most Rust projects. For project-specific quirks (e.g. this repo's `--exclude swink-agent-local-llm` on `--all-features`), use `.claude/publish-rust.toml`.
