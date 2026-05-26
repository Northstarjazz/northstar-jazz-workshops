# NOTE: "The MMS widget isn't showing up" — diagnosis & fix

_Recurring issue. Read this before assuming the embed code or the HTML is broken._

## Symptom
A MyMusicStaff Sign-Up widget is embedded on a course page, but the form
doesn't render — blank space, or a 404 where the white form card should be.

## The embed code is almost never the problem
The embed is a single line:

```html
<script src="https://app.mymusicstaff.com/Widget/v4/Widget.ashx?settings=BASE64"></script>
```

The `settings=` blob is base64 that decodes to:
`{"SchoolID":"sch_fFfJc","WebsiteID":"wbs_5yYJ0","WebsiteBlockID":"wbb_XXXX"}`

For every NSJW widget, SchoolID and WebsiteID are the SAME
(`sch_fFfJc` / `wbs_5yYJ0`). The ONLY thing that differs per course is the
`WebsiteBlockID` (e.g. `wbb_zp56dJz` = Foundations, `wbb_zpRJ2Jj` = Guitar).

So if one course's widget renders and another doesn't, and the page markup is
identical, the HTML is NOT the cause. The difference is entirely the block ID.

## The actual cause: the widget BLOCK isn't live in MMS
MMS hands you an embed code as soon as the block exists — even if that block is
a draft / unpublished / disabled. An unpublished block serves empty or 404
content, so the script loads but paints nothing.

This is the same thing Phase 1 documented: two blocks, same school/website,
one rendered fine (`wbb_zp56WSjU`) and one was a 404 (`wbb_zp56dJz` at the time).

## Fix (all in MMS — cannot be fixed in the HTML)
1. Open that specific Sign-Up widget in MMS.
2. Confirm it is **published / active**, not a draft. Look for a publish or
   enable toggle and turn it on.
3. Check it renders in **MMS's own preview**. If it's blank there too, it's
   definitely the block, not the page.
4. Re-test the live page. Compare against a course whose widget DOES render
   (e.g. Foundations) — identical markup + one blank = it's the block.

## How to verify it's the block vs. the page (fast)
- Load the broken page and a working course page side by side.
- If markup is identical and only one is blank → MMS block issue.
- Decode the `settings=` blob (base64 → JSON) and confirm the WebsiteBlockID
  matches the widget you think you're embedding (catches paste mix-ups).

## Embedding checklist (what the HTML side already does right)
- Wrapper `.enroll-widget` uses `overflow: visible` (NOT hidden — hidden clips
  the widget to nothing on mobile; this was a separate, real bug we fixed).
- `.enroll-widget * { max-width: 100%; box-sizing: border-box; }`
- `@media (max-width: 720px) { .enroll-widget { max-width: 100%; } }`
- The `<script>` sits directly inside `.enroll-widget`.
These are correct and not the cause of a blank widget — the cause is the block.

## One-line summary
Blank MMS widget = the block isn't published in MMS. Publish it there.
The embed code and the page HTML are fine.
