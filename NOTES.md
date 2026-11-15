# Build process (draft tooling)

This repo carries a draft-only tooling layer in `index.html` — a Settings
panel, a Claude panel, and a "view as mobile" toggle — on top of the public
furniture schedule. See the three small icons fixed to the top-right of the
page (phone / gear / Claude mark).

## Versioning

Before shipping a change that touches `index.html`:

1. Archive the **current** `index.html` into `Build History/index_v<version>_<date>.html`,
   rewriting any relative `img:"..."`, `src="..."` and `href="..."` paths to
   local assets so they still resolve one folder down (prefix with `../`).
   Leave absolute URLs (http/https/data:) alone.
2. Bump `builds.json` with a new `{ "version", "date", "notes" }` entry
   describing what changed. The live page reads this file at load to show
   the current build badge (footer, Settings panel, Claude panel) and the
   "Previous builds" list in the Claude panel.
3. Never silently overwrite a build — always archive first.

## Notes from the live page

The Claude panel's "Save to GitHub" button pushes the notes box straight to
**`NOTES_INBOX.md`** in this repo via the GitHub Contents API, using a
personal access token the person pastes into the panel (kept only in that
browser's `localStorage`, scoped ideally to just this repo). The textarea
also keeps a local draft in `localStorage` between saves, but the thing that
actually reaches this repo is `NOTES_INBOX.md`.

**At the start of any session working on this repo: pull, then read
`NOTES_INBOX.md`** if it exists — that's reviewer feedback waiting to be
acted on. Once it's been read and actioned, clear it out (empty the file or
delete it) so stale notes don't get re-read and re-applied next time.

## A `--public` build

If this ever needs to ship without the draft layer: strip the Settings and
Claude panels, the mobile-preview toggle and its three header icons, the
draft-only toggles (placeholder highlighting, hi-res reminder dots), and
convert the `@container (max-width:600px)` block back to a plain
`@media (max-width:600px)` query (drop `#page-shell`'s
`container-type:inline-size` too, since nothing will need it any more).

## Cross-device sync (state.json)

Bought ticks, removed items, custom items and leading options live in
`state.json` at the repo root, not just in each browser. The page reads it
through the GitHub API (not the Pages URL, which can lag) and writes changes
back after a short pause, merging per change against the last synced state.
Commits named "Update schedule state" come from this. Collapse state and the
settings panel stay per-browser.

Sync needs the shared `github-token` in the browser (entered on the home
page). Without it the page works on this device only and says so in the
toolbar.
