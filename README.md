# NPL Task List

A single-page to-do list for NPL work, published as a Claude Artifact.

- `index.html` — the whole thing: markup, styles, and logic in one self-contained file.

## How it works

The page renders from a JSON block embedded in itself (`<script id="state">`). Editing a
task, ticking a checkbox, or adding a row updates that state and republishes the page as a
new version, so changes persist for everyone with the link — not just in one browser.
If publishing isn't available to a viewer, changes fall back to that browser's local storage.

Live page: https://claude.ai/code/artifact/2ceaa546-4a1c-46dd-9dc2-c1b75c1ee75d

## Editing

Edit `index.html` and republish it to the same artifact URL. The seeded tasks live
in the `state` JSON near the bottom of the file; everything else is the shell.

## Owners and tracks

A task can carry an **owner** and a list of **tracks** — the parallel pieces of one
piece of work, each with its own owner and checkbox. Tracks exist so work that several
people do at the same time stays one item on the list instead of splintering into four
unrelated cards.

Every chip is editable in place: click an owner to rename it, `+ Track` to add a track,
the `×` to drop one (clearing a track's text removes it too). Owner chip colors are
derived from the name itself, so the same person is the same color on every card and
nobody has to configure a palette.

In the state JSON these are optional per task:

```json
"owner": "Me",
"steps": [{ "id": "c-site", "text": "…", "owner": "Me", "done": false }]
```

Tasks without them render exactly as before, with an empty "Owner" chip to fill in.

## Dates

A task can carry an optional `date` (plain `YYYY-MM-DD`). It renders as a date picker
chip with a countdown beside it — "in 8 weeks", "in 6 days", "tomorrow" — which turns
red inside two weeks so a dated commitment starts shouting before it arrives. Dates are
read as local midnight, so the day never shifts under a viewer in another timezone.
Tasks without a date show a `+ Date` button instead; clearing the field removes the date.

## Code section

Below the task list, a **Code** section lists every branch on
`claudekovalenko/npl-to-code-list`, read live from GitHub's public REST API each time
the page loads (there's also a Refresh button). The default branch is pinned to the top
and flagged; the rest are ordered freshest-first. Each row links to that branch's code
view on GitHub and to its tip commit, and shows the commit subject, author, and age.

The lookups are anonymous, so they only work for a public repo and share GitHub's
60-requests-per-hour-per-IP anonymous limit. Commit details cost one request per branch,
so they're fetched for the first 15 branches only; the rest still list with name and SHA.
If the API can't be reached — rate limit, offline, or a sandbox that blocks outside
requests (the Claude artifact copy does) — the section falls back to a link to the
branches page on GitHub.

## Live site

Every push to the default branch deploys this repo to GitHub Pages via
`.github/workflows/pages.yml`:

https://claudekovalenko.github.io/npl-to-code-list/

The Pages copy is a plain static file, so it has no access to the Claude runtime —
edits there save to the visitor's own browser only. Shared, persistent edits happen
on the Claude artifact link above.
