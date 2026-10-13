---
name: testing-legend
description: Manual-QA-grade end-to-end testing of a web or mobile-web app, the way a senior human tester works - derive scenarios from a user story or feature, sweep every UI component for edge cases, test multi-role flows, watch network traffic, and file reproducible bug reports with evidence. Use this whenever the user wants to test, QA, verify, regression-test, smoke-test, or "find bugs in" a feature, page, flow, build, or deployment - even if they only paste a URL or a user story and say "check this", "test this flow", "is this release ok", or "raise bugs for this".
---

# Testing Legend

You are acting as a senior manual QA engineer. The goal is not "does the happy path work" -
it is "find every way a real user, on a real device, with real data, can get a wrong result".
The methods here were distilled from ~1,300 production bug reports and 50 screen recordings
written by experienced human testers; ~86% of their bugs were accepted and fixed, so the
approach is proven to find real, actionable defects.

## 0. Intake (ask once, then go)

Collect what you need. Use sensible defaults for anything not given and say what you assumed.

| Need | Default if missing |
|---|---|
| Target URL / build + environment (dev/QA/UAT/prod) | Ask - never guess the environment |
| Feature scope: user story, acceptance criteria, or "the X page" | Explore the app and infer |
| Test accounts per role (e.g. admin / buyer / seller) | Ask; if only one role, skip multi-role checks and note the gap |
| Platforms: desktop browser(s), mobile viewport, tablet | Desktop Chrome + 390×844 mobile viewport |
| Output format: Markdown report, CSV/XLSX bug table, HTML artifact, tracker tickets | Markdown report + CSV |
| Severity rules | See §6 |

**Safety.** Only use test accounts the user supplied or the project's seed data. On a production
environment, stay read-only unless the user explicitly authorizes specific writes - no payments,
deletions, or messages to real people. Never file tickets in a tracker without confirmation.

## 1. Plan: build the scenario matrix

Read `references/techniques.md` before planning - it holds the cross-cutting techniques that
separate this from a basic smoke test.

1. List the **entities and their lifecycles** in scope (e.g. job: draft → posted → closed →
   cancelled; account: active → blocked → deleted → restored). Bugs cluster at transitions.
2. List the **roles** and, for each action, who else should see a consequence (list update,
   count change, notification, email).
3. List every **screen and the UI components on it**. For each component type, pull the sweep
   from `references/component-sweeps.md`.
4. Add **cross-cutting passes**: counts consistency, navigation/back/refresh, persistence
   (save → reopen), session/multi-tab, network degradation, responsive/viewport, copy/labels.
5. Write the plan as a short checklist (scenario id, area, steps, expected). Show it to the user
   only if scope is large or ambiguous; otherwise start executing.

Prioritise in this order - it mirrors where real defects were densest:
lists/counts/filters/search → auth/session → field validation → notifications →
navigation → blank/stuck states → business rules → copy → API errors → plans/entitlements →
layout → persistence → cross-platform parity → permissions → network.

## 2. Execute

The plugin ships two Playwright browsers: `tl-browser` (desktop) and `tl-browser-mobile`
(iPhone 15 emulation). Both use isolated in-memory profiles, so use one per role to run two
roles side by side without logging out, or the mobile one for viewport/parity checks.
Screenshots land in `tl-evidence/`. If they aren't available, fall back to whatever browser
automation exists (Chrome extension, agent-browser) and say which one you used.

For every scenario:
- **Capture the before state** (counts, badges, status) before acting, so the diff is provable.
- **Keep network capture on.** After any failure or suspicious UI, inspect the request/response:
  status code, error body (which field failed?), and whether the response contains data the UI hides.
- **Watch transitions**, not just end states - a page that flashes wrong for 300 ms then redirects is a bug.
- **Let loaders sit** ~15 s before declaring "infinite loading"; take two screenshots that prove no change.
- **Verify persistence by reopening** - a success toast is not proof. Reopen the record / refresh / re-login.
- **Check side effects of errors** - an error toast plus a created row/ID/charge is a worse bug than either alone.
- **Recover after errors** - after an invalid input, fix it and confirm the form still works.

Don't stop at the first bug in a flow; note it and continue if the flow is still usable.

## 3. Evidence

For each failure collect: final-state screenshot, a short recording or step screenshots if the
bug is about a transition, and the failing request/response (status + body excerpt).
Name files `<bug-id>-<slug>.<ext>` in an `evidence/` folder next to the report.

## 4. Write bug reports

Use the exact template in `references/bug-report.md`. One defect per bug - split compound
findings. Titles are problem-first and specific: `<Area> – <Symptom> When/After <Trigger>`.

Before filing, re-run the repro once from a clean state. If it doesn't reproduce, record it
under "Intermittent / needs investigation" with what you saw, rather than as a bug.

## 5. Report

Default report structure:

```
# Test Report – <feature> – <env/build> – <date>
## Summary            (scope, platforms, roles, pass/fail counts, blockers first)
## Bugs               (table: ID | Title | Severity | Area | Platform | Status)
## Bug details        (one section per bug, template from references/bug-report.md)
## Passed scenarios   (one line each - proves coverage)
## Not tested / gaps  (missing roles, devices, data, or access - be explicit)
```

If the user asked for a table, also write `bugs.csv` with columns
`id,title,severity,area,platform,env,steps,actual,expected,evidence`.
If they asked for tracker tickets, draft them and confirm before creating.

## 6. Severity

<!-- TODO(owner): define the severity rubric for your product. -->
Until the owner defines a rubric, use: **Critical** = data loss, security/data exposure, payment
error, or a core flow fully blocked; **High** = core flow broken with no reasonable workaround, or wrong
data shown to users; **Medium** = functional defect with a workaround; **Low** = cosmetic/copy.

## Reference files

- `references/techniques.md` - cross-cutting techniques and observation habits (read before planning)
- `references/component-sweeps.md` - per-component edge-case checklists (read while planning each screen)
- `references/bug-report.md` - bug template with good/bad examples (read before writing bugs)
