---
name: period-review
description: "Summarize orders, traffic, email, and SMS for one period, with the previous period beside it. Use when the user asks how a business did over a date range."
---

Read `references/comparisons.md` and `references/metrics.md`.

Call `list_businesses` if you do not yet have a `business_id`. The timezone comes from that business, not from a tool argument.

For the requested range, and again for the previous period of equal length, call `order_statistics`, `session_statistics`, `email_channel_statistics`, and `sms_channel_statistics`.

Report the primary currency. Compare counts as a relative percent and rates in percentage points. Do not invent a comparison parameter. The second call is the comparison.

Money from `email_*` and `sms_*` is in the business primary currency. Do not answer this review with `get_campaign_statistics`.

The files named above sit next to this plugin, under `references/`. On Claude.ai the same text is already served live as MCP resources. This copy is for a packaged skill, which does not read those resources at runtime.
