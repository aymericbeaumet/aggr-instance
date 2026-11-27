---
title: What are MCP Servers?
link: https://fly.io/blog/mcps-everywhere/
source: fly-io
published: 2025-06-12T00:00:00Z
updated: 2026-11-05T00:08:46Z
first_seen: 2026-11-27T00:01:05.630507547Z
content: extracted
html: 2025-06-12-what-are-mcp-servers.html
remote_preview:
  url: https://fly.io/blog/mcps-everywhere/assets/mcps-everywhere.webp
  alt: A grassy park with trees and flowers and little playful M's with eyes playing and hiding and relaxing
extra:
  thumbnail: https://fly.io/blog/mcps-everywhere/assets/mcps-everywhere.webp
---

Author

![Sam Ruby](https://fly.io/static/images/sam.webp)

Name

Sam Ruby

bsky:intertwingly.net

[bsky:intertwingly.net](https://bsky.app/profile/intertwingly.net)

![A grassy park with trees and flowers and little playful M's with eyes playing and hiding and relaxing](https://fly.io/blog/mcps-everywhere/assets/mcps-everywhere.webp)\
 Image by [Annie Ruygt](https://annieruygtillustration.com/)

The introduction to [Model Context Protocol](https://modelcontextprotocol.io/introduction "") starts out with:

> MCP is an open protocol that standardizes how applications provide context to LLMs. Think of MCP like a USB-C port for AI applications. Just as USB-C provides a standardized way to connect your devices to various peripherals and accessories, MCP provides a standardized way to connect AI models to different data sources and tools.

That paragraph, to me, is both comforting (“USB for LLM”? Cool! Got it!), and simultaneously vacuous (Um, but what do I actually *do* with this?).

I’ve been digging deeper and have come up with a few more analogies and observations that make sense to me. Perhaps one or more of these will help you.

## MCPs are Alexa Skills

You buy an Echo Dot and a Hue light. You plug them both in and connect them to your Wi-Fi. There is one more step you need to do: you need to install and enable the Philips Hue skill to connect the two.

Now you might be using Siri or Google Assistant. Or you may want to connect a Ring Doorbell camera or Google Nest Thermostat. But the principle is the same, though the analogy is slightly stronger with a skill (which is a noun) as opposed to the action of pairing your Hue Bridge with Apple HomeKit (a verb).

## MCPs are API 2.0

HTTP 1.1 is simple. You send a request, you get a response. While there are cookies and sessions, it is pretty much stateless and therefore inefficient, as each request needs to establish a new connection. WebSockets and Server-Sent Events (SSE) mitigate this a bit.

[HTTP 2.0](https://en.wikipedia.org/wiki/HTTP/2 "") introduces features like multiplexing and server push. When you visit a web page for the first time, your browser can now request all of the associated JavaScript, CSS, and images at once, and the server can respond in any order.

APIs today are typically request/response. MCPs support multiplexing and server push.

## MCPs are APIs with Introspection/Reflection

With [OpenAPI](https://learn.openapis.org/ ""), requests are typically JSON, and responses are too. Many OpenAPI providers publish a separate [OpenAPI Description (OAD)](https://learn.openapis.org/specification/structure.html ""), which contains a schema describing what requests are supported by that API.

With MCP, requests are also JSON and responses are also JSON, but in addition, there are standard requests built into the protocol to obtain useful information, including what tools are provided, what arguments each tool expects, and a prose description of how each tool is expected to be used.

As an aside, don’t automatically assume that you will get good results from [auto-generating MCP Servers from OpenAPI schemas](https://neon.com/blog/autogenerating-mcp-servers-openai-schemas ""):

> Effective MCP design means thinking about the workflows you want the agent to perform and potentially creating higher-level tools that encapsulate multiple API calls, rather than just exposing the raw building blocks of your entire API spec.

[MCP vs API](https://glama.ai/blog/2025-06-06-mcp-vs-api#five-fundamental-differences "") goes into this topic at greater depth.

In many cases you will get better results treating LLMs as humans. If you have a CLI, consider using that as the starting point instead.

## MCPs are ***not*** serverless

[Serverless](https://en.wikipedia.org/wiki/Serverless_computing ""), sometimes known as [FaaS](https://en.wikipedia.org/wiki/Function_as_a_service ""), is a popular cloud computing model, where AWS Lambda is widely recognized as the archetype.

MCP servers are not serverless; they have a well-defined and long-lived [lifecycle](https://modelcontextprotocol.io/specification/2025-03-26/basic/lifecycle ""):

## You can play with this right now.

MCPs are barely six months old, but we are keeping up with the latest

[Try launching your MCP server on Fly.io today →](https://docs.fly.io/model-context-protocol)

![](https://fly.io/static/images/cta-turtle@2x.webp)

## MCPs are ***not*** Inherently Secure or Private

Here I am not talking about [prompt injection](https://simonwillison.net/2025/Apr/9/mcp-prompt-injection/ "") or [exploitable abilities](https://simonwillison.net/2025/May/26/github-mcp-exploited/ ""), though those are real problems too.

I’m talking about something more fundamental and basic. Let’s take a look at the very same [GitHub MCP](https://github.com/github/github-mcp-server "") featured in the above two descriptions of an exploitable exposure. The current process for installing a stdio MCP into Claude involves placing secrets in plain text in a well-defined location. And the process for installing the *next* MCP server is to download a program from a third party and run that tool in a way that has access to this very file.

Addressing MCP security requires a holistic approach, but one key strategy component is the ability to run an MCP server on a remote machine which can only be accessed by you, and only after you present a revocable bearer token. That remote machine may require access to secrets (like your GitHub token), but those secrets are not something either your LLM client or other MCP servers will be able to access directly.

## MCPs should be considered family

Recapping: [Philips](https://www.usa.philips.com/ "") has an [API and SDK](https://developers.meethue.com/ "") for Hue that is used by perhaps thousands, and has an [Alexa Skill](https://www.amazon.com/Philips-Hue/dp/B01LWAGUG7 "") that is used by untold millions. Of course, somebody already built a [Philips Hue MCP Server](https://github.com/ThomasRohde/hue-mcp "").

LLMs are brains. MCPs give LLMs eyes, hands, and legs. The end result is a robot. Most MCPs today are brainless—they merely are eyes, hands, and legs. The results are appliances. I buy and install a dishwasher. You do too. Both of our dishwashers perform the same basic function. As do any Hue lights we may buy.

In The Jetsons, [Rosie](https://thejetsons.fandom.com/wiki/Rosey "") is a maid and housekeeper who is also a member of the family. She is good friends with Jane and takes the role of a surrogate aunt towards Elroy and Judy. Let’s start there and go further.

A person with an LLM can build things. Imagine having a 3D printer that makes robots or agents that act on your behalf. You may want an agent to watch for trends in your business, keep track of what events are happening in your area, screen your text messages, or act as a DJ when you get together with friends.

You may share the MCP servers you create with others to use as starting points, but they undoubtedly will adapt them and make them their own.

## Closing Thoughts

Don’t get me wrong. I am not saying there won’t be MCPs that are mere appliances—there undoubtedly will be plenty of those. But not all MCPs will be appliances; many will be agents.

Today, most people exploring LLMs are trying to add LLMs to things they already have—for example, IDEs. MCPs flip this. Instead of adding LLMs to something you already have, you add something you already have to an LLM.

[Desktop Commander MCP](https://desktopcommander.app/ "") is an example I’m particularly fond of that does exactly this. It also happens to be a prime example of a tool you want to give root access to, then lock it in a secure box and run someplace other than your laptop. [Give it a try](https://docs.fly.io/model-context-protocol/examples#desktop-commander "").

Microsoft is actively working on [Agentic Windows](https://developer.microsoft.com/en-us/windows/agentic/ ""). Your smartphone is the obvious next frontier. Today, you have an app store and a web browser. MCP servers have the potential to make both obsolete—and this could happen sooner than you think.

Next post ↑

[Phoenix.new – The Remote AI Runtime for Phoenix](https://fly.io/blog/phoenix-new-the-remote-ai-runtime/)

Previous post ↓

[My AI Skeptic Friends Are All Nuts](https://fly.io/blog/youre-all-nuts/)
