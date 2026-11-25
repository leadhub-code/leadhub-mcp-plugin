# Leadhub MCP plugin

Leadhub is a marketing automation platform for e-shops. It collects orders, products, and website visits from the shop, builds customer segments and RFM profiles from them, and reaches customers with targeted email and SMS campaigns, automated journeys, and popups. Every campaign is measured by the orders and revenue it brings.

This plugin brings your Leadhub account into Claude, ChatGPT, Gemini CLI, and Cursor. Today it can:

- Analyze how the shop, products, and email and SMS campaigns performed in any period, compared with an earlier one, including RFM segments and repeat purchases.
- Find the customers you want to reach and save them as an audience.
- Set up RFM profiles: the score buckets and the named customer segments of the RFM analysis.
- Change the texts, buttons, and images of a campaign email.
- Draft website popups and sign-up forms, choose where and to whom they show, and launch, pause, or archive them.
- Archive an automated email or SMS campaign, which stops it, or restore an archived one as paused.

You stay in control: anything that overwrites, deletes, starts, or stops something (an audience, an RFM profile, a popup, or an automated campaign) waits for your confirmation. An email edit is saved as a new version in the Leadhub editor, and no email or SMS is ever sent. A changed audience that syncs to ad platforms is updated there too, and a deleted one is deleted there.

Eleven workflows are bundled as skills: a period review, an email diagnosis, an SMS diagnosis, repeat-purchase retention, RFM segments, traffic, a campaign content review, a segmentation review, a marketing strategy, email editing, and a website popup.

You sign in with your Leadhub account. The plugin talks only to the Leadhub MCP server at `https://api.leadhub.co/mcp` and sees only the businesses your account can access.

## Claude (Anthropic)

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

Claude apps (web, desktop, mobile) and Cowork: install Leadhub from the Claude directory, or add a custom connector with the URL `https://api.leadhub.co/mcp`. The workflows are available there as prompts from the server.

## ChatGPT (OpenAI)

Install Leadhub from the ChatGPT plugin directory, or connect the URL `https://api.leadhub.co/mcp` in developer mode. Codex uses the same plugin.

## Gemini CLI (Google)

```bash
gemini extensions install leadhub-code/leadhub-mcp-plugin
```

## Cursor

Install Leadhub from the Cursor marketplace, or copy this repository to the local plugins directory and run Developer: Reload Window.

## Support and privacy

- Documentation: [api.leadhub.co/mcp/docs](https://api.leadhub.co/mcp/docs)
- Help: [podpora.leadhub.co](https://podpora.leadhub.co/)
- Privacy policy: [leadhub.co/zasady-zpracovani-osobnich-udaju](https://www.leadhub.co/zasady-zpracovani-osobnich-udaju)
- Terms of service: [leadhub.co/podminky-pouziti](https://www.leadhub.co/podminky-pouziti)
- Security issues: [SECURITY.md](SECURITY.md)

Notes for maintainers, the role of each file, and release notes are in [SUBMISSION.md](SUBMISSION.md).
