# North Star — Site Maintenance Guide

How to keep the site updated once it's live. Written for whoever is making changes
(probably Ben, possibly with Claude/Cowork doing the repetitive parts).

The short version: **routine content updates are easy. Site-wide structural changes
are tedious by hand — hand those to the AI.** Nothing here requires being a developer.

---

## How the site is built (the one thing to understand)

Every page is a **single self-contained HTML file**. The CSS, the JavaScript, the nav,
the footer, and the images (as base64) all live *inside each file*. There is no shared
stylesheet, no shared header/footer, no build step.

**Why this matters for maintenance:**
- Editing **one page's content** = open one file, change text, done. Easy.
- Changing something that appears on **every page** (the nav, the footer, the brand
  color) = the same edit repeated in ~15 files, because each file has its own copy.
  Tedious by hand. This is the main maintenance tax.

This was the right call for launching fast. The trade-off is the duplication above.
See "When to consider Astro" at the end for the eventual fix.

---

## Where the site lives

- **Source of truth:** the GitHub repo (`northstar-jazz-workshops` or similar).
- **Live host:** Netlify, auto-deploys on every push to the main branch.
- **Operational backend:** MyMusicStaff at `portal.northstarjazzworkshops.com`
  (student records, scheduling, billing, the enrollment widgets).

**The golden rule:** the GitHub repo is the truth. Always edit the version in the repo,
never a copy floating in Downloads or Drive. (The "which copy is latest?" confusion is
the #1 way to lose work — the repo eliminates it.)

---

## The update workflow (routine changes)

For any text/content change:

1. Open the file in the GitHub repo (you can edit directly in GitHub's web editor —
   no software to install).
2. Make the edit.
3. Commit (GitHub calls this "Commit changes").
4. Netlify auto-deploys in ~30 seconds. Refresh the live site to confirm.

That's the whole loop. No local setup required for small edits.

---

## Common updates, and how hard each is

### Easy (single-file, do anytime)
- **Change a cohort date, time, or tuition** on a course page → edit that page's hero,
  details band, and enroll intro. **But also update the matching MMS widget** (see
  gotcha below).
- **Swap the season banner** (the bar at the very top of each page).
- **Update "what's happening now"** on the home page (the 3 cards).
- **Swap a photo** → replace the base64 image in the file. (Ask the AI to do the
  encoding/duotone — it's fiddly by hand.)

### Easy-to-moderate (one new file from a template)
- **Add a new event** → copy `otherlands-trio.html`, change the content, commit. Add a
  card to `events.html` and the home page's "what's happening now."
- **Add a new course** → copy the closest existing course page
  (`foundations-of-jazz.html` is the cleanest template), swap the content and the MMS
  widget, commit.

### Moderate (touches every page — hand to the AI)
- **Change the nav** (add/remove a link, rename one).
- **Change the footer.**
- **Change the brand color or a font.**
- **A site-wide bug fix** (like the mobile nav fix we did).

These mean editing all ~15 files. Doing it by hand is an hour of tedious, error-prone
find-and-replace. Doing it with Claude/Cowork is a 5-minute "update the footer on every
page" request. **This is the recommended path for site-wide changes** — not a crutch,
just the right tool. (It's exactly what the Cowork audit was for.)

---

## Gotchas (the things that bite people)

1. **The MMS widget and the page are two separate places.** Each course page shows the
   price/date in its copy, AND the MMS widget collects payment with its own
   price/date/confirmation. If you change a cohort date, you must update **both** — the
   page HTML *and* the widget in MMS — or they'll contradict each other. (We hit a
   July/April/fall mismatch exactly this way.)

2. **A blank widget = unpublished MMS block, not broken HTML.** If a course page's
   sign-up form shows blank, the fix is in MMS (publish/activate that widget block),
   not on the page. See `NOTE-mms-widget-not-rendering.md`.

3. **Widget block IDs** (so you know which is which):
   - Foundations of Jazz → `wbb_zp56dJz`
   - Jazz Guitar Foundations → `wbb_zpRJ2Jj`
   - Improv Lab → `wbb_zpR5xJM`
   - Jazz Singers Playground → `wbb_zpR9LJ5`
   - Jazz Voice Lab (WAITLIST — no payment) → `wbb_zpRx3J8`
   - Private Lessons / Starter Sessions → `wbb_zpRYLJ1`
   All share SchoolID `sch_fFfJc` and WebsiteID `wbs_5yYJ0`.

4. **The Voice Lab widget must never charge.** It's a waitlist. If anyone ever edits it,
   Online Payments and Registration Fee must stay OFF.

5. **Calendly + Google Form links** appear in page copy. If those URLs change, search the
   files for `calendly.com` and `forms.gle` and update everywhere.

6. **Self-contained files mean no accidental shared breakage** — editing one page can't
   break another. The flip side is the duplication tax above. It cuts both ways.

---

## Design tokens (so edits stay on-brand)

If you're changing styles, the palette and type are consistent across all pages:
- Background cream `#f1ebdc`, ink `#1a1714`, coral accent `#d04338`, nav blue `#a6c0d6`,
  sage `#b8c98a`, navy `#3a4a6a`.
- Font: Lora (Google Fonts).
- Photos use a duotone treatment (navy or coral). Don't drop in a raw color photo — it
  won't match. Ask the AI to apply the duotone.

Keep formatting restrained — the look is type-driven with lots of cream space.
(Full detail in the Visual Identity doc.)

---

## Analytics

Every page carries Google Analytics 4 (`G-896FKT56BM`) and Microsoft Clarity
(`wtvei5x13g`). If you add a new page from a template, those come along automatically —
just confirm they're present before publishing.

---

## When to consider Astro (the "v2" question)

The current single-file approach is fine **as long as updates are mostly content**
(dates, events, photos, copy). You may never need to change it.

The signal to migrate to **Astro** (a static site generator) is when the *duplication
tax gets annoying* — i.e. you find yourself frequently making the same change across
many files, or the site grows past ~20 pages. Astro converts the repeated nav/footer/CSS
into **shared components**, so a nav change becomes *one* edit instead of fifteen.

It's not free: Astro adds a build step and a bit of complexity. So it's a deliberate
"when the pain justifies it" move, not a launch requirement. Until then, "let the AI do
the multi-file edit" is the cheaper answer.

---

## One-line summary

Content updates: easy, edit-and-commit. Site-wide changes: hand them to Claude/Cowork.
Keep the page and its MMS widget in sync. The repo is the source of truth. Migrate to
Astro only when repetitive multi-file edits become a regular chore.
