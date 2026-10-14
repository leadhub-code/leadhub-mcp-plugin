---
name: sms-diagnosis
description: "Explain SMS results: revenue, channel activity, campaign types, and campaigns. Use when the user asks what SMS did."
---

Read `references/campaign-categories.md`, `references/metrics.md`, and `references/comparisons.md`.

Use `sms_channel_statistics`, `sms_campaign_type_statistics`, and `sms_campaign_performance` for the requested range. Repeat `sms_channel_statistics` for the previous period.

SMS engagement is a click, not an open, and these tools have no deliverability section. Rates are percent from 0 to 100.

Raw types such as `targeted-sms` go in `campaign_types`. These tools are the period analytics, not `get_campaign_statistics`.

The files named above sit next to this plugin, under `references/`. On Claude.ai the same text is already served live as MCP resources. This copy is for a packaged skill, which does not read those resources at runtime.
