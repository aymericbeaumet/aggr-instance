---
title: Provisioning Machines using MCPs
link: https://fly.io/blog/mcp-provisioning/
source: fly-io
published: 2025-05-07T00:00:00Z
updated: 2026-11-05T00:08:46Z
first_seen: 2026-11-27T00:01:05.630507547Z
content: extracted
html: 2025-05-07-provisioning-machines-using-mcps.html
remote_preview:
  url: https://fly.io/blog/mcp-provisioning/assets/Hello.webp
extra:
  thumbnail: https://fly.io/blog/mcp-provisioning/assets/Hello.webp
---

Author

![Sam Ruby](https://fly.io/static/images/sam.webp)

Name

Sam Ruby

bsky:intertwingly.net

[bsky:intertwingly.net](https://bsky.app/profile/intertwingly.net)

![](https://fly.io/blog/mcp-provisioning/assets/Hello.webp)\
 Image by [Annie Ruygt](https://annieruygtillustration.com/)

Today’s state of the art is K8S, Terraform, web based UIs, and CLIs. Those days are numbered.

On Monday, I created my first fly volume using an [MCP](https://modelcontextprotocol.io/introduction ""). For those who don’t know what MCPs are, they are how you attach tools to [LLM](https://en.wikipedia.org/wiki/Large_language_model "")s like Claude or Cursor. I added support for [fly volume create](https://docs.fly.io/flyctl/cmd/fly_volumes_create "") to [fly mcp server](https://docs.fly.io/flyctl/cmd/fly_mcp_server ""), and it worked the first time. A few hours later, and with the assistance of GitHub Copilot, i added support for all [fly volumes](https://docs.fly.io/flyctl/cmd/fly_volumes "") commands.

* * *

I’m reminded of the memorable scene in the film [Star Trek IV: The Voyage Home](https://en.wikipedia.org/wiki/Star_Trek_IV:_The_Voyage_Home "") (1986). Chief Engineer Scotty, having time-travelled 200 years back to late 20th century San Francisco with his crew mates, encounters an early Personal Computer (PC).

Sitting in front of it, he addresses the machine affably as “Computer!” Nothing happens. Scotty repeats himself; still no response. He doesn’t realise that voice recognition capability hadn’t arrived yet.

Exasperated, he picks up the mouse and speaks into it: “Hello, computer?” The computer’s owner offers helpful advice: “Just use the keyboard.”

Scotty looks astonished. “A keyboard?” he asks, and adds in a sarcastic tone: “How quaint!”

* * *

A while later, I asked for a list of volumes for an existing app, and Claude noted that I had a few volumes that weren’t attached to any machines. So I asked it to delete the oldest unattached volume, and it did so. Then it occurred to me to do it again; this time I captured the screen:

![Deleting a volume using MCP: \"What is my oldest volume\"? ... \"Delete that volume too\"](https://fly.io/blog/mcp-provisioning/assets/volume-delete.png)

A few notes:

- I could have written a program using the [machines API](https://docs.fly.io/machines/api/volumes-resource ""), but that would have required some effort.
- I could have used [flyctl](https://docs.fly.io/flyctl/cmd/fly_volumes "") directly, but to be honest, I would have had to use the documentation and a bit of copy/pasting of arguments.
- I could have navigated to a list of volumes for this application in the Fly.io dashboard, but there currently isn’t any way to sort the list or delete a volume. Had those been in place, that would have been a reasonable alternative if that was something I was actively looking for. This felt different to me, the LLM noted something, brought it to my attention, and I asked it to make a change as a result.
- Since this support is built on `flyctl`, I would have received an error had I tried to destroy a volume that is currently mounted. Knowing that gave me the confidence to try the command.

All in all, I found it to be a very intuitive way to make changes to the resources assigned to my application.

* * *

Imagine a future where you say to your favorite LLM “launch my application on Fly.io”, and your code is scanned and you are presented with a plan containing details such as regions, databases, s3 storage, memory, machines, and the like. You are given the opportunity to adjust the plan and, when ready, say “Make it so”.

For many, the next thing you will see is that your application is up and running. For others, there may be a build error, a secret you need to set, a database that needs to be seeded, or any number of other mundane reasons why your application didn’t work the first time.

Not to fear, your LLM will be able to examine your logs and will not only make suggestions as to next steps, but will be in a position to take actions on your behalf should you wish it to do so.

And it doesn’t stop there. There will be MCP servers running on your Fly.io private network - either on separate machines, or in “sidecar” containers, or even integrated into your app. These will enable you to monitor and interact with your application.

This is not science fiction. The ingredients are all in place to make this a reality. At the moment, it is a matter of “some assembly required”, but it should only be a matter of weeks before all this comes together into a neat package..

* * *

Meanwhile, you can try this now. Make sure you run [fly version upgrade](https://docs.fly.io/flyctl/cmd/fly_version_upgrade "") and verify that you are running v0.3.117.

Then configure your favorite LLM. Here’s my `claude_desktop_config.json` for example:

```wrapper
{
  "mcpServers": {
    "fly.io": {
      "command": "/Users/rubys/.fly/bin/flyctl",
      "args": [ "mcp", "server" ]
    }
  }
}
```

Adjust the path to `flyctl` as needed. Restart your LLM, and ask what tools are available. Try a few commands and let us know what you like and let us know if you have any suggestions. Just be aware this is not a demo, if you ask it to destroy a volume, that operation is not reversable. Perhaps try this first on a throwaway application.

You don’t even need a LLM to try out the flyctl MCP server. If you have Node.js installed, you can run the [MCP Inspector](https://modelcontextprotocol.io/docs/tools/inspector ""):

```wrapper
fly mcp server -i
```

Once started, visit [http://127.0.0.1:6274/](http://127.0.0.1:6274/), click on “Connect”, then “List Tools”, select “fly-platform-status”, then click on “Run Tool”.

The plan is to see what works well and what doesn’t work so well, make adjustments, build support in a bottoms up fashion, and iterate rapidly.

By providing feedback, you can be a part of making this vision a reality.

* * *

At the present time, *most* of the following are roughed in:

- [apps](https://docs.fly.io/flyctl/cmd/fly_apps "")
- [logs](https://docs.fly.io/flyctl/cmd/fly_logs "")
- [machine](https://docs.fly.io/flyctl/cmd/fly_machine "")
- [orgs](https://docs.fly.io/flyctl/cmd/fly_orgs "")
- [platform](https://docs.fly.io/flyctl/cmd/fly_platform "")
- [status](https://docs.fly.io/flyctl/cmd/fly_status "")
- [volumes](https://docs.fly.io/flyctl/cmd/fly_volumes "")

The code is open source, and the places to look is at [server.go](https://github.com/superfly/flyctl/blob/master/internal/command/mcp/server.go "") and the [server](https://github.com/superfly/flyctl/tree/master/internal/command/mcp/server "") directory.

Feel free to open [issues](https://github.com/superfly/flyctl/issues "") or start a discussion on [community.fly.io](https://community.fly.io/ "").

* * *

Not ready yet to venture into the world of LLMs? Just remember, resistance is futile.

Next post ↑

[Launching MCP Servers on Fly.io](https://fly.io/blog/mcp-launch/)

Previous post ↓

[30 Minutes With MCP and flyctl](https://fly.io/blog/30-minute-mcp/)
