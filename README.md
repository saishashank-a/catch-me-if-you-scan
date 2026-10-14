# Catch Me If You Scan

Formerly `testing-legend`. If you installed it under that name, reinstall with the commands below.

A Claude Code plugin that tests web apps the way a senior manual QA engineer does: it plans scenarios from a user story, sweeps every UI component for edge cases, runs multi-role flows side by side, watches network traffic, and writes reproducible bug reports with evidence.

## Install

```
/plugin marketplace add saishashank-a/catch-me-if-you-scan
/plugin install catch-me-if-you-scan@catch-me-if-you-scan
```

The plugin bundles two Playwright browsers (desktop and iPhone 15 emulation). If Chromium is missing, run `npx playwright install chromium` once.

## Use

Ask Claude something like:

> Test the checkout flow on https://staging.example.com. Buyer and seller test accounts are in `.env.test`. Give me a bug report and a CSV.

The skill asks for anything missing (environment, test accounts per role, output format), then plans, executes, and reports.

## What's inside

- `skills/catch-me-if-you-scan/SKILL.md` – the workflow
- `references/techniques.md` – cross-cutting techniques (count consistency, save-then-reopen, error side effects, multi-role/multi-tab, navigation, network)
- `references/component-sweeps.md` – edge-case checklists per UI component
- `references/bug-report.md` – bug template with examples

Only point it at apps and accounts you are authorised to test. On production it stays read-only unless you allow specific writes.

## License

MIT. See [LICENSE](LICENSE).
