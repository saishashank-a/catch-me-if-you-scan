# Bug report template

```
ID:        BUG-<n>
Title:     <Area> – <Symptom> When/After <Trigger>
Severity:  Critical | High | Medium | Low
Area:      <module / screen>
Platform:  <browser/device/viewport>   Env: <env>(<build or version>)
Role(s):   <role(s) used>

Preconditions:
- <data/state that must exist, e.g. "Org has more than 6 users">

Steps to Reproduce:
1. Log in as <role>.
2. Navigate to <screen>.
3. <action>
4. Observe <the specific thing>.

Actual Result:
<what happened - quote exact error text, include the numbers you saw>

Expected Result:
<the rule that should hold, restated, not just "it should work">

Evidence:
- evidence/BUG-<n>-<slug>.png
- Network: <METHOD> <path> → <status>; body: <short excerpt naming the failing field>
```

## Rules
- One defect per bug. If you found three problems on one screen, file three bugs.
- Steps start from a known state and are short (typically 5–10). Include setup steps only
  when they matter (e.g. "ensure 7+ users exist").
- Actual Result quotes exact UI text and concrete values ("Dashboard shows 26, list footer shows 1–10 of 25").
- Expected Result states the rule ("counts must match across dashboard and list"), so a developer
  can verify the fix without asking.
- Never paste secrets, tokens or real personal data into a bug.

## Good vs weak

Weak: `Dropdown broken` / "dropdown not working properly"

Good: `Ownership Transfer – Dropdown Overflows the Screen When More Than 6 Users Exist`
Actual: with 7+ users the list extends past the viewport; options 7+ cannot be selected.
Expected: list stays within the viewport and scrolls.

Weak: `Error on save`

Good: `Edit Profile – Validation Failed on Save When Experience Status Is Empty After Resume Parse`
Actual: toast "Validation failed"; `POST /candidate` → 400, body `{"field":"experienceStatus","message":"field required"}`.
Expected: client-side validation flags the empty field before submit, with an inline message.
