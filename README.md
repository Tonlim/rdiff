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
| Platform | Native Linux, Wayland only |
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

## Developer installation on Arch Linux

Use official Arch packages and the
[ArchWiki's rustup approach](https://wiki.archlinux.org/title/Rust#rustup):

```sh
sudo pacman -Syu base-devel git rustup wayland libxkbcommon
```

| Package | Purpose |
| --- | --- |
| `base-devel` | Native compiler, linker, build utilities, and `pkgconf` |
| `git` | Repository development and the Git CLI used by rdiff |
| `rustup` | Rust toolchain manager |
| `wayland` | Native Wayland client libraries |
| `libxkbcommon` | Keyboard handling for the Wayland GUI |

The Arch `rustup` package does not install a Rust toolchain. Install stable Rust,
rustfmt, Clippy, the language server, and standard-library sources as your normal user, without `sudo`:

```sh
rustup default stable
rustup component add rustfmt clippy rust-analyzer rust-src
```

Do not install Arch's `rust` package alongside `rustup`; the packages conflict.
Update rustup itself through pacman, not `rustup self update`. Update the system
packages and stable toolchain separately:

```sh
sudo pacman -Syu
rustup update stable
```

## Development

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
