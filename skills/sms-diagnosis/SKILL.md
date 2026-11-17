---
name: sms-diagnosis
description: "Explain SMS results: revenue, channel activity, campaign types, and campaigns. Use when the user asks what SMS did."
---

Read `references/campaign-categories.md`, `references/metrics.md`, and `references/comparisons.md`.

The question is what SMS did in the requested range and what drove it.

A good start: `sms_channel_statistics` for the channel (and again for the previous period), `sms_campaign_type_statistics`, and `sms_campaign_performance`. Use what the question needs.

SMS engagement is a click, not an open, and these tools have no deliverability section. Rates are percent from 0 to 100.

Raw types such as `targeted-sms` go in `campaign_types`. These tools are the period analytics, not `get_campaign_statistics`.

The files named above ship with this plugin, under `references/`. They are copies of the resources the Leadhub MCP server serves, so the skill does not need to read those resources.
