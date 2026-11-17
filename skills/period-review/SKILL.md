---
name: period-review
description: "Summarize orders, traffic, email, and SMS for one period, with the previous period beside it. Use when the user asks how a business did over a date range."
---

Read `references/comparisons.md` and `references/metrics.md`.

The question is how the business did in the requested range compared with the period before it.

Call `list_businesses` if you do not yet have a `business_id`. The timezone comes from that business, not from a tool argument.

`order_statistics`, `session_statistics`, `email_channel_statistics`, and `sms_channel_statistics` cover the main areas; add other tools where the results raise a question.

The comparison is a second call with the previous period of equal length, not a parameter. Compare counts as a relative percent and rates in percentage points. Money is in the business primary currency. Do not answer this review with `get_campaign_statistics`.

Lead with what changed the most and why it matters, rather than walking through every number.

The files named above ship with this plugin, under `references/`. They are copies of the resources the Leadhub MCP server serves, so the skill does not need to read those resources.
