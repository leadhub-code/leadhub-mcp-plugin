---
name: rfm-review
description: Review current RFM segments, how their mix changed, and who moved. Use when the user asks about RFM or churn between two dates.
---

Read `references/rfm.md`.

Call `list_rfm_profiles` and use the default profile unless the user names one.

Call `get_rfm_analysis` for the current window, `rfm_segment_composition` for the last 12 month-ends, and `rfm_movement` between a start and end reference date the user cares about. Default the end to now and the start to 30 days earlier.

Explain named segments, New, and Churned. Use `bucket_movement` for shifts along recency, frequency, and monetary. Do not edit the profile.

The files named above sit next to this plugin, under `references/`. On Claude.ai the same text is already served live as `analytics://` resources. This copy is for a packaged skill, which does not read those resources at runtime.
