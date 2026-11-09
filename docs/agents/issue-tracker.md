# Issue tracker: Forgejo

Issues and specs for this repo live as issues on the self-hosted Forgejo at `https://git.serrebiradio.com/serrebi/BlindTerm-directory`. Use the `tea` CLI (login `serrebiradio`) for all operations. `gh` does not work here: GitHub is not the tracker.

Pass `--repo serrebi/BlindTerm-directory` when the clone's remote does not point at Forgejo. Add `--output simple` (or `json`) to list commands; the default output is a box-drawn table.

## Conventions

- **Create an issue**: `tea issues create --title "..." --description "..."`. For multi-line bodies use `--description-file -` and a heredoc.
- **Read an issue**: `tea api repos/serrebi/BlindTerm-directory/issues/<number>`, and `tea api repos/serrebi/BlindTerm-directory/issues/<number>/comments` for the comments.
- **List issues**: `tea issues list --state open --output json` with `--labels` and `--state` filters.
- **Make an issue a child of a parent**: Forgejo has no sub-issues. Put `Part of #<parent>` at the top of the child body, and add the child to a task list in the parent body.
- **Comment on an issue**: `tea comment <number> "..."`
- **Apply / remove labels**: `tea issues edit <number> --add-labels "..."` / `--remove-labels "..."`
- **Close**: `tea comment <number> "..."`, then `tea issues close <number>`.

## Pull requests as a triage surface

**PRs as a request surface: no.** _(Set to `yes` if this repo treats external PRs as feature requests; `/triage` reads this flag.)_

When set to `yes`, PRs run through the same labels and states as issues:

- **Read a PR**: `tea pulls <number>`, plus `tea api repos/serrebi/BlindTerm-directory/pulls/<number>.diff` for the diff.
- **List external PRs for triage**: `tea pulls list --state open --output json`, then drop authors who are the owner or a collaborator.
- **Comment / label / close**: `tea comment <number> "..."`, `tea issues edit <number> --add-labels`/`--remove-labels` (labels are shared with issues), `tea pulls close <number>`.

Forgejo shares one number space across issues and PRs, so a bare `#42` may be either: resolve with `tea pulls 42` and fall back to the issue read above.

## When a skill says "publish to the issue tracker"

Create a Forgejo issue.

## When a skill says "fetch the relevant ticket"

Read it as in **Read an issue** above.

## Wayfinding operations

Used by `/wayfinder`. The **map** is a single issue with **child** issues as tickets.

- **Map**: a single issue labelled `wayfinder:map`, holding the Notes / Decisions-so-far / Fog body. `tea issues create --labels wayfinder:map ...`.
- **Child ticket**: an issue with `Part of #<map>` at the top of its body and an entry in the map's task list. Labels: `wayfinder:<type>` (`research`/`prototype`/`grilling`/`task`). Once claimed, the ticket is assigned to the driving dev.
- **Blocking**: Forgejo's native issue dependencies. Add an edge with `tea api -X POST repos/serrebi/BlindTerm-directory/issues/<child>/dependencies -d '{"owner":"serrebi","repo":"BlindTerm-directory","index":<blocker>}'`, and list blockers with `tea api repos/serrebi/BlindTerm-directory/issues/<child>/dependencies`. Also keep a `Blocked by: #<n>, #<n>` line at the top of the child body so the gate is readable as text. A ticket is unblocked when every blocker is closed.
- **Frontier query**: list the map's open children (the map's task list, read against `tea issues list --state open --output json`), drop any with an open blocker or an assignee; first in map order wins.
- **Claim**: `tea issues edit <n> --add-assignees serrebi`, the session's first write.
- **Resolve**: `tea comment <n> "<answer>"`, then `tea issues close <n>`, then append a context pointer (gist + link) to the map's Decisions-so-far.
