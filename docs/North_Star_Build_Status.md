# North Star — Build Status (as of this session)

A snapshot of where the site build actually stands, so the next work session starts
from truth instead of re-deriving. Pairs with the Build Plan, Site Architecture,
Site Maintenance guide, and the MMS widget troubleshooting note.

---

## The "final spec" every page should match

This session locked a consistent standard. A page is "done" when it has ALL of:

1. **Blue sticky nav** with the hamburger system at `@media (max-width: 820px)` —
   and NOT the old `@media (max-width: 720px) { .nav-links a:not(.nav-cta){display:none} }`
   rule (that old rule conflicts with the hamburger and breaks mobile nav).
2. **`html { scroll-behavior: smooth; scroll-padding-top: 80px; }`** — so in-page
   anchors don't scroll under the sticky nav.
3. **Nav links point to real destinations** (`/who-we-are`, `/instructors`, `/programs`,
   `/events`, `/get-involved`), brand → `/`. Nav label reads **"Events"** (not "What's On").
   Exception: the home page's nav uses in-page anchors (`#happening` etc.) by design.
4. **No `file:///` artifacts** (browser-save damage) anywhere.
5. **Testimonials anonymized** to "North Star Member" — no participant names.
   (Instructor names in teacher sections / testimonials are fine — e.g. Tommy, Anya.)
6. **Founders' names not in body copy.**
7. **GA4 (`G-896FKT56BM`) + Clarity (`wtvei5x13g`)** present.
8. **Course/enroll pages:** MMS widget in a `.enroll-widget` card with
   `overflow: visible` (NOT hidden — hidden clips the widget on mobile), a book-a-call
   line below it (no duration stated), and the widget wrapper full-width on mobile.

---

## Page-by-page status

### Done this session — updated to final spec (in the GitHub repo / outputs)
- `index.html` (`/`) — ensembles tier, "what's happening now" wired to events,
  scroll effects, anonymized reviews, founders removed.
- `get-involved.html` (`/get-involved`) — landing-page build, form + call, anonymized.
- `events.html` (`/events`) — Otherlands feature + 7-workshop archive.
- `otherlands-trio.html` (`/events/otherlands-trio`) — event page / reusable template.
- `foundations-of-jazz.html` — MMS widget `wbb_zp56dJz` ($150), book-a-call, mobile fixes.
- `jazz-guitar-foundations.html` — MMS widget `wbb_zpRJ2Jj` ($150), full pass.
- `improv-lab.html` — built new from template; MMS widget `wbb_zpR5xJM` ($150);
  two teachers (Joe Hartnett, Chris Bates); July–Sept dates (Jul 8/22, Aug 12/26, Sep 9/23).
- `jazz-singers-playground.html` — MMS widget `wbb_zpR9LJ5` ($150); August cohort;
  teacher Tommy Boynton; profile photo recentered.
- `jazz-voice-lab.html` — WAITLIST (no payment); MMS widget `wbb_zpRx3J8`; teacher Anya Menk.
- `private-lessons.html` — Starter Sessions ($160) MMS widget `wbb_zpRYLJ1`; OR book an
  intro call (Calendly `/30min`) with the Google intake form as call prep.

### Also brought to final spec this session (the older pages, now aligned)
These were built in prior sessions; they got the full final-spec pass this session —
nav label "Events", real nav destinations (no home/dead anchors), scroll-padding,
stale nav rule removed, file:// artifacts removed, testimonials anonymized, GA4+Clarity
confirmed:
- `who-we-are.html` (keeps its "The people behind it" founder-bio section — correct here)
- `instructors.html` and `instructors-styled.html`
- `programs.html`
- `ensembles.html`
- `open-spaces-ensemble.html`
- `scholarship-fund.html`
- `enrollment-policies.html`
- `jam-sessions.html`

**The whole site is now on one consistent spec.** The Cowork audit prompt is kept on
file for future use (new pages, periodic re-checks) but is no longer needed for these.

Note: `open-spaces-ensemble.html` and `jam-sessions.html` still have placeholder MMS
form links (`#rsvp-mms-form` on jam sessions; check open-spaces enroll module) — wire
those when the MMS blocks exist, same as the course pages.

### Decided NOT to build
- Instructor sub-pages (`/instructors/[name]`) — dropped.
- `/thank-you-form` — handled in MMS instead.
- `/member-portal` — links out to MMS (`https://www.northstarjazzworkshops.com/member-portal`).

---

## MMS widget block IDs (all share SchoolID `sch_fFfJc`, WebsiteID `wbs_5yYJ0`)
| Page | Block ID | Charge |
|---|---|---|
| Foundations of Jazz | `wbb_zp56dJz` | $150 deposit |
| Jazz Guitar Foundations | `wbb_zpRJ2Jj` | $150 deposit |
| Improv Lab | `wbb_zpR5xJM` | $150 deposit |
| Jazz Singers Playground | `wbb_zpR9LJ5` | $150 deposit |
| Jazz Voice Lab (WAITLIST) | `wbb_zpRx3J8` | **NONE — must never charge** |
| Private Lessons / Starter Sessions | `wbb_zpRYLJ1` | $160 |

---

## Open items (not blocking content, but needed before/at launch)

### MMS-side (only Ben can do — inside MMS)
- [ ] **Publish all six widget blocks** so they render (embedded but blank until published).
- [ ] Paste in the confirmation copy (success message + email) drafted this session for
      each widget. Strip call durations to match the site.
- [ ] Confirm charge amounts: $150 courses, $160 Starter Sessions, $0 Voice Lab.
- [ ] Reconcile cohort dates between each page and its widget (the July/April/fall trap).

### Deployment (Phase 2 — the real launch blocker)
- [ ] GitHub repo + Netlify (auto-deploy), staging branch, GA4 + Clarity accounts live.
- [ ] Styled 404 page.
- [ ] `_redirects` file: old MMS URLs → new URLs.
- [ ] Then DNS cutover (Phase 6) after staging is tested.

### Confirmations still open
- [ ] Calendly slugs real? Courses use `/15min`, Private Lessons uses `/30min`.
- [ ] Events archive: 4 undated past workshops need month/year; confirm 3 inferred years.
- [ ] Run the Cowork audit to bring the untouched pages to final spec.

---

## One-line status
Content for the 10 pages built this session is done and consistent. The remaining work
is (1) deployment infrastructure, (2) MMS-side widget publishing + config, and (3) a
Cowork pass to bring the older untouched pages up to the same final spec.
