# rdiff

An opinionated, lightweight diff viewer and editor written in Rust.

rdiff is a personal, native Linux GUI for reviewing diffs, editing files, staging
changes, and creating commits. It is intended to replace the use of VS Code and
the GitHub diff viewer for this workflow.

All repository operations use the installed `git` CLI and locally available
repository state. There is no GitHub API or `gh` CLI integration.

## Stack

| Area | Choice |
| --- | --- |
| Language | Stable Rust, edition 2024 |
| Platform | Native Linux only |
| GUI | `egui` + `eframe` |
| Git access | Installed `git` CLI through `std::process::Command` |
| Project structure | One Cargo package, with separate UI and Git modules |
| Build tooling | Cargo and rustup |
| Formatting | `rustfmt` |
| Linting | Clippy, with warnings treated as errors in CI |
| Testing | Cargo's built-in test runner |

Keep the UI and Git code separate within the same application. Do not introduce a
client/server split or separate backend binary.

There is no settings system. Change behavior and defaults in Rust code. Add other
dependencies only when a concrete requirement justifies them.

## Development

Development requires a stable Rust toolchain, the `rustfmt` and Clippy components,
Git, and a native Linux desktop environment to run the GUI. Any system build
dependencies will be documented when the application is scaffolded.

Once the Cargo package exists, use:

```sh
cargo fmt --all
cargo clippy --all-targets -- -D warnings
cargo test
cargo run
```

The project aims for fast startup, responsive interaction, and low memory use.

## Project documents

- [SCOPE.md](SCOPE.md) defines what rdiff is and is not.
- [AGENTS.md](AGENTS.md) defines the coding and verification rules for agents.
