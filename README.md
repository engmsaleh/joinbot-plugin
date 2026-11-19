# JoinBot plugin

[![JoinBot MCP server](https://mcprush.com/badge/joinbot-mcp/card.svg)](https://mcprush.com/to/joinbot-mcp?ref=badge)

Puts your agent into Google Meet, Teams and Zoom meetings as a named AI participant ("Hakim (AI)") that hears
everything and answers out loud when someone says its name. JoinBot handles listening and speaking; your agent
supplies the answers with its own knowledge and tools.

One package for Grok Bot, ChatGPT Dots and Claude Duet; the same MCP server also works in OpenBot, Rakazo, Rerun, MausBot and dusk bot. Each reads its own manifest; the skill in `skills/joinbot/` is shared.

| Host | Manifest | MCP config | Sign-in |
| --- | --- | --- | --- |
| Grok Bot (Cursor plugins) | `.cursor-plugin/plugin.json` | `cursor-mcp.json` | workspace connect key header |
| ChatGPT Dots (OpenAI plugins, Agent Plugins 1.0) | `plugin.json` | `mcp.json` | OAuth (JoinBot account, via Clerk) |
| Claude Duet (Claude plugins) | `.claude-plugin/plugin.json` | `.mcp.json` | OAuth (JoinBot account, via Clerk) |

The MCP server is `https://usejoinbot.com/api/mcp`. OAuth clients find the sign-in server from
`/.well-known/oauth-protected-resource/api/mcp`. A signed-in user acts for their first JoinBot workspace.

## Grok Bot

1. In JoinBot, open **Bots → Connect a bot** and create or rotate the workspace connect key. Copy it.
2. In Grok Bot, add this plugin (Settings → Plugins, or your team marketplace) and set `JOINBOT_CONNECT_KEY` to that key
   when asked.
3. Message your bot: **"Connect to JoinBot."** It registers itself and sets up a "JoinBot meeting" routine so JoinBot
   can wake it.
4. Approve the bot in JoinBot (**Bots**), unless auto-approve is on.

Now **Send** in JoinBot puts the bot in a meeting and wakes it; it starts listening and answering on its own.
You can also give the bot a meeting link directly.

If the bot can't read its routine's webhook URL and key, paste them into **Bots → your bot → Wake-up**.

## ChatGPT Dots and Claude Duet

1. Install the JoinBot plugin (or add `https://usejoinbot.com/api/mcp` as a custom connector) and sign in with your
   JoinBot account when asked.
2. Tell your agent: **"Connect to JoinBot."** It registers itself.
3. Approve it in JoinBot (**Bots**), unless auto-approve is on.
4. Give it a meeting link: it joins and answers out loud.

## OpenBot, Rakazo, Rerun, MausBot and dusk bot

These apps add any MCP server by URL. Use your workspace connect key (JoinBot → **Bots → Connect a bot**):

| Setting | Value |
| --- | --- |
| URL | `https://usejoinbot.com/api/mcp` (Streamable HTTP) |
| Header | `Authorization: Bearer <your connect key>` |

- **OpenBot** (openbot.run, and OpenBot by CopilotKit at open.bot): add a custom MCP server with the URL and the header.
- **Rakazo:** install this repository as a plugin, or add the URL as an MCP server with the header.
- **Rerun** (rerun.build): add an MCP server with the URL and the header.
- **MausBot** and **dusk bot:** Plugins → MCP servers → Add server, then paste
  `claude mcp add --transport http joinbot https://usejoinbot.com/api/mcp --header "Authorization: Bearer <your connect key>"`.

Then tell your bot **"Connect to JoinBot."** and approve it in JoinBot. If your app can start the bot from a webhook
(Rakazo, Rerun and MausBot can), the skill sets up wake-up so JoinBot can send it to meetings from the app.
