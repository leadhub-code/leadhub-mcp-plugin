# Leadhub

Leadhub is a marketing automation platform for e-shops. It collects orders, products, and website visits from the shop, builds customer segments and RFM profiles from them, and reaches customers with targeted email and SMS campaigns, automated journeys, and popups. Every campaign is measured by the orders and revenue it brings.

This plugin brings your Leadhub account into Claude, ChatGPT, Cursor, and Gemini CLI. Today it can:

- Analyze how the shop, products, and email and SMS campaigns performed in any period, compared with an earlier one, including RFM segments and repeat purchases.
- Find the customers you want to reach and save them as an audience.
- Change the texts, buttons, and images of a campaign email.

You stay in control: an audience is changed or deleted only after you confirm, and an email edit is saved as a new version in the Leadhub editor. Nothing is ever sent. A changed audience that syncs to ad platforms is updated there too, and a deleted one is deleted there.

Ten workflows are bundled as skills: a period review, an email diagnosis, an SMS diagnosis, repeat-purchase retention, RFM segments, traffic, a campaign content review, a segmentation review, a marketing strategy, and email editing.

You sign in with your Leadhub account. The plugin talks only to the Leadhub MCP server at `https://api.leadhub.co/mcp` and sees only the businesses your account can access.

## Install

Claude Code 2.1.275 or later:

```
/plugin install leadhub --marketplace leadhub-code/leadhub-mcp-plugin
```

In an older version, add the marketplace first, then install the plugin:

```
/plugin marketplace add leadhub-code/leadhub-mcp-plugin
/plugin install leadhub@leadhub
```

Then try `/leadhub:period-review`.

Claude.ai and Cowork: add a custom connector with the URL `https://api.leadhub.co/mcp`. The workflows are available there as prompts from the server.

ChatGPT: install Leadhub from the plugin directory, or connect the MCP URL in developer mode.

Cursor: install Leadhub from the Cursor marketplace, or copy this folder to the local plugins directory and run Developer: Reload Window.

Gemini CLI:

```bash
gemini extensions install leadhub-code/leadhub-mcp-plugin
```

## Support and privacy

- Documentation: [api.leadhub.co/mcp/docs](https://api.leadhub.co/mcp/docs)
- Help: [podpora.leadhub.co](https://podpora.leadhub.co/)
- Privacy policy: [leadhub.co/zasady-zpracovani-osobnich-udaju](https://www.leadhub.co/zasady-zpracovani-osobnich-udaju)
- Terms of service: [leadhub.co/podminky-pouziti](https://www.leadhub.co/podminky-pouziti)
- Security issues: [SECURITY.md](SECURITY.md)

## For maintainers

This folder is the plugin. It lives in the Leadhub API repository as `plugins/leadhub`, and `make publish-plugin` copies it to the root of the public repository [leadhub-code/leadhub-mcp-plugin](https://github.com/leadhub-code/leadhub-mcp-plugin). Release notes for each version are in `SUBMISSION.md`.

### What each app reads

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
| `references/` | packaged skill | copies of the metric, category, comparison, and RFM notes |

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
