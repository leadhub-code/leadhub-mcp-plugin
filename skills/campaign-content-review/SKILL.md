---
name: campaign-content-review
description: "Review email campaigns by content and targeting, next to their results. Use when the user asks what an email said and who it went to."
---

Read `references/metrics.md` and `references/segmentation-conditions.md`.

Call `email_campaign_performance` for the requested range to pick the campaigns, or use the campaigns the user names (`list_campaigns` or `search_campaigns`).

For each campaign call `get_campaign_email_content` and `get_campaign_targeting`.

Relate results to the subject, preheader, copy, calls to action, and who the email went to. For A/B campaigns compare the variants and say what the winner did differently.

Suggest concrete changes to content and segment conditions. The tools are read-only: the user applies changes in Leadhub.

The files named above sit next to this plugin, under `references/`. On Claude.ai the same text is already served live as MCP resources. This copy is for a packaged skill, which does not read those resources at runtime.
