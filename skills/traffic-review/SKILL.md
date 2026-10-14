---
name: traffic-review
description: "Review traffic volume, sources, journeys, and the channel transition matrix. Use when the user asks where visits come from and which sources convert."
---

Read `references/comparisons.md` and `references/metrics.md`.

Call `session_statistics` for the total and again with `group_by` source and medium. Call `session_statistics_trend` for the shape of the range. Call `journey_stats` and `transition_matrix`. Repeat `session_statistics` for the previous period.

Say which sources introduce visits, which ones convert, and which ones end without a purchase. The comparison is a second call, not a parameter.

The files named above sit next to this plugin, under `references/`. On Claude.ai the same text is already served live as MCP resources. This copy is for a packaged skill, which does not read those resources at runtime.
