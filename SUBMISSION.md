# ChatGPT directory submission notes

The review test cases and the release note of the current version are in `.codex-plugin/plugin.json` under `extensions.com.openai`, where the OpenAI submission docs put them; the portal imports and validates them with the ZIP. The test account goes into the review form, and the demo video URL goes into `review.demo_recording_url` once it is recorded.

The test account needs a business whose newest newsletter (`targeted-emailing`) is a draft or scheduled, so the email editing case can say when recipients get the change. For a sent campaign the edit changes only the email kept in the editor.

## Tool hints

Each tool has one of three sets of annotations (`lh_api/mcp/annotations.py`):

| Tools | `readOnlyHint` | `destructiveHint` | Why |
| --- | --- | --- | --- |
| Read: analytics, campaigns, products, lists, RFM, `list_audiences`, `get_audience` | true | false | They only read the Leadhub account the token can access. |
| Write: `create_audience` | false | false | It adds an audience in Leadhub and changes no existing one. It does not turn ad syncs on. |
| Write: `edit_campaign_email`, `restore_campaign_email_revision` | false | false | They save a new revision of a campaign email in the Leadhub email editor and never launch or send a campaign. Earlier revisions stay in the editor history, and `restore_campaign_email_revision` brings one back. |
| Destructive: `update_audience`, `delete_audience` | false | true | `update_audience` overwrites the name or the segment; an audience the user syncs to an ad platform passes a new segment there. `delete_audience` also deletes the audience on Facebook. The app asks the user first. |

`openWorldHint` is false for every tool: a call stays inside that one Leadhub account. It does not search the public web or call an open set of other services.

## Release notes

`make publish-plugin` takes the GitHub release text from the line of the manifest version. The release note in `.codex-plugin/plugin.json` matches it.

0.1.0 — First listing. Read-only Leadhub MCP: businesses, campaigns, products, interest lists, RFM, and analytics, plus six analytics workflows.
0.1.1 — Adds skills for campaign content and segmentation.
0.1.2 — Listing fixes for the OpenAI review: a 30-character short description, a composer icon, and expected tools for each test prompt. Every manifest names its homepage, repository, and license. Adds SECURITY.md, .codexignore, and a Claude Code marketplace file, so the public repository installs with one command.
0.2.0 — Edits campaign emails. New tools change the texts, buttons, images, and layout of an email and save a new revision in the Leadhub email editor, without launching or sending the campaign; earlier revisions can be restored. Audiences can be created, edited, copied, and deleted; a delete is confirmed by the user first. Adds the email editing and marketing strategy skills.
1.0.0 — First directory release. Reads Leadhub businesses, campaigns, products, interest lists, RFM, and analytics. Creates, edits, copies, and deletes audiences; every edit or delete is confirmed by the user first. Edits campaign emails: a change saves a new revision in the Leadhub email editor and never sends the campaign. Includes ten workflows for analytics, campaign content, segmentation, marketing strategy, and email editing.
