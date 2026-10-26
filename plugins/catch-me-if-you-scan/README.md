# Catch Me If You Scan

A Claude Code plugin that tests web apps the way a senior manual QA engineer does. It plans a scenario matrix from a user story, sweeps every UI component for edge cases, runs multi-role flows side by side, watches network traffic, and writes reproducible bug reports with evidence. It also builds an HTML report with Copy bug and Copy fix prompt buttons.

## Install

```
claude plugin marketplace add saishashank-a/catch-me-if-you-scan
claude plugin install catch-me-if-you-scan@catch-me-if-you-scan
```

The plugin bundles two pinned Playwright MCP browsers (`@playwright/mcp@0.0.83`): desktop, and iPhone 15 emulation. If Chromium is missing, run `npx playwright install chromium` once.

## Use

Ask Claude something like: "Test the checkout flow on https://staging.example.com. Buyer and seller test accounts are in `.env.test`." The skill asks for anything missing (environment, test accounts per role, output format), then plans, executes and reports.

Only point it at apps and accounts you are authorised to test. On production it stays read-only unless you allow specific writes, and it never files tracker tickets without confirmation.

Full docs: https://github.com/saishashank-a/catch-me-if-you-scan

## Privacy

The plugin has no server and collects nothing. It drives a local Playwright browser against the app you point it at, using the test accounts you supply. Everything it reads stays on your machine: page content, screenshots, network captures and reports land in `scan-evidence/` and the report folder in your project. The pages it opens can show personal data from your test environment, so use test data where you can. The skill tells Claude to keep secrets, tokens and real personal data out of bug reports. Nothing is sent to the plugin author or any third party, and there is no telemetry. Your conversation with Claude is covered by Anthropic's own privacy policy.

## License

MIT. See [LICENSE](LICENSE).
