---
name: sms-diagnosis
description: "Explain SMS results for one period: revenue, audience, campaign types, and campaigns. Use when the user asks what SMS did."
---

Read `references/campaign-categories.md`, `references/metrics.md`, and `references/comparisons.md`.

Use `sms_revenue`, `sms_audience_activity`, `sms_campaign_type_statistics`, and `sms_campaign_performance` for the requested range. Repeat `sms_revenue` and `sms_audience_activity` for the previous period.

SMS engagement is a click, not an open. These tools have no deliverability section. Rates are fractions from 0 to 1.

Raw types such as `targeted-sms` go in `campaign_types`. These tools are the period analytics, not `get_campaign_statistics`.

The files named above sit next to this plugin, under `references/`. On Claude.ai the same text is already served live as `analytics://` resources. This copy is for a packaged skill, which does not read those resources at runtime.
