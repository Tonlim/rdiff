# Scope

## rdiff is

rdiff is an opinionated, lightweight diff viewer and editor written in Rust. It
is a personal tool intended to replace the use of VS Code and the GitHub diff
viewer for local diff review and related edits.

rdiff is a native Linux GUI built with `egui` and `eframe`. The application should
start fast, remain responsive, and use little memory.

rdiff is a local Git tool. Repository information and operations come exclusively
from the installed `git` CLI and state already available on the machine.

## Core workflow

The application must support:

- Viewing local diffs.
- Editing local files as part of reviewing changes.
- Staging changes.
- Creating local Git commits.

Viewing alone is not the full scope. Editing, staging, and committing are part of
the intended application.

## Personal-tool conventions

There is no settings system, settings UI, or application configuration file.
Change defaults and behavior in Rust code when needed.

Favor a small, understandable implementation. Add libraries and infrastructure
for concrete requirements rather than anticipated future features.

## rdiff is not

- A GitHub client. It does not use GitHub APIs or the `gh` CLI, even for read-only
  access.
- An integration with any remote hosting or review service. It does not retrieve
  repository or review data from a server.
- A tool for fetching, pulling, or pushing. Its workflow uses local state and
  creates local changes and commits.
- A browser or WASM application. WASM support was considered and dropped in favor
  of simplicity.
- A terminal UI or a cross-platform desktop project. Linux is the only target.
- A client/server application, hosted service, or local web server.
- A replacement implementation of Git. Repository operations go through the
  installed Git CLI rather than an embedded Git library.
- A general-purpose IDE. Replacing VS Code refers to the diff review and editing
  workflow, not to reproducing its full feature set.
- A configurable product with user-managed settings. This is a personal tool
  whose behavior can be changed in code.

## Decisions still open

The agreed stack does not prescribe the diff layout, supported comparison modes,
editing depth, or staging granularity. Those details need separate decisions.

No libraries or implementation strategies have been selected for syntax
highlighting, intraline differences, or performance optimization. They are not
requirements for setting up the initial stack.
