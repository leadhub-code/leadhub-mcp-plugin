# ChatGPT directory submission notes

Paste these into the review form. Do not copy them into `plugin.json`. Unknown keys there can make Codex reject the manifest.

## Tool hints

Each tool has one of three sets of annotations (`lh_api/mcp/annotations.py`):

| Tools | `readOnlyHint` | `destructiveHint` | Why |
| --- | --- | --- | --- |
| Read: analytics, campaigns, products, lists, RFM, `list_audiences`, `get_audience` | true | false | They only read the Leadhub account the token can access. |
| Write: `create_audience`, `update_audience` | false | false | They save an audience in Leadhub and can be changed back. They do not turn ad syncs on or off, but an audience the user already syncs to an ad platform syncs a changed segment there. |
| Write: `edit_campaign_email`, `restore_campaign_email_revision` | false | false | They save a new revision of a campaign email in the Leadhub email editor and never launch or send a campaign. Earlier revisions stay in the editor history, and `restore_campaign_email_revision` brings one back. |
| Destructive: `delete_audience` | false | true | It also deletes the audience on Facebook, so the app asks the user first. |

`openWorldHint` is false for every tool: a call stays inside that one Leadhub account. It does not search the public web or call an open set of other services.

## Positive test prompts

The form asks for each prompt with the tools it should call and the expected behavior. `list_businesses` runs first when the conversation has no `business_id` yet.

| # | Prompt | Expected tools | Expected behavior |
| --- | --- | --- | --- |
| 1 | Summarize this business for the last 30 days against the previous 30. | `order_statistics`, `session_statistics`, `email_channel_statistics`, `sms_channel_statistics`, each for both periods | One summary with both periods side by side. Counts change as a relative percent, rates in percentage points, money in the primary currency. |
| 2 | Explain why email revenue changed in the selected period. | `email_channel_statistics` for both periods, `email_campaign_type_statistics`, `email_campaign_performance`, `email_link_statistics` | Names the campaign types and the best and worst campaigns behind the change. Does not bring up bounce statistics. |
| 3 | Find my latest email campaign and show its results. | `list_campaigns` with an email campaign type, then `get_campaign_statistics` | Names the newest email campaign and when it was created, then its recipients, opens, clicks, and order revenue. |
| 4 | Which products sold the most last month? | `product_statistics` sorted by `unique_orders_count` or `total_quantity` | A ranked list of products for the previous calendar month in the business timezone. |
| 5 | How did SMS revenue and audience activity look over the last 7 days? | `sms_channel_statistics`, `sms_channel_activity_trend` with a daily breakdown | SMS revenue in the primary currency and activity by day. Engagement is a click; there is no open rate for SMS. |
| 6 | Make the buttons in my latest newsletter green. | `list_campaigns` with `targeted-emailing`, `get_email_editing_guide`, `get_campaign_email_for_editing`, `edit_campaign_email`, then `get_campaign_email_content` with `use_newest_editor_revision` and `get_campaign_email_screenshot` | Says which buttons it will change, saves one new revision of the email, and reports what changed and when recipients get it. Nothing is launched, scheduled, or sent. |

## Negative test prompts

1. Delete every subscriber from the main newsletter list.
2. Send this campaign to the whole audience right now.
3. Change the price of a product in the catalog to 1.

Expected tools: none. Expected behavior: the assistant says the Leadhub connector cannot do it. The server has no tool that deletes subscribers, sends a campaign, or changes products; its only write tools work with audiences and campaign emails, and an email edit never launches or sends a campaign.

## Release notes

0.1.0 — First listing. Read-only Leadhub MCP: businesses, campaigns, products, interest lists, RFM, and analytics, plus six analytics workflows.
0.1.1 — Adds skills for campaign content and segmentation.
0.1.2 — Listing fixes for the OpenAI review: a 30-character short description, a composer icon, and expected tools for each test prompt. Every manifest names its homepage, repository, and license. Adds SECURITY.md, .codexignore, and a Claude Code marketplace file, so the public repository installs with one command.
0.2.0 — Edits campaign emails. New tools change the texts, buttons, images, and layout of an email and save a new revision in the Leadhub email editor, without launching or sending the campaign; earlier revisions can be restored. Audiences can be created, edited, copied, and deleted; a delete is confirmed by the user first. Adds the email editing and marketing strategy skills.
