---
title: Building a multiplayer game world with autonomous Fable-class models
link: https://simonwillison.net/2026/Oct/29/how-i-built-the-most-ai-pilled-company-in-the-world/
source: simonwillison-net
published: 2026-10-29T17:00:26Z
updated: 2026-10-29T17:00:26Z
first_seen: 2026-10-29T20:30:27.834895129Z
labels:
- agentic-engineering
- ai
- claude
- claude-mythos-fable
- coding-agents
- game-design
- generative-ai
- llms
summary: 'How I Built the Most AI-Pilled Company in the World Don''t be put off by the clickbait headline, this piece by Jacob Sansbury describing how they are building Spawn is a fascinating exploration of the bleeding edge of what''s possible with agentic engineering against frontier models. Spawn is a massively multiplayer gaming environment where players can use prompts (to the in-game agent, Savi) to modify the game world. I fired it up on my phone for a few minutes and was able to prompt a castle into existence, then a dragon, then ride the dragon to fly around that castle. By Jacob''s telling, Spawn is now being developed autonomously by a swarm of Fable-powered agents costing over $3 million in tokens per month ("we spend 30x payroll on AI tokens"). Our agents change the live code directly and ship to production hourly, so they read every error, bug report and metric themselves. About 60% of our work starts from a production error, a Savi bug report (Savi is the AI you build games with in Spawn), something odd in our logs, or a feature request. Savi files bugs herself. When a user hits one, or Savi hits one in the product, she files it. It goes straight to an agent that decides whether it''s a real bug to fix, something we won''t fix, or intentional design that needs a decision. Usually it becomes a change, delivered to that user within an hour, with a message in the same chat. This is continuous deployment turned up to 11: A loop this tight only works if it never stops, so we treat an hour without a ship as a SEV-0, our most severe kind of incident. Unshipped changes pile up, and every problem already in production lasts longer. The feedback slows down too. An agent can only keep a few hours of work in its head, so shipping faster than that actually helps it stay coherent. [...] We can roll back a broken change in 30 seconds, exactly where it''s broken and nowhere else. When a monitor or a log alert fires, it starts or wakes an agent that owns the incident, and we break things on purpose to test that those recovery paths work. My eyebrows raised at least a dozen times reading this. I have trouble imagining this style of development working on software with higher stakes for when something goes wrong, but for a game this appears to be working surprisingly well. Jacob says that Fable 5 (and then 5.1) was the model that made this level of autonomy possible. Opus 5.5 doesn''t quite hack it for them, and when they tried GPT-6 Astra to apply fixes from a security review everything stopped working: Every feature had been disabled and marked to be turned on again once the security fixes were done. Astra had gone through our whole product and made the insecure things secure by turning them off. These kinds of extreme experiments are why I''m excited about game development as a substrate for exploring the edges of what today''s coding agents can do. Tags: game-design, ai, generative-ai, llms, claude, coding-agents, agentic-engineering, claude-mythos-fable'
content: extracted
html: 2026-10-29-building-a-multiplayer-game-world-with-autonomous-fable.html
remote_preview:
  url: https://static.simonwillison.net/static/2026-10-29/IMG_9406.webp
  alt: 'Two mobile screenshots side by side from a 3D voxel-style game. Left: a first-person view from the saddle of a small teal dragon, its neck with pale diamond markings and two curved white horns in the foreground, flying toward a dark stone castle with tall teal-gray conical spires, glowing windows an'
---

**[How I Built the Most AI-Pilled Company in the World](https://twitter.com/jsnnsa/status/2115623382721126826)**. Don't be put off by the clickbait headline, this piece by Jacob Sansbury describing how they are building [Spawn](https://spawn.co) is a fascinating exploration of the bleeding edge of what's possible with agentic engineering against frontier models.

Spawn is a massively multiplayer gaming environment where players can use prompts (to the in-game agent, Savi) to modify the game world.

I fired it up on my phone for a few minutes and was able to prompt a castle into existence, then a dragon, then ride the dragon to fly around that castle.

![Two mobile screenshots side by side from a 3D voxel-style game. Left: a first-person view from the saddle of a small teal dragon, its neck with pale diamond markings and two curved white horns in the foreground, flying toward a dark stone castle with tall teal-gray conical spires, glowing windows and teal pennants, on a rocky island in a lake with green and snowy hills behind. Top bar: profile icon, \"keep it\", \"your den\" dropdown, \"+ new game\" button and a \"...\" menu. Right side: icon buttons and a portrait of a white-haired, freckled character labeled \"tap to talk\". Bottom right buttons: \"JUMP\" and \"USE\". Right: the same game with the chat panel open over the castle scene. Top bar: \"your den\" dropdown, \"+ new game\". Button: \"earlier ↑\". User message: \"Make the dragon smaller and land it next to me again\". Replies from the white-haired character: \"on it. shrinking it to a pony-sized dragon and landing it right beside you.\" Then: \"it's a little dragon now and you're already up on it over the lake. your view sits right in its saddle, horns ahead. fly back to the house and look down. saw your screen just now: flying over the water, the neck and horns ahead, the house on the far shore\" Then: \"oh, idea: USE while flying could breathe fire instead, with JUMP held to land. want it?\" Input box: \"what do you want to bring in?\" Buttons below: upload, a small pixel-art icon, \"lock in\" and a microphone.](https://static.simonwillison.net/static/2026-10-29/IMG_9406.webp)

By Jacob's telling, Spawn is now being developed autonomously by a swarm of Fable-powered agents costing over $3 million in tokens *per month* ("we spend 30x payroll on AI tokens").

> Our agents change the live code directly and ship to production hourly, so they read every error, bug report and metric themselves. About 60% of our work starts from a production error, a Savi bug report (Savi is the AI you build games with in Spawn), something odd in our logs, or a feature request.
>
> Savi files bugs herself. When a user hits one, or Savi hits one in the product, she files it. It goes straight to an agent that decides whether it's a real bug to fix, something we won't fix, or intentional design that needs a decision. Usually it becomes a change, delivered to that user within an hour, with a message in the same chat.

This is [continuous deployment](https://en.wikipedia.org/wiki/Continuous_deployment) turned up to 11:

> A loop this tight only works if it never stops, so we treat an hour without a ship as a SEV-0, our most severe kind of incident. Unshipped changes pile up, and every problem already in production lasts longer. The feedback slows down too. An agent can only keep a few hours of work in its head, so shipping faster than that actually helps it stay coherent. \[...\]
>
> We can roll back a broken change in 30 seconds, exactly where it's broken and nowhere else. When a monitor or a log alert fires, it starts or wakes an agent that owns the incident, and we break things on purpose to test that those recovery paths work.

My eyebrows raised at least a dozen times reading this. I have trouble imagining this style of development working on software with higher stakes for when something goes wrong, but for a game this appears to be working surprisingly well.

Jacob says that Fable 5 (and then 5.1) was the model that made this level of autonomy possible. Opus 5.5 doesn't quite hack it for them, and when they tried GPT-6 Astra to apply fixes from a security review everything stopped working:

> Every feature had been disabled and marked to be turned on again once the security fixes were done. Astra had gone through our whole product and made the insecure things secure by turning them off.

These kinds of extreme experiments are why I'm [excited about game development](https://simonwillison.net/2026/Oct/28/vibe-game-development/) as a substrate for exploring the edges of what today's coding agents can do.
