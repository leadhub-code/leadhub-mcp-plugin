---
name: segmentation-review
description: "Explain who a campaign or saved segment targets, and suggest better conditions. Use when the user asks who a campaign or saved segment targets."
---

Read `references/segmentation-conditions.md`.

For a campaign call `get_campaign_targeting`; for a saved segment call `list_segments` and `get_segment`.

Resolve ids in the conditions to names: `campaign_id` with `get_campaign`, interest lists with `interest_lists`, RFM segments with `list_rfm_profiles`.

Explain the segment in plain language, block by block, including the AND/OR logic and the interest lists. For automated campaigns explain when a contact enters and whether `send_to_initial_segment` applies.

Then suggest concrete condition changes using only conditions from the catalog, as rule JSON the user can rebuild in the Leadhub segment designer. The tools are read-only.

The files named above sit next to this plugin, under `references/`. On Claude.ai the same text is already served live as MCP resources. This copy is for a packaged skill, which does not read those resources at runtime.
