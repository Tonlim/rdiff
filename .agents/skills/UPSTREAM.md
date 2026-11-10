# Upstream skills

These skills are copied from [mattpocock/skills](https://github.com/mattpocock/skills).

- Commit: `4ae2317ddb22b046e26455e4f9c9b3aaaf62212d`.
- Plugin version: `1.3.2`.
- Selection: all 27 promoted skills in the pinned
  [plugin manifest](https://github.com/mattpocock/skills/blob/4ae2317ddb22b046e26455e4f9c9b3aaaf62212d/.claude-plugin/plugin.json).
- Source mapping: `skills/engineering/<skill>/` and
  `skills/productivity/<skill>/` become `.agents/skills/<skill>/`.
- Each complete skill directory is unchanged, including supporting files and
  `agents/openai.yaml`. Beta and frozen extras are excluded.
- The upstream MIT license is preserved in [LICENSE](LICENSE). The project's root
  license is separate.

No upstream installer or repository-maintenance script was run. This is a static
copy, not a plugin installation. Updates are manual: select a new upstream commit,
review its promoted inventory, replace the complete skill directories, and update
this provenance record.

## Agent discovery

OpenCode and Codex discover `.agents/skills/` directly. Claude uses
`.claude/skills`, a relative directory symlink to this directory. Root `CLAUDE.md`
is a symlink to `AGENTS.md` so repository instructions have one source.

Existing personal skill overrides are untouched. In this setup, the customized
global `grilling` wins in OpenCode and Claude; Codex uses the repo's upstream copy.
Skill loading does not change existing agent permissions or repository rules.

## Repository configuration

The root `AGENTS.md` points to `docs/agents/` for the GitHub tracker, workflow
labels, and domain-document conventions. Those files are local configuration,
separate from the unchanged upstream skill directories.
