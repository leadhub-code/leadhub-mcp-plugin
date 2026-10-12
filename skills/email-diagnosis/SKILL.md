---
name: email-diagnosis
description: "Explain email results for one period: revenue, audience, campaign types, campaigns, links, and deliverability. Use when the user asks what email did."
---

Read `references/campaign-categories.md`, `references/metrics.md`, and `references/comparisons.md`.

Use `email_revenue`, `email_audience_activity`, `email_campaign_type_statistics`, `email_campaign_performance`, `email_link_statistics`, and `email_deliverability` for the requested range. Repeat `email_revenue` and `email_audience_activity` for the previous period.

Group types with `campaign_type_groups` from the categories reference. Raw types such as `targeted-emailing` go in `campaign_types`. The tool does not expand a category name on its own.

Name the best and worst campaigns by attributed revenue. When deliverability is weak, call out bounce sides: recipient, host, sender.

Engagement on email is an open. Rates are fractions from 0 to 1. These tools are the period analytics, not `get_campaign_statistics`.

The files named above sit next to this plugin, under `references/`. On Claude.ai the same text is already served live as `analytics://` resources. This copy is for a packaged skill, which does not read those resources at runtime.
