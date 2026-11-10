# Issue tracker: GitHub

Issues, specs, and tickets live in
[Tonlim/rdiff](https://github.com/Tonlim/rdiff/issues). Use the `gh` CLI for tracker
operations. This is development-agent tooling, not GitHub integration in the rdiff
application.

## Target repository

Pass `--repo Tonlim/rdiff` to issue and label commands. API paths must begin with
`repos/Tonlim/rdiff/`. Never publish this project's work to `mattpocock/skills`.

Before the first issue creation in a session, check the clone's default target:

```sh
gh repo view --json nameWithOwner
```

It must report `Tonlim/rdiff`. If it differs, stop and confirm the target with the
user. Keep passing the explicit repository even when the default is correct.

## Issue operations

Create an issue with a literal heredoc for its Markdown body:

```sh
gh issue create --repo Tonlim/rdiff --title "..." --body-file - <<'BODY'
Issue body goes here.
BODY
```

Read an issue, including its discussion:

```sh
gh issue view <number> --repo Tonlim/rdiff --json number,title,body,labels,comments,assignees
```

List issues with the appropriate state and label filters:

```sh
gh issue list --repo Tonlim/rdiff --state open --json number,title,body,labels,comments,assignees
```

Use a sufficient `--limit`, or `gh api --paginate` for exhaustive queries. Do not
assume the default first page contains every issue. GitHub's issues API also
returns PRs, so exclude entries with a `pull_request` field when using that API.

Other operations:

| Operation | Command |
| --- | --- |
| Comment | `gh issue comment <number> --repo Tonlim/rdiff --body "..."` |
| Update the body | `gh issue edit <number> --repo Tonlim/rdiff --body-file -` |
| Add a label | `gh issue edit <number> --repo Tonlim/rdiff --add-label "..."` |
| Remove a label | `gh issue edit <number> --repo Tonlim/rdiff --remove-label "..."` |
| Assign to the driving developer | `gh issue edit <number> --repo Tonlim/rdiff --add-assignee @me` |
| Close | `gh issue close <number> --repo Tonlim/rdiff` |

To resolve an issue, post the explanation as a separate comment before closing it.
On an already-closed issue, `gh issue close --comment` can drop the comment while
still exiting successfully. Preserve existing body content when updating only
part of an issue.

When a skill says "publish to the issue tracker", create a GitHub issue. When it
says "fetch the relevant ticket", read the issue and its comments.

## Parent tickets and blockers

Prefer GitHub's native sub-issues and issue dependencies. These flags require
`gh` 2.94 or later:

```sh
gh issue create --repo Tonlim/rdiff --parent <parent> --title "..." --body "..."
gh issue edit <parent> --repo Tonlim/rdiff --add-sub-issue <child>
gh issue edit <child> --repo Tonlim/rdiff --add-blocked-by <blocker>
```

Creation can also use `--blocked-by <number>,<number>`. A ticket is startable only
when all of its blockers are closed. `gh issue view <number> --repo Tonlim/rdiff
--json blockedBy` includes closed blockers, so count only those whose state is
`OPEN`. The API's open-blocker count is available with:

```sh
gh api repos/Tonlim/rdiff/issues/<number> --jq .issue_dependencies_summary.blocked_by
```

For older clients, use the REST endpoints:

```sh
gh api --method POST repos/Tonlim/rdiff/issues/<parent>/sub_issues -F sub_issue_id=<child-database-id>
gh api --method POST repos/Tonlim/rdiff/issues/<child>/dependencies/blocked_by -F issue_id=<blocker-database-id>
```

Those endpoints require numeric database IDs, not issue numbers or node IDs.
Read an ID with `gh api repos/Tonlim/rdiff/issues/<number> --jq .id`.

If native relationships are unavailable, put `Part of #<parent>` at the top of
the child body and add the child to a task list in the parent. Represent blockers
with `Blocked by: #<number>, #<number>` at the top of the child body.

## Pull requests as a triage surface

**PRs as a request surface: no.** Triage issues, not external pull requests.
GitHub shares a number space between issues and PRs. For an ambiguous reference,
resolve its type with `gh pr view <number> --repo Tonlim/rdiff` or
`gh issue view <number> --repo Tonlim/rdiff` before acting.

## Wayfinding operations

The map is one issue labelled `wayfinder:map`. Its body holds Notes,
Decisions-so-far, and Fog. Each ticket is a child issue with one type label:
`wayfinder:research`, `wayfinder:prototype`, `wayfinder:grilling`, or
`wayfinder:task`. Use only wayfinder labels on these issues, not triage labels.

- Create the map with `gh issue create --repo Tonlim/rdiff --label wayfinder:map`.
- Link child tickets and blockers using the native relationships above, or their
  documented body/task-list fallbacks.
- To find the frontier, read the map's children and retain open, unassigned
  tickets with no open blockers. Follow the map's ordering.
- Claim the chosen ticket by assigning it to the driving developer. This is the
  wayfinding session's first tracker write.
- Resolve it by posting the answer, closing the ticket, and adding a short
  summary and issue link to the map's Decisions-so-far section.
