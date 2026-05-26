# North Star Jazz Workshops — website

Custom site for North Star Jazz Workshops (Minneapolis). Each page is a **single
self-contained HTML file** (inline CSS/JS, base64 images) — no build step, no shared
stylesheet, no framework. Hosted on Netlify, auto-deploying from the `main` branch.

## Structure
- Root `.html` files serve at clean URLs (e.g. `who-we-are.html` → `/who-we-are`).
- `events/index.html` → `/events`; `events/otherlands-trio.html` → `/events/otherlands-trio`.
- `community-programs/jam-sessions.html` → `/community-programs/jam-sessions`.
- `_redirects` maps old MyMusicStaff URLs to the new ones.
- `docs/` — build status, maintenance guide, the Cowork deployment prompt, and the
  MMS-widget troubleshooting note.

## Maintenance
Routine content edits: change a file, commit, Netlify redeploys in ~30s. Site-wide
changes (nav/footer): hand to Claude/Cowork. Course price/date changes must also be
updated in the matching MyMusicStaff widget. Full detail in `docs/North_Star_Site_Maintenance.md`.

## Operational backend
MyMusicStaff at `portal.northstarjazzworkshops.com` (student records, billing, the
enrollment widgets embedded on course pages).

## Notes for deployment
- This is staging until a deliberate DNS cutover. See `docs/Cowork_Deployment_Prompt.md`.
- Staging is `noindex` via `netlify.toml` — remove that header at real launch.
- Course widgets may render blank until their MMS blocks are published — that's an MMS
  setting, not an HTML bug.
