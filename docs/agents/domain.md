# Domain docs

This is a single-context repository. Before exploring a domain concept, read the
relevant entries in root `GLOSSARY.md` and decisions under `docs/adr/`.

If those paths do not exist, proceed silently. Do not create empty glossary or
ADR files, flag their absence, or suggest creating them upfront. The
`domain-modeling` skill creates them when terms or decisions are resolved.

## Layout

- `GLOSSARY.md`: shared domain vocabulary.
- `docs/adr/0001-<decision>.md`, followed by sequentially numbered ADRs: recorded
  design decisions.

## Use the glossary's vocabulary

Use its terms in issue titles, proposals, hypotheses, tests, and code. Follow any
distinctions it makes instead of substituting synonyms. If a needed concept is
missing, reconsider whether it belongs in this project or note the gap for
`domain-modeling`.

## Flag ADR conflicts

When a proposal contradicts an ADR, name the ADR and explain the conflict before
proposing a different decision. Do not silently override a recorded decision.
