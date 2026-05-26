# Cowork Prompt — Deploy the North Star site (Phase 2: GitHub + Netlify)

Paste everything below the line into Claude Cowork. It assumes Cowork can see a local
folder containing the final HTML files and the project reference docs. Fill in the one
bracketed blank (the folder path) before sending.

---

## ROLE & CONTEXT

You are helping deploy the website for **North Star Jazz Workshops (NSJW)**, a jazz
education program in Minneapolis. The site is a set of **single-file HTML pages** (all
CSS, JS, and images are inline/base64 in each file — there is no build step, no shared
stylesheet, no framework). We are migrating from a MyMusicStaff-hosted site to a custom
site hosted on **Netlify**, with source in **GitHub**.

This task is **Phase 2 of the build plan: stand up the repo + hosting infrastructure and
get the site live on a Netlify STAGING URL.** It is NOT the launch. The real domain
(`northstarjazzworkshops.com`) keeps serving the old MyMusicStaff site untouched. DNS
cutover is a separate, later, deliberate step (Phase 6) and is explicitly OUT OF SCOPE here.

The files to deploy are in: **[PASTE THE FOLDER PATH HERE]**

Reference docs (read them first if present in the folder): `North_Star_Build_Status.md`,
`North_Star_Site_Architecture.md`, `North_Star_Build_Plan.md`, `North_Star_Site_Maintenance.md`.

---

## GOAL (definition of done)

A working Netlify staging URL (e.g. `something-random.netlify.app`) where:
- Every page loads at a clean, extensionless URL (`/who-we-are`, not `/who-we-are.html`).
- The nav works on desktop and mobile, and every nav/footer link resolves (no 404s).
- The course-page enrollment widgets are present (they may render blank until the MMS
  blocks are published — that's expected and not your problem to fix; see notes).
- Analytics tags are present on every page.
- The source lives in a GitHub repo connected to Netlify, so future commits auto-deploy.

---

## DIVISION OF LABOR (important)

**You (Cowork) do:** read and verify the files, organize them into the correct repo
folder structure, generate the supporting files (`_redirects`, `404.html`, `README.md`,
optional `netlify.toml`), prepare all git commands, and walk the human through each
external step with exact instructions.

**The human (Ben) does, themselves:** create the GitHub account (if needed), create the
Netlify account (if needed), create/confirm the Google Analytics and Microsoft Clarity
accounts, and perform any login / OAuth / "authorize" clicks. Do NOT attempt to create
accounts or enter passwords on his behalf — pause and hand him clear instructions at each
such step, then continue once he confirms it's done.

**Out of scope entirely (do NOT do):** DNS changes, pointing the real domain, publishing
MMS widget blocks, anything that makes this the public production site.

---

## STEP 1 — Inventory & verify the files

List every `.html` file in the folder and confirm you have the full set. The expected
pages and their target URLs:

| File | Serves at | Type |
|---|---|---|
| `index.html` | `/` | Home |
| `who-we-are.html` | `/who-we-are` | Story / founders |
| `instructors.html` | `/instructors` | Instructors index |
| `programs.html` | `/programs` | Programs index |
| `ensembles.html` | `/ensembles` | Flagship ensembles |
| `open-spaces-ensemble.html` | `/open-spaces-ensemble` | Ensemble course |
| `get-involved.html` | `/get-involved` | Inquiry form |
| `foundations-of-jazz.html` | `/foundations-of-jazz` | Course + widget |
| `jazz-guitar-foundations.html` | `/jazz-guitar-foundations` | Course + widget |
| `improv-lab.html` | `/improv-lab` | Course + widget |
| `jazz-singers-playground.html` | `/jazz-singers-playground` | Course + widget |
| `jazz-voice-lab.html` | `/jazz-voice-lab` | Waitlist + widget |
| `private-lessons.html` | `/private-lessons` | Lessons + widget |
| `scholarship-fund.html` | `/scholarship-fund` | Policy |
| `enrollment-policies.html` | `/enrollment-policies` | Policy |
| `events.html` | `/events` | Events index |
| `otherlands-trio.html` | `/events/otherlands-trio` | Event page |
| `jam-sessions.html` | `/community-programs/jam-sessions` | Jam sessions |

**Critical verification — make sure these are the FINAL versions, not older copies.**
Some older copies of the course pages exist that are MISSING the enrollment widget. For
each of the six widget-bearing pages — `foundations-of-jazz`, `jazz-guitar-foundations`,
`improv-lab`, `jazz-singers-playground`, `jazz-voice-lab`, `private-lessons` — confirm the
file contains a MyMusicStaff widget script (search each file for the text
`mymusicstaff.com/Widget`). If any of those six does NOT contain that string, STOP and tell
the human — that file is a stale copy and the correct version (with the widget) must be
swapped in before deploying.

**Two things to confirm with the human before proceeding:**
1. There are two instructor files in some sets (`instructors.html` and a styled variant).
   Confirm which one is canonical for `/instructors`. Deploy only one to that URL; if he
   wants to keep the other, give it a different path or leave it out. Don't deploy both to
   the same URL.
2. Confirm the URL slug for `open-spaces-ensemble` (assumed `/open-spaces-ensemble`).

---

## STEP 2 — Build the repo folder structure

Create a clean local folder (this becomes the repo). Netlify serves `.html` files at
extensionless URLs automatically, so most pages sit flat at the root. Two pages live in
subfolders to produce nested URLs:

```
/                                      (repo root)
├── index.html
├── who-we-are.html
├── instructors.html
├── programs.html
├── ensembles.html
├── open-spaces-ensemble.html
├── get-involved.html
├── foundations-of-jazz.html
├── jazz-guitar-foundations.html
├── improv-lab.html
├── jazz-singers-playground.html
├── jazz-voice-lab.html
├── private-lessons.html
├── scholarship-fund.html
├── enrollment-policies.html
├── 404.html                           (you will create this — see Step 3)
├── _redirects                         (you will create this — see Step 3)
├── netlify.toml                        (optional — see Step 3)
├── README.md                          (you will create this — see Step 3)
├── events/
│   ├── index.html                     (this is the current events.html, RENAMED)
│   └── otherlands-trio.html
└── community-programs/
    └── jam-sessions.html
```

So: move `events.html` to `events/index.html`, move `otherlands-trio.html` to
`events/otherlands-trio.html`, and move `jam-sessions.html` to
`community-programs/jam-sessions.html`. Everything else stays flat at the root.

Do not edit the contents of the HTML files in this step — only place them. (Their internal
links already use clean paths like `/who-we-are` and `/events`, which is why the structure
above makes them resolve correctly.)

---

## STEP 3 — Create the supporting files

**`_redirects`** (plain text, no extension) — preserves old MyMusicStaff URLs so existing
links don't break after the eventual cutover. Create it with exactly this content:

```
# Old MyMusicStaff paths → new custom URLs
/welcome              /                                             301
/classes-workshops    /programs                                     301
/member-portal        https://portal.northstarjazzworkshops.com     301
```

(Pages whose path is unchanged — `/who-we-are`, `/instructors`, `/scholarship-fund`,
`/enrollment-policies`, `/community-programs/jam-sessions` — need no redirect. Note for the
human: the old preview Netlify subdomains, e.g. the Get Involved and Otherlands previews,
can't be redirected from this repo — those redirects, if wanted, are set on those old
preview sites. Flag this; don't try to handle it here.)

**`404.html`** — a not-found page styled in the site's design system so a wrong URL
doesn't dump an ugly default. Match the existing pages: cream background `#f1ebdc`, ink
`#1a1714`, coral accent `#d04338`, Lora font, the same blue sticky nav and footer as the
other pages. Keep it simple: a short "Page not found" message and a button back to `/`,
plus the standard nav and footer so a lost visitor can navigate. Copy the nav and footer
markup from an existing page (e.g. `who-we-are.html`) so it matches exactly. Include the
same analytics tags (see Step 4). Netlify uses `404.html` as the custom 404 automatically.

**`README.md`** — a short repo readme so future-you knows what this is. Include: what the
project is, that each page is a self-contained HTML file (no build step), that Netlify
auto-deploys from the main branch, the link to `North_Star_Site_Maintenance.md` for the
update workflow, and the note that the old MMS site / member portal lives at
`portal.northstarjazzworkshops.com`.

**`netlify.toml`** (optional) — Netlify serves clean URLs by default, so this isn't
required. If you create one, keep it minimal (it can pin the publish directory to the repo
root). Don't add a build command — there is no build step.

Also copy the four reference `.md` docs into the repo (or a `/docs` subfolder) so the
documentation travels with the code.

---

## STEP 4 — Confirm analytics before pushing

Every page should carry Google Analytics 4 (ID `G-896FKT56BM`) and Microsoft Clarity
(ID `wtvei5x13g`). Verify each HTML file contains both IDs; flag any that are missing them.

**Ask the human to confirm those two IDs belong to real accounts he owns** (the build plan
lists "create GA4 + Clarity accounts" as a step — the IDs may be live, or may be
placeholders). If they are placeholders, he creates the real accounts (his task), gives you
the real IDs, and you do a find-and-replace of the old IDs across every file before pushing.

---

## STEP 5 — Create the GitHub repo and push (human authenticates)

1. Have the human create a new GitHub repository named `northstar-jazz-workshops`
   (private is fine). He does the account creation / sign-in himself. Pause here until he
   confirms the empty repo exists and gives you its URL.
2. Initialize git in the local folder, commit all files, and push to the new repo's `main`
   branch. Prepare the exact commands and run them (or hand them to the human to run if you
   can't execute shell commands):

   ```
   cd "[the repo folder]"
   git init
   git add .
   git commit -m "Initial site: all pages, redirects, 404, docs"
   git branch -M main
   git remote add origin https://github.com/<his-username>/northstar-jazz-workshops.git
   git push -u origin main
   ```

   If he isn't set up with git auth, walk him through it (GitHub now uses a personal access
   token or the GitHub CLI / Desktop app — pick whichever he's comfortable with; GitHub
   Desktop is easiest for a non-developer). Do not enter his credentials for him.

---

## STEP 6 — Connect Netlify (human authenticates)

1. Have the human create / sign in to Netlify and choose "Add new site → Import an existing
   project → GitHub," then authorize Netlify to access the `northstar-jazz-workshops` repo.
   He does the auth clicks.
2. Build settings: **no build command**; **publish directory = the repo root** (`.`).
   Deploy from the `main` branch.
3. Let it deploy. Netlify assigns a random staging URL like `name-name-123456.netlify.app`.
   Capture that URL.
4. Set up a **staging/preview workflow** per the build plan: enable Netlify's deploy
   previews for pull requests, OR create a `staging` branch that deploys to a branch URL, so
   changes can be previewed before they hit the main live URL. Recommend the simplest option
   that works and explain it to the human in one or two sentences.
5. Confirm the site is set to `noindex` for now (it's staging — we don't want Google
   indexing it before launch). The simplest way: add a `X-Robots-Tag: noindex` header via
   `netlify.toml` or a meta robots tag, OR confirm Netlify's "prevent search engine
   indexing" setting is on for the staging site. Note this for the human; it gets removed at
   real launch.

---

## STEP 7 — Verify the deploy (this is the real test)

On the staging URL, check ALL of the following and report results page by page:

- [ ] Every page in the Step 1 table loads at its clean URL (no `.html`, no 404).
- [ ] `/events` loads (from `events/index.html`) and `/events/otherlands-trio` loads.
- [ ] `/community-programs/jam-sessions` loads.
- [ ] The redirects work: visiting `/welcome` lands on `/`, `/classes-workshops` lands on
      `/programs`, `/member-portal` goes to the portal URL.
- [ ] A made-up URL (e.g. `/asdf`) shows the styled 404, not a default Netlify page.
- [ ] On every page, the top nav links and footer links resolve to real pages (click through
      them; list any that 404).
- [ ] Mobile: at a narrow width the hamburger menu opens and links are tappable.
- [ ] The six course/lessons pages show their enrollment widget area. NOTE: if a widget
      renders blank, that is an MMS issue (the block isn't published yet), NOT a deploy bug —
      just note which ones are blank; do not try to fix them here.
- [ ] View-source on a couple of pages and confirm the GA4 and Clarity IDs are present.

---

## STEP 8 — Report back

Produce a short report with:
- The staging URL.
- A page-by-page pass/fail from Step 7.
- Any broken internal links found (page → bad link).
- Which course widgets rendered vs. came up blank (for the human to publish in MMS).
- Confirmation of what was set up (repo, Netlify connection, staging workflow, noindex).
- A clear list of what remains before going live (DNS cutover, publishing MMS widget
  blocks, removing noindex, final pre-cutover checklist) — but do NOT do any of those.

---

## REFERENCE DATA (quick lookup)

- **Repo name:** `northstar-jazz-workshops`
- **Host:** Netlify, auto-deploy from `main`. Publish dir = root, no build command.
- **Analytics:** GA4 `G-896FKT56BM`, Microsoft Clarity `wtvei5x13g`
- **Member portal / operational backend:** `portal.northstarjazzworkshops.com` (MyMusicStaff)
- **Design tokens (for the 404 page):** bg cream `#f1ebdc`, ink `#1a1714`, coral `#d04338`,
  nav blue `#a6c0d6`, sage `#b8c98a`, navy `#3a4a6a`; font Lora.
- **Hard boundary:** staging only. No DNS, no domain pointing, no MMS changes, no account
  creation on the human's behalf. The old MMS site stays live throughout.
