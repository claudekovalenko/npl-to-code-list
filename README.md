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

## Live site

Every push to the default branch deploys this repo to GitHub Pages via
`.github/workflows/pages.yml`:

https://claudekovalenko.github.io/npl-to-code-list/

The Pages copy is a plain static file, so it has no access to the Claude runtime —
edits there save to the visitor's own browser only. Shared, persistent edits happen
on the Claude artifact link above.
