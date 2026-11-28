---
name: segmentation-review
description: "Explain who a campaign or audience targets, and suggest better conditions. Use when the user asks who a campaign or audience targets."
---

Read `references/segmentation-conditions.md`.

The question is who a campaign or an audience reaches, in plain language, and how it could target better.

`list_segment_conditions` lists the conditions this business can use. For a campaign, `get_campaign_targeting` gives the segment; for an audience, `list_audiences` and `get_audience`. Ids in the conditions resolve with `get_campaign`, `interest_lists`, and `list_rfm_profiles`.

Explain the AND/OR logic and the interest lists. For an automated campaign, when a contact enters and whether `send_to_initial_segment` applies.

Suggested changes use only conditions from `list_segment_conditions`, as rule JSON the user can rebuild in the Leadhub segment designer. `validate_segment_query` checks a proposed tree and `count_segment_audience` says how many contacts it reaches. A proposal the user approves can be saved as an audience with `create_audience` or `update_audience`.

The files named above are in the `references/` folder of this skill. They are copies of the resources the Leadhub MCP server serves, so the skill does not need to read those resources.
