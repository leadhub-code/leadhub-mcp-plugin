---
name: rfm-review
description: "Review current RFM segments, how their mix changed, and who moved. Use when the user asks about RFM or churn between two dates."
---

Read `references/rfm.md`.

The question is what the customer base looks like by RFM segment, and how it is shifting.

`list_rfm_profiles` names the profiles; use the default one unless the user names another. `get_rfm_analysis` gives the current window, `rfm_segment_composition` the mix over time (the web uses the last 12 month-ends), and `rfm_movement` who moved between two reference dates (without dates from the user, the last 30 days is a sensible default).

(not in earlier window) and (not in later window) are customers entering and leaving the window. `bucket_movement` shows shifts along recency, frequency, and monetary.

Focus on the segments and moves that matter for this business.

The files named above are in the `references/` folder of this skill. They are copies of the resources the Leadhub MCP server serves, so the skill does not need to read those resources.
