# Cross-cutting techniques

These are the habits that make a tester find bugs automated happy-path tests miss.
Each one is here because it repeatedly produced accepted bugs.

## Consistency triangle (counts)
The same number usually appears in 3+ places: dashboard tile, tab badge `Applied (3)`,
pagination footer `1–10 of 25`, a number inside a notification sentence, an admin portal.
Read all of them and compare. The pagination footer is the most trustworthy; dashboards drift.
Also compare the list rating/status with the detail view of the same record.

## Save → reopen (persistence)
Edit → save → **reopen the record** (and separately: refresh, log out/in, open in a new tab).
Check: the new value shows in *view* mode, not just *edit* mode; a cleared optional field stays
cleared; uploaded files are still attached; derived fields (city from postcode) persisted.
Also clone/template/draft flows: does the copy carry every field?

## Error side-effects
When an error appears, check whether the action half-happened anyway: record created, history
row added, ID generated, data saved despite "invalid" message. "Error shown but saved" and
"success shown but not saved" are both high-value finds.

## State-transition sweep
For each entity lifecycle, at every state ask: what actions are offered (should Delete/Edit
still exist?), what can other roles see, what notifications fire, can a terminal state be edited,
does an expired/cancelled item still accept actions via direct URL?

## Single-variable mutation (rule engines)
For matching/eligibility/recommendation/pricing rules: create a record that satisfies the rule,
then clone it and change exactly ONE input (experience, location, salary, plan). Verify the outcome
flips everywhere at once - list, badge, ranking order, email, in-app notification.

## Multi-role and multi-session
- Two roles side by side (separate browser contexts): A acts, B verifies immediately, then back.
- Same account in two tabs: act in tab A, check tab B for stale state; log out in A, act in B.
- Actor exclusion: the person who did the action should not be notified about their own action.
- Cascades: block/delete a parent (organization) and check every child (its users, jobs, saved items);
  then unblock/restore and check everything came back - including roles.

## Navigation
Back arrow vs browser back vs refresh, on every screen reached from a tab, filter, or search:
does it return to the same tab, keep filters, keep the result count, keep scroll position?
Direct URL to a deep page while logged out / logged in / as another role. Shared links when the
app is backgrounded vs closed. Logout then press back.

## Filters, search, pagination
Each filter value returns only matching records. Filter options include every value present in
data and stay available after applying. Clear really clears (no refresh needed). Search by full
name, partial, phone, with spaces. Pagination: last page, page N after filter, page size change,
and switching tabs must not carry the page number over.

## Auth and session
OTP: expiry at the stated limit, reuse, wrong-then-correct, resend, switch channel, rapid double
verify, non-digits. Social login edge cases (relay/hidden email). Session length vs spec. Deleted or
blocked account login message. First-login forced password change with the creator-set password.

## Network and device
Throttle to slow 3G and offline mid-upload/mid-submit; restore and retry - the flow must recover.
Mobile viewport, tablet portrait/landscape, OS font scale ×1.3, Safari/WebKit if available.

## API visibility
With network capture on: 4xx/5xx hidden behind a generic toast; the failing field named in the
error body (put it in the bug); responses that contain data the UI masks (privacy bug); frontend
accepting what the backend rejects (missing client validation); duplicate requests on double click.
Optionally hit key endpoints 3× and note large latency variance.

## Observation habits
- Record the before state; watch transitions; let loaders sit ~15 s.
- Look for leaked placeholders: `null`, `undefined`, `NaN`, `{variableName}`, `null null null`, `....`.
- Read every toast fully - wrong message type (toast vs modal), wrong wording, success shown on failure.
- Empty states: correct message, no orphan action buttons (e.g. "Create" shown on an empty expired tab).
