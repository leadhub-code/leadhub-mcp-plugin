---
name: campaign-content-review
description: "Review email campaigns by content and targeting, next to their results. Use when the user asks what an email said and who it went to."
---

Read `references/metrics.md` and `references/segmentation-conditions.md`.

The question is how the content and the targeting of email campaigns relate to their results.

Pick the campaigns with `email_campaign_performance` for the requested range, or use the campaigns the user names (`list_campaigns` or `search_campaigns`). `get_campaign_email_content` and `get_campaign_targeting` show what each one said and who it went to.

Subject, preheader, copy, calls to action, and the audience are the usual suspects; for an A/B campaign, what the winner did differently. Look at whatever explains the results.

Suggest changes the user can make in Leadhub. Change an email only when the user asks for it, with `edit_campaign_email`.

The files named above are in the `references/` folder of this skill. They are copies of the resources the Leadhub MCP server serves, so the skill does not need to read those resources.
