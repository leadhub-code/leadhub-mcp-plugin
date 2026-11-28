---
name: traffic-review
description: "Review traffic volume, sources, journeys, and the channel transition matrix. Use when the user asks where visits come from and which sources convert."
---

Read `references/comparisons.md` and `references/metrics.md`.

The question is where visits come from, which of them convert, and how that compares with the period before.

A good start: `session_statistics` for the total and grouped by source and medium, `session_statistics_trend` for the shape of the range, `journey_stats`, and `transition_matrix`. Use what the question needs.

The comparison is a second call with the previous period, not a parameter.

Sources that bring visits, sources that convert, and journeys that end without a purchase are often worth a look. Follow what the data shows.

The files named above are in the `references/` folder of this skill. They are copies of the resources the Leadhub MCP server serves, so the skill does not need to read those resources.
