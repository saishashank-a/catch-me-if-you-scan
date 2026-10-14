# Component sweeps

Run the sweep for every instance of a component in scope. Skip lines that don't apply;
don't skip the whole sweep.

## Text input
- Empty, whitespace only, leading/trailing spaces
- Min length, max length, ~150 characters, very long single word (layout overflow)
- Special characters `_ - + \ ? / . ' " < > &`, emoji, non-Latin script, a dot in a name ("B.")
- Numbers: leading 0, all-same digits (9999999999), negative, decimal, huge value
- Paste; clear then save (does clearing persist?); edit middle of text (cursor jump?)
- Enter / Space / Tab key side effects (submits form? opens something?)
- Mobile: keyboard dismisses on tap-outside; field not hidden behind keyboard

## Phone / postcode / email
- Length per selected country code (10 digits accepted for a UK code = bug)
- Change country after entering number → revalidated?
- Postcode autofills city/state; invalid postcode blocks save (not just warns)
- Email: long valid email (100+ chars), plus-addressing, uppercase, already-registered

## Dropdown / select / combobox
- More than ~6 options: stays within viewport, scrolls
- Search inside, no-match state, type a new value (allowed?)
- Every option selectable and displayed after selection (including "Not set" / "Other")
- Sensible default; dependent fields reset (Country → State → City → Postcode)
- Multi-select: add many, remove one, remove all

## Date / time picker
- Today, past, far future, cross-month and cross-year selection
- Start > end, same day, timezone edges (00:00, 12:00 AM)
- Display format matches spec (e.g. DD/MM/YYYY); placeholder vs default value; Safari behavior

## Buttons / CTAs / links
- Works on first click; double/rapid click creates duplicates?
- Disabled state visibly disabled and actually inert; re-enabled after fixing errors
- Correct destination; external links open; links in emails/notifications deep-link correctly

## Forms (single and multi-step)
- Submit empty → errors shown inline, inside the modal, near the field
- Errors clear once fixed; Save scrolls/focuses to the first error
- Back/forward between steps keeps data; refresh mid-flow; skip a step via URL
- Optional fields truly optional; mandatory markers match validation
- Consent/T&C checkbox: required, uncheckable, link opens

## Lists / tables / cards
- Empty, one item, many items; empty-state message and no orphan actions
- Count badge = rows; sort order matches label (ranks #1 above #2)
- Long text truncates with tooltip; no `null`/`undefined` cells
- Sticky header doesn't overlap rows when scrolling; table actually scrolls
- Row actions consistent across rows (Delete missing on some = bug unless permission-based)
- Scroll position kept after an action or returning from detail

## Modals / popups / toasts / panels
- Close via X, outside click, Escape; reopen shows fresh state
- Content not empty; correct type (toast vs blocking modal); doesn't reappear once done
- Notification panel: unread badge stays until actually read; clicking an item opens the record

## File upload
- Valid file; no extension; wrong type (random PDF, image instead of document); oversized
- Cancel mid-upload; replace; remove; re-upload same file
- Network drop mid-upload then retry; file name preserved after re-login
- Parsed/extracted data is sane (junk file should not produce a "valid" profile)

## Auth screens
- Back/close available on every auth screen; secondary links readable (contrast)
- Wrong password, locked/blocked/deleted account messages
- Logout from every role; browser back after logout shows no private data

## Media / images
- Aspect ratio preserved (square stays square); orientation (portrait photo not rotated)
- Shows after save, refresh, re-login, and in every place the image appears
