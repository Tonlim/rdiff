# Working on rdiff

Read [README.md](README.md) and [SCOPE.md](SCOPE.md) before making changes. Keep work
within the requested scope. This is a personal native Linux application, not a
cross-platform framework or a general-purpose IDE.

## Stack and structure

- Use stable Rust and edition 2024. Do not require nightly Rust.
- Use `egui` and `eframe` for the native Linux GUI.
- Keep one Cargo package. Do not introduce a workspace, additional binaries,
  or a client/server architecture without a concrete need and user agreement.
- Separate UI code and Git code into modules. Keep command execution and Git
  output parsing out of UI rendering code.
- Use plain Rust types at the boundary between UI and Git modules. Git code must
  not depend on `egui` types.
- Do not add WASM targets, browser tooling, remote integrations, or a settings
  system. Define defaults and behavior in Rust code.

## Rust conventions

- Prefer straightforward code and the standard library. Add abstractions when
  existing code needs them, not for hypothetical future implementations.
- Use structs and enums to represent meaningful states. Avoid stringly typed
  commands, flags, and results when a dedicated type is clearer.
- Borrow data when practical. Do not add clones, shared ownership, or interior
  mutability merely to avoid understanding an ownership problem.
- Keep visibility narrow. Use private items by default and `pub(crate)` when
  another module needs access.
- Use `Path`, `PathBuf`, `OsStr`, and `OsString` for filesystem paths and process
  arguments. Do not require paths to be valid UTF-8 or use lossy conversions for
  filesystem operations.
- Propagate recoverable errors with `Result` and add useful context at operation
  boundaries. Report failures to the user rather than silently ignoring them.
- Do not use `unwrap()` or `expect()` for failures caused by user input, Git,
  files, or the environment. They are acceptable in tests and for documented
  internal invariants.
- Forbid unsafe code in the application crate. This does not forbid dependencies
  from using unsafe code internally.
- Comment on non-obvious constraints and decisions. Do not narrate code that is
  already clear.

## Git integration and safety

- Use the installed `git` executable through `std::process::Command`. Do not
  replace it with `git2`, `gix`, GitHub APIs, or the `gh` CLI.
- Pass arguments individually. Do not build shell command strings or invoke a
  shell to run Git.
- Use Git's documented, machine-readable output where available. Preserve path
  boundaries with NUL-delimited output where supported.
- Keep repository operations limited to locally available state. Do not add
  fetch, pull, push, or remote hosting integrations.
- Editing, staging, and committing are application capabilities. Mutating actions
  must follow an explicit user action, not occur as a side effect of viewing.
- Never use the developer's checkout as a fixture for mutating integration
  tests. Use disposable temporary repositories.
- Preserve changes you did not make. Do not reset, revert, or discard user work.

## Dependencies and tooling

- Use Cargo and rustup. Keep `Cargo.lock` tracked for the application.
- Add dependencies only for a current requirement. Explain the need and avoid
  overlapping libraries for the same job.
- Do not add a configuration library, async runtime, database, or alternate test
  runner without a demonstrated need.
- Keep code compatible with the project's selected stable toolchain. Do not
  change the toolchain or dependency versions incidentally.

## Formatting and linting

- Use standard `rustfmt` formatting. Run the formatter after Rust changes rather
  than only running its check mode.
- Use Clippy and treat warnings as errors in CI.
- Configure lint policy centrally in `Cargo.toml`. Forbid unsafe
  code and deny `clippy::dbg_macro`, `clippy::todo`, and `clippy::unimplemented` in
  production code.
- Start with Clippy's default lint set. Add useful lints individually rather than
  enabling all pedantic, nursery, or restriction lints.
- Fix warnings instead of suppressing them broadly. Any necessary allowance must
  be narrow and have a reason.

## Testing and verification

- Use Cargo's built-in test runner.
- Add or update tests for changed behavior. Prefer focused unit tests for logic
  that does not need a GUI or Git process.
- Test Git integration against disposable local fixtures. Do not require network
  access, credentials, GitHub, or the developer's Git configuration.
- Keep tests deterministic. Avoid sleeps and assumptions about a user's home
  directory, current branch name, or global Git settings.
- Run these commands after Rust changes:

  ```sh
  cargo fmt --all
  cargo clippy --all-targets -- -D warnings
  cargo test
  ```

- For documentation-only changes, review the diff and check Markdown links and
  whitespace instead.
- Ask the user to verify native GUI appearance and interaction when needed. Do
  not introduce browser-based verification.
- Report what you verified and what you could not verify. Do not fix unrelated
  test failures as part of the task.

## Agent skills

### Issue tracker

Before publishing specs or tickets, reading issues, or updating issue relationships,
read [docs/agents/issue-tracker.md](docs/agents/issue-tracker.md). Use GitHub issues in
`Tonlim/rdiff` through `gh`. This is agent tooling, not application integration.

### Triage labels

Before triaging issues or applying workflow labels, read
[docs/agents/triage-labels.md](docs/agents/triage-labels.md).

### Domain docs

Before exploring domain concepts or recording terms and decisions, read
[docs/agents/domain.md](docs/agents/domain.md). This repo uses a single-context
glossary and ADR layout.
