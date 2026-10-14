---
name: email-diagnosis
description: "Explain email results: revenue, channel activity, campaign types, campaigns, links, and deliverability. Use when the user asks what email did."
---

Read `references/campaign-categories.md`, `references/metrics.md`, and `references/comparisons.md`.

Use `email_channel_statistics`, `email_campaign_type_statistics`, `email_campaign_performance`, `email_link_statistics`, and `email_deliverability` for the requested range. Repeat `email_channel_statistics` for the previous period.

Group types with `campaign_type_groups` from the campaign categories. Raw types such as `targeted-emailing` go in `campaign_types`. A category name is not expanded on its own.

Name the best and worst campaigns by attributed revenue. Call out bounce sides (recipient, host, sender) only when `email_deliverability` returns bounce data. When the account has no bounce statistics, that tool returns `bounces_available` false.

Engagement on email is an open. Rates are percent from 0 to 100. These tools are the period analytics, not `get_campaign_statistics`.

The files named above sit next to this plugin, under `references/`. On Claude.ai the same text is already served live as MCP resources. This copy is for a packaged skill, which does not read those resources at runtime.
