# Cowork Audit Prompt — bring all pages to final spec

Paste the section below to Cowork (or a fresh Claude session with access to the
GitHub repo / the folder of HTML files). It audits every page and fixes the pages that
were built in earlier sessions and never updated, so the WHOLE site matches the standard
the latest pages set.

**Already at final spec (don't need fixing, use as the reference for "correct"):**
index, get-involved, events, otherlands-trio, foundations-of-jazz, jazz-guitar-foundations,
improv-lab, jazz-singers-playground, jazz-voice-lab, private-lessons.

**Likely still have old bugs (the real targets):**
who-we-are, programs, instructors, ensembles, scholarship-fund, enrollment-policies,
community-programs/jam-sessions.

---

## PROMPT TO PASTE

You are auditing the North Star Jazz Workshops website — a set of single-file HTML pages
(inline CSS/JS, base64 images). Each page is self-contained, so every fix below must be
applied to EACH page individually. Go through every `.html` file in the repo and bring
each one up to the "final spec" below. Work file by file. For each file, report what you
changed; if a file is already compliant, say so and move on. Do not alter page content,
copy, or images beyond what these fixes require. Verify tag balance after editing each file.

### Fixes to apply to every page

1. **Remove the conflicting mobile-nav rule.** Delete any rule like
   `@media (max-width: 720px) { .nav-links a:not(.nav-cta) { display: none; } }`.
   It conflicts with the hamburger menu and breaks mobile navigation. The hamburger
   system should be at `@media (max-width: 820px)`. If a page is missing the hamburger
   system entirely, flag it in your report (don't fabricate one — note it for a human).

2. **Add scroll-padding** so anchor links don't hide under the sticky nav. Ensure the
   `html` rule reads: `html { scroll-behavior: smooth; scroll-padding-top: 80px; }`.

3. **Fix the enrollment/MMS widget container** (course pages only). The widget wrapper
   (`.enroll-widget` or similar) must use `overflow: visible` — NOT `overflow: hidden`,
   which clips the widget on mobile. Ensure it's full-width on mobile:
   `@media (max-width: 720px) { .enroll-widget { max-width: 100%; } }` plus
   `.enroll-widget * { max-width: 100%; box-sizing: border-box; }`.

4. **Remove `file:///` artifacts.** Delete any `<!-- saved from url=(....)file:///... -->`
   comments or absolute local file paths left by browser "save as".

5. **Check `overflow` on hero/section wrappers** — horizontal scroll/overflow bugs on
   mobile usually trace to a section without `overflow-x: hidden` or an element wider
   than the viewport. Fix any you find.

### Consistency sweep (every page)

6. **Nav label:** the nav link must read **"Events"**, not "What's On".

7. **Nav destinations:** non-home pages link to real destinations —
   `/who-we-are`, `/instructors`, `/programs`, `/events`, `/get-involved`; brand → `/`.
   (The home page intentionally uses in-page anchors like `#happening` — leave those.)

8. **Testimonials anonymized:** any participant/reviewer name (e.g. "Travis F.",
   "Jeffrey S.", real first-name-last-initial) → "North Star Member". KEEP instructor
   names (Tommy Boynton, Anya Menk, etc.) — naming the teacher is intentional.

9. **No founders' names in body copy** (Ben Litwin, Joe Hartnett, Chris Bates as
   *founders*). Note: Joe and Chris appear legitimately as *instructors* on Improv Lab —
   that's fine. The rule is no "founded by [names]" in body copy.

10. **Analytics present:** every page must have GA4 (`G-896FKT56BM`) and Microsoft
    Clarity (`wtvei5x13g`). Add to any page missing them.

11. **Call durations:** the visible call CTA copy should NOT state a duration
    ("Book a call" / "Book an intro call", not "Book a 15-minute call"). Links stay as-is.

### REPORT (after the audit, list these — don't fix, just surface for a human)

- **Calendly slugs** used on each page (courses should be `/15min`, private lessons
  `/30min`) — flag any that look wrong or inconsistent.
- **Cohort dates** stated on each course page — list them so they can be cross-checked
  against the MMS widget config (page and widget must agree).
- **MMS widget block IDs** present on each course page (see table below) — flag any page
  whose embedded block doesn't match the expected one.
- **Pages missing the hamburger nav system entirely** — these need a human to add it.
- **Any broken internal links** (links to pages that don't exist in the repo).
- **Any page still showing a blank widget** — note that this is an MMS publish issue
  (the block isn't published), NOT an HTML bug.

### Reference: expected MMS widget block IDs
(all share SchoolID `sch_fFfJc`, WebsiteID `wbs_5yYJ0`)
- Foundations of Jazz → `wbb_zp56dJz` ($150)
- Jazz Guitar Foundations → `wbb_zpRJ2Jj` ($150)
- Improv Lab → `wbb_zpR5xJM` ($150)
- Jazz Singers Playground → `wbb_zpR9LJ5` ($150)
- Jazz Voice Lab → `wbb_zpRx3J8` (WAITLIST — no charge)
- Private Lessons / Starter Sessions → `wbb_zpRYLJ1` ($160)

### Design tokens (so any CSS edits stay on-brand)
Background cream `#f1ebdc`, ink `#1a1714`, coral `#d04338`, nav blue `#a6c0d6`,
sage `#b8c98a`, navy `#3a4a6a`. Font: Lora. Photos use SVG duotone (navy/coral).

---

## Why this exists
The pages built in the latest session got all of the above. Pages from earlier sessions
did not — they predate the fixes. This audit closes that gap so the whole site is
consistent. The single-file architecture means there's no shared nav/footer/CSS, so each
fix genuinely has to be repeated per file — which is exactly the kind of repetitive
multi-file work Cowork/Claude should do rather than a human.
