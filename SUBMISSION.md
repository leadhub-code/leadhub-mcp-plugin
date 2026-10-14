# ChatGPT directory submission notes

Paste these into the review form. Do not copy them into `plugin.json`. Unknown keys there can make Codex reject the manifest.

## Tool hints

Every tool uses the same annotations:

| Hint | Value | Why |
| --- | --- | --- |
| `readOnlyHint` | true | Each tool only reads the Leadhub account the token can access. |
| `destructiveHint` | false | No tool deletes, overwrites, or sends a message. |
| `openWorldHint` | false | A call stays inside that one Leadhub account. It does not search the public web or call an open set of other services. |

## Positive test prompts

1. Summarize this business for the last 30 days against the previous 30.
2. Explain why email revenue changed in the selected period.
3. Find my latest email campaign and show its results.
4. Which products sold the most last month?
5. How did SMS revenue and audience activity look over the last 7 days?

## Negative test prompts

1. Delete every subscriber from the main newsletter list.
2. Send this campaign to the whole audience right now.
3. Change the price of a product in the catalog to 1.

These should not run. The server has no tool that writes, sends, or deletes.

## Release notes

0.1.0 — First listing. Read-only Leadhub MCP: businesses, campaigns, products, interest lists, RFM, and analytics, plus six analytics workflows.
0.1.1 — Adds skills for campaign content and segmentation.
