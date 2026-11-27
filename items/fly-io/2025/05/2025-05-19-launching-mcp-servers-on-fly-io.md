---
title: Launching MCP Servers on Fly.io
link: https://fly.io/blog/mcp-launch/
source: fly-io
published: 2025-05-19T00:00:00Z
updated: 2026-11-05T00:08:46Z
first_seen: 2026-11-27T00:01:05.630507547Z
content: extracted
html: 2025-05-19-launching-mcp-servers-on-fly-io.html
remote_preview:
  url: https://fly.io/blog/mcp-launch/assets/fly-mcp-launch.png
extra:
  thumbnail: https://fly.io/blog/mcp-launch/assets/fly-mcp-launch.png
---

Author

![Sam Ruby](https://fly.io/static/images/sam.webp)

Name

Sam Ruby

bsky:intertwingly.net

[bsky:intertwingly.net](https://bsky.app/profile/intertwingly.net)

![](https://fly.io/blog/mcp-launch/assets/fly-mcp-launch.png)\
 Image by [Annie Ruygt](https://annieruygtillustration.com/)

This is a blog post. Part showing off. Part opinion. Plan accordingly.

The [Model Context Protocol](https://www.anthropic.com/news/model-context-protocol "") is days away from turning six months old. You read that right, six *months* old. MCP Servers have both taken the world by storm, and still trying to figure out what they want to be when they grow up.

There is no doubt that MCP servers are useful. But their appeal goes beyond that. They are also simple and universal. What’s not to like?

Well, for starters, there’s basically two types of MCP servers. One small and nimble that runs as a process on your machine. And one that is a HTTP server that runs presumably elsewhere and is [standardizing](https://modelcontextprotocol.io/specification/2025-03-26/basic/authorization "") on OAuth 2.1. And there is a third type, but it is deprecated.

Next there is the configuration. Asking users to manually edit JSON seems so early 21th century. With Claude, this goes into `~/Library/Application Support/Claude/claude_desktop_config.json`, and is found under a `MCPServer` key. With Zed, this file is in `~/.config/zed/settings.json` and is found under a `context_servers` key. And some tools put these files in a different place depending on whether you are running on MacOS, Linux, or Windows.

Finally, there is security. An MCP server running on your machine literally has access to everything you do. Running remote solves this problem, but did I mention [OAuth 2.1](https://datatracker.ietf.org/doc/html/draft-ietf-oauth-v2-1-12 "")? Not exactly something one sets up for casual use.

None of these issues are fatal - something that is obvious by the fact that MCP servers are quite popular. But can we do better? I think so.

* * *

Demo time.

Let’s try out the [Slack MCP Server](https://github.com/modelcontextprotocol/servers/blob/main/src/slack/README.md ""):

> MCP Server for the Slack API, enabling Claude to interact with Slack workspaces.

That certainly sounds like a good test case. There is a small amount of [setup](https://github.com/modelcontextprotocol/servers/blob/main/src/slack/README.md#setup "") you need to do, and when you are done you end up with a *Bot User OAuth Token* staring with `xoxb-` and a *Team ID* starting with a `T`.

You *would* run it using the following:

```wrapper
npx -y @modelcontextprotocol/server-slack
```

But instead, you convert that command to JSON and find the right configuration file and put this information in there. And either run the slack MCP server locally or set up a server with or without authentication.

Wouldn’t it be nice to have the simplicity of a local process, the security of a remote server, and to eliminate the hassle of manually editing JSON?

Here’s our current thinking:

```wrapper
fly mcp launch \
  "npx -y @modelcontextprotocol/server-slack" \
  --claude --server slack \
  --secret SLACK_BOT_TOKEN=xoxb-your-bot-token \
  --secret SLACK_TEAM_ID=T01234567
```

You can put this all on one line if you like, I just split this up so it fits on small screens and so we can talk about the various parts.

The first three words seem reasonable. The quoted string is just the command that we want to run. So lets talk about the four flags. The first tells us which tool’s configuration file we want to update. The second specifies the name to use for the server in that configuration file. The last two set secrets.

Oh, did I say current thinking? Perhaps I should mention that it is something you can run right now, as long as you have flyctl `v0.3.125` or later. Complete with the options you would expect from Fly.io, like the ability to configure auto-stop, file contents, flycast, secrets, region, volumes, and VM sizes.

And, hey, lookie there:

![testing, testing, 1, 2, 3](https://fly.io/blog/mcp-launch/assets/mcp-slack.png)

Support for Claude, Cursor, Neovim, VS Code, Windsurf, and Zed are built in. You can select multiple clients and configuration files.

By default, bearer token authentication will be set up on both the server and client.

You can find the complete set of options on our [`fly mcp launch`](https://docs.fly.io/flyctl/cmd/fly_mcp_launch "") docs page.

* * *

But this post isn’t just about experimental demoware that is subject to change. It is about the depth of support that we are rapidly bringing online, including:

- Support for all transports, not just the ones we recommend.
- Ability to deploy using the command line or the Machines API, with a number of different options that allow you to chose between elegant simplicity and excruciating precise control.
- Ability to deploy each MCP server to a separate Machine, container, or even inside your application.
- Access via HTTP Authorization, wireguard tunnels and flycast, or reverse proxies.

You can see all this spelled out in our [docs](https://docs.fly.io/model-context-protocol ""). Be forewarned, most pages are marked as *beta*. But the examples provided all work. Well, there may be a bug here or there, but the examples *as shown* are thought to work. Maybe.

Let’s figure out the ideal ergonomics of deploying MCP servers remotely together!

Next post ↑

[Litestream Revamped: SQLite Replication](https://fly.io/blog/litestream-revamped/)

Previous post ↓

[Provisioning Machines using MCPs](https://fly.io/blog/mcp-provisioning/)
