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

The Claude panel's notes box is **local to the viewer's browser**
(`localStorage` only) — this is a static GitHub Pages site with no backend,
so the page itself has no way to commit anything to this repo. When the
person reviewing a build wants the next version built from their notes,
they copy them across (the panel's "Copy notes" button) and paste them into
a message here. Fold anything durable from that into this file or into the
next commit's message — there's no `notes.md` auto-synced from the page to
read instead.

## A `--public` build

If this ever needs to ship without the draft layer: strip the Settings and
Claude panels, the mobile-preview toggle and its three header icons, the
draft-only toggles (placeholder highlighting, hi-res reminder dots), and
convert the `@container (max-width:600px)` block back to a plain
`@media (max-width:600px)` query (drop `#page-shell`'s
`container-type:inline-size` too, since nothing will need it any more).
