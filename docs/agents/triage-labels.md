# Triage labels

These roles map directly to labels in `Tonlim/rdiff`.

| Skill role | GitHub label | Meaning |
| --- | --- | --- |
| `needs-triage` | `needs-triage` | Maintainer needs to evaluate this issue |
| `needs-info` | `needs-info` | Waiting on the reporter for more information |
| `ready-for-agent` | `ready-for-agent` | Fully specified, ready for an AFK agent |
| `ready-for-human` | `ready-for-human` | Requires human implementation |
| `wontfix` | `wontfix` | Will not be actioned |

Use exactly one triage state and one category label, `bug` or `enhancement`, on
each triaged issue. Change states by removing the old state label and applying
the new one. Preserve unrelated labels.

## Wayfinder labels

Wayfinder maps and tickets use only wayfinder labels. They are separate from the
triage queue.

| Label | Purpose |
| --- | --- |
| `wayfinder:map` | Decision map for a wayfinding effort |
| `wayfinder:research` | Research ticket |
| `wayfinder:prototype` | Prototype ticket |
| `wayfinder:grilling` | Human decision ticket |
| `wayfinder:task` | Implementation task |

Use `gh` with `--repo Tonlim/rdiff` for label operations. Existing repository
labels keep their current names, descriptions, and colors.
