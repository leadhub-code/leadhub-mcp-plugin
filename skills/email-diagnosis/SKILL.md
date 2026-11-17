---
name: email-diagnosis
description: "Explain email results: revenue, channel activity, campaign types, campaigns, and links. Use when the user asks what email did."
---

Read `references/campaign-categories.md`, `references/metrics.md`, and `references/comparisons.md`.

The question is what email did in the requested range and what drove it.

A good start: `email_channel_statistics` for the channel (and again for the previous period), `email_campaign_type_statistics`, `email_campaign_performance`, and `email_link_statistics`. Use what the question needs.

Group types with `campaign_type_groups` from the campaign categories. Raw types such as `targeted-emailing` go in `campaign_types`. A category name is not expanded on its own.

Things often worth a look: which campaigns or types carry the revenue, where engagement moved, and which links people click. Follow what the data shows.

Engagement on email is an open. Rates are percent from 0 to 100. These tools are the period analytics, not `get_campaign_statistics`.

The files named above ship with this plugin, under `references/`. They are copies of the resources the Leadhub MCP server serves, so the skill does not need to read those resources.
