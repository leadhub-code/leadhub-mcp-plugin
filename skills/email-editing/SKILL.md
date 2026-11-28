---
name: email-editing
description: "Change the email of a campaign: texts, buttons, images, structure, or a translation. Use when the user asks to change, rewrite, restyle, or translate the email of a campaign."
---

Read `references/email-editing-rules.md`.

Find the campaign with `list_campaigns` or `search_campaigns`. For an A/B campaign, ask which variant to change unless the user said.

Call `get_email_editing_guide`, then `get_campaign_email_for_editing`. Read the element sections of the guide for the element types you will add or change.

Tell the user briefly what you will change. Then call `edit_campaign_email` with all operations of the change and the revision and `revision_id` you read as `base_revision` and `base_revision_id`. When it fails, nothing was saved: fix the operations and send all of them again, or read the email again when it changed in the meantime.

Check the result with `get_campaign_email_content` and `use_newest_editor_revision` true. When the change affects how the email looks (layout, images, colors, buttons), also look at it with `get_campaign_email_screenshot`. Report what changed, what you could not change, and the `revision_effect`, and offer the user a look at the email as `get_campaign_email_screenshot` describes. To undo, use `list_campaign_email_revisions` and `restore_campaign_email_revision`.

The files named above are in the `references/` folder of this skill. They are copies of the resources the Leadhub MCP server serves, so the skill does not need to read those resources.
