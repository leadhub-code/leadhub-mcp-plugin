# Directory submission and maintainer notes

This folder is the plugin. It lives in the Leadhub API repository as `plugins/leadhub`, and `make publish-plugin` copies it to the root of the public repository [leadhub-code/leadhub-mcp-plugin](https://github.com/leadhub-code/leadhub-mcp-plugin).

## ChatGPT review

`make plugin-zip-openai` builds the ZIP for the portal from the committed plugin, without the Claude, Cursor, and Gemini manifests. The review test cases and the release note of the current version are in `.codex-plugin/plugin.json` under `extensions.com.openai`, where the OpenAI submission docs put them; the portal imports and validates them with the ZIP. The test account goes into the review form, and the demo video URL goes into `review.demo_recording_url` once it is recorded.

The test account needs a business whose newest newsletter (`targeted-emailing`) is a draft or scheduled, so the email editing case can say when recipients get the change. For a sent campaign the edit changes only the email kept in the editor.

The business must have popups available and an email interest list, so the popup case can draft a sign-up popup with a SUBSCRIBE action. Its website must be a test site: a reviewer who approves a launch shows the popup to that site's visitors within seconds.

## Tool hints

Each tool has one of four sets of annotations (`lh_api/mcp/annotations.py`):

| Tools | `readOnlyHint` | `destructiveHint` | `openWorldHint` | Why |
| --- | --- | --- | --- | --- |
| Read: analytics, campaigns, products, lists, RFM, `list_audiences`, `get_audience`, `get_popup_definitions`, `get_popup_content`, `validate_popup_launch` | true | false | false | They only read the Leadhub account the token can access. |
| Write: `create_audience` | false | false | false | It adds an audience in Leadhub and changes no existing one. It does not turn ad syncs on. |
| Write: `create_rfm_profile` | false | false | false | It adds an RFM profile in Leadhub and changes no existing one. No customer gets anything from it. |
| Write: `edit_campaign_email`, `restore_campaign_email_revision` | false | false | false | They save a new revision of a campaign email in the Leadhub email editor and never launch or send a campaign. Earlier revisions stay in the editor history, and `restore_campaign_email_revision` brings one back. |
| Write: `create_popup_draft`, `create_popup_from_template`, `update_popup_content`, `set_popup_rules`, `restore_campaign` | false | false | false | They add a popup draft, or save a draft or paused popup with a new revision in its history; earlier revisions stay. `restore_campaign` brings an archived popup or automated campaign back paused or as a draft. Nothing is shown on the shop's website until a launch. |
| Destructive: `update_audience`, `delete_audience` | false | true | false | `update_audience` overwrites the name or the segment; an audience the user syncs to an ad platform passes a new segment there. `delete_audience` also deletes the audience on Facebook. The app asks the user first. |
| Destructive: `update_rfm_profile`, `delete_rfm_profile`, `restore_rfm_profile` | false | true | false | `update_rfm_profile` overwrites the bounds, segments, or name of a profile. `delete_rfm_profile` stops the RFM conditions that name the profile from matching anyone, and `restore_rfm_profile` makes them match its segments again. RFM conditions of audiences and campaigns find a segment by its name, so a renamed or removed segment matches nobody there. The app asks the user first. |
| Destructive, open-world: `launch_popup`, `pause_popup`, `cancel_popup_schedule`, `archive_campaign` | false | true | true | They change what visitors of the shop's website see: a launch or schedule starts a popup, a pause, schedule cancel, or archive stops it (`archive_campaign` also stops an automated email or SMS campaign). The app asks the user first. |

`openWorldHint` is true only for the destructive open-world tools: anyone who visits the shop's website sees the popup they start or stop, and `archive_campaign` also stops the emails or texts an automated campaign sends to recipients. `update_audience` and `delete_audience` stay closed-world: they change one audience in the ad account the user connected, and only the user's own ad campaigns, run outside these tools, decide who sees an ad. No tool searches the public web or calls an open set of other services.

## Plugin files

| File | App | Role |
| --- | --- | --- |
| `.claude-plugin/plugin.json` | Claude Code, Cowork | plugin identity |
| `.claude-plugin/marketplace.json` | Claude Code | makes the public repository a marketplace with this one plugin |
| `.mcp.json` | Claude Code, Cowork | remote MCP URL (`type: http`) |
| `.codex-plugin/plugin.json` | ChatGPT, Codex | identity, listing text, three starter prompts, review test cases and the release note for the ChatGPT review |
| `mcp.codex.json` | ChatGPT, Codex | remote MCP URL (`type: http`) |
| `.cursor-plugin/plugin.json` | Cursor | plugin identity, paths to skills and MCP |
| `mcp.cursor.json` | Cursor | remote MCP URL (`url` only; Cursor infers the transport) |
| `gemini-extension.json` | Gemini CLI | extension identity and remote MCP URL (`httpUrl`, Streamable HTTP) |
| `skills/*/SKILL.md` | every app | when to use the workflow and which tools to call |
| `skills/*/agents/openai.yaml` | ChatGPT, Codex | skill name and MCP binding (`transport: streamable_http`) |
| `references/` | packaged skill | copies of the metric, category, comparison, RFM, segmentation, capability, email editing, and popup notes |

Skills and `references/` are one shared copy. Only the manifests and MCP links differ per app.

`streamable_http` in `agents/openai.yaml` uses an underscore. `http` in `mcp.codex.json` and `.mcp.json` is the same Streamable HTTP transport. In `gemini-extension.json`, `httpUrl` is Streamable HTTP. A `url` field in that file would mean SSE.

The only network call is the MCP client talking to the Leadhub server. The plugin does not call any other host, and it does not read tokens from the machine.

The Gemini CLI gallery needs the GitHub topic `gemini-cli-extension` and a git tag equal to `version` in the manifests (`1.0.0`).

Run the plugin from a local folder:

```bash
claude --plugin-dir plugins/leadhub
gemini extensions link plugins/leadhub
```

From a clone of the public repository, use `.` instead of `plugins/leadhub`. Restart Gemini CLI after linking.

## Release notes

`make publish-plugin` takes the GitHub release text from the line of the manifest version. The release note in `.codex-plugin/plugin.json` matches it.

0.1.0 — First listing. Read-only Leadhub MCP: businesses, campaigns, products, interest lists, RFM, and analytics, plus six analytics workflows.
0.1.1 — Adds skills for campaign content and segmentation.
0.1.2 — Listing fixes for the OpenAI review: a 30-character short description, a composer icon, and expected tools for each test prompt. Every manifest names its homepage, repository, and license. Adds SECURITY.md, .codexignore, and a Claude Code marketplace file, so the public repository installs with one command.
0.2.0 — Edits campaign emails. New tools change the texts, buttons, images, and layout of an email and save a new revision in the Leadhub email editor, without launching or sending the campaign; earlier revisions can be restored. Audiences can be created, edited, copied, and deleted; a delete is confirmed by the user first. Adds the email editing and marketing strategy skills.
1.0.0 — First directory release. Reads Leadhub businesses, campaigns, products, interest lists, RFM, and analytics. Creates, edits, copies, and deletes audiences, and creates, edits, deletes, and restores RFM profiles. Edits campaign emails: a change saves a new revision in the Leadhub email editor and never sends the campaign. Drafts, edits, launches, pauses, and archives website popups, and archives or restores automated email and SMS campaigns. Anything that overwrites, deletes, starts, or stops something is confirmed by the user first. Includes eleven workflows for analytics, campaign content, segmentation, marketing strategy, email editing, and website popups.
