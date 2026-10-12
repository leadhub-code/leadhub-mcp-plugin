# Leadhub

Read-only access to a Leadhub account: businesses, campaigns, products, interest lists, RFM, and analytics. The plugin does not create, update, or delete anything. Every request goes to the Leadhub MCP server at `https://api.leadhub.co/mcp`.

Six workflows are bundled as skills: a period review, an email diagnosis, an SMS diagnosis, repeat-purchase retention, RFM segments, and traffic. Other areas (campaigns, products, lists) are available as tools on the same server. Money from the email and SMS tools is in the business primary currency. A period comparison is a second call, not a parameter on the first one.

## What each app reads

| File | App | Role |
| --- | --- | --- |
| `.claude-plugin/plugin.json` | Claude Code, Cowork | plugin identity |
| `.mcp.json` | Claude Code, Cowork | remote MCP URL (`type: http`) |
| `.codex-plugin/plugin.json` | ChatGPT, Codex | identity, listing text, three starter prompts |
| `mcp.codex.json` | ChatGPT, Codex | remote MCP URL (`type: http`) |
| `.cursor-plugin/plugin.json` | Cursor | plugin identity, paths to skills and MCP |
| `mcp.cursor.json` | Cursor | remote MCP URL (`url` only; Cursor infers the transport) |
| `gemini-extension.json` | Gemini CLI | extension identity and remote MCP URL (`httpUrl`, Streamable HTTP) |
| `skills/*/SKILL.md` | every app | when to use the workflow and which tools to call |
| `skills/*/agents/openai.yaml` | ChatGPT, Codex | skill name and MCP binding (`transport: streamable_http`) |
| `references/` | packaged skill | copies of the metric, category, comparison, and RFM notes |

Skills and `references/` are one shared copy. Only the manifests and MCP links differ per app.

`streamable_http` in `agents/openai.yaml` uses an underscore. `http` in `mcp.codex.json` and `.mcp.json` is the same Streamable HTTP transport. In `gemini-extension.json`, `httpUrl` is Streamable HTTP. A `url` field in that file would mean SSE.

The only network call is the MCP client talking to that Leadhub server. The plugin does not call any other host, and it does not read tokens from the machine.

## Install

This folder is the plugin. The public repository is [peterkruty-leadhub/leadhub-plugin](https://github.com/peterkruty-leadhub/leadhub-plugin). These files are the repository root, so `gemini-extension.json` sits next to this README. In the Leadhub API repository the same folder is `plugins/leadhub`.

Gemini CLI gallery also needs the GitHub topic `gemini-cli-extension` and a git tag equal to `version` in the manifests (`0.1.0`).

Claude Code, from a clone of the public repository:

```bash
claude --plugin-dir .
```

From the Leadhub API repository:

```bash
claude --plugin-dir plugins/leadhub
```

Then `/leadhub:period-review`. The client connects to the URL in `.mcp.json`.

Cursor: copy this folder to the local plugins directory and run Developer: Reload Window. Cursor reads `.cursor-plugin/plugin.json`. The marketplace takes the public repository URL.

Claude.ai custom connector uses the same MCP URL and takes tools, resources, and prompts from the server. It does not load skills from this folder. Those workflows are also available as prompts on the server. The directory listing is this folder in a public GitHub repository.

ChatGPT: connect the MCP URL in developer mode, or submit this folder for directory review. Refreshing tools does not need a new review. A skill change is a new plugin version.

Gemini CLI, from a clone of the public repository:

```bash
gemini extensions link .
```

From the Leadhub API repository:

```bash
gemini extensions link plugins/leadhub
```

Restart the CLI after linking. Install from GitHub with:

```bash
gemini extensions install peterkruty-leadhub/leadhub-plugin
```
