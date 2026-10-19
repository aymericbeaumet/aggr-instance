---
title: Bringing more control over your connectors
link: https://mistral.ai/news/more-control-over-connectors/
source: mistral-ai
published: 2026-06-24T12:00:30Z
updated: 2026-06-24T12:00:30Z
first_seen: 2026-10-19T01:26:53.998049939Z
content: extracted
html: 2026-06-24-bringing-more-control-over-your-connectors.html
remote_preview:
  url: https://mistral.ai/cms-media/api/media/file/cover-mistral-sovereign%20copie.webp
---

Thinking

Summary

New admin controls let you manage connector access per workspace or org, while API keys with scoped permissions and multi-account connectors enhance security for automated AI workloads. The Connectors Debugger provides detailed root cause analysis for broken connections, and integrations in Workflows and Vibe Code ensure seamless, governed access to enterprise tools. Over 60 pre-built connectors are available, with custom MCP options for additional platforms.

![](https://mistral.ai/_astro/cover-connector-1_Zx93lD.webp?dpl=6ad1d7b6946342e26a4d2b4f)

## Govern which tools run where, connect more than one account to a single connector, and trace any connection that breaks.

Today, we are introducing several new capabilities in Connectors for a more secure experience integrating to external enterprise platforms. Starting now, you can use:

- **Enriched admin controls** (GA) to set connector access per workspace and switch individual tools on or off across an org or workspace

- **API keys with connector scopes** (GA) to prevent impersonation in automated AI workloads that integrate with 3rd-party systems

- **Multi-account connectors** (GA) allowing users to authenticate to a single connector with multiple accounts

- **Connectors Debugger** (Public Preview) for end-to-end root cause analysis for MCP connectors

- **Connectors in Vibe Code** (GA) to reuse your connectors in developer interfaces

- **Connectors in Workflows** (Public Preview) allow for uninterrupted long-running tasks powered by all the tools you need

Async agents are moving into everyday work. For an agent to be trustworthy and useful inside an organization, it needs real enterprise data: CRM records, repositories, inboxes, knowledge bases. Connecting an agent to that data in a demo is easy. Running it in production is where most setups stall.

Production connectivity has a few non-negotiables. A connector should respect two sets of rules at once: the permissions already set in the source platform, and the controls your administrators set in Mistral Studio or Vibe. Automated work should run on behalf of a user or a service account, never impersonate the person who wrote it. When a connection breaks, you should be able to find out why. And an administrator should be able to decide, down to the individual tool, what is available in each part of the organization.

## More control over what connectors can do

**Enriched admin controls** work at two levels: across your teams, and inside each connector.

- **Workspace and org controls** let you give each team its own connector access. Your finance workspace can reach internal data sources with no open web access, while engineering gets developer tools and the internet. Same directory, different rules per team.

- **Tool-level controls** go a step deeper. Inside any connector, turn individual tools on or off for a whole org or one workspace. Block anything that writes data, or one specific action like `delete_file` on a knowledge base. The connector stays connected; only the tools you approve can run.

![](https://mistral.ai/_astro/media-connectors_Z1KXkTd.webp?dpl=6ad1d7b6946342e26a4d2b4f)

**API keys with connector scope** handle identity. When you create a key, you choose whether it reaches only a workspace's shared connectors or your private ones too, so an automated job runs with exactly the access it needs. Paired with service accounts, automated work runs as a defined identity, never as the person who wrote it.

**Multi-account connectors** let one connector hold more than one login. Connect a personal and a work account to the same connector, set a default, and switch between them per task. Each account is stored and refreshed on its own, so an agent acts in the right one without you standing up a second connector.

## See why a connection fails

**Connectors Debugger**  tells you exactly where a connection breaks. Point it at an MCP server URL, add OAuth credentials if the server needs them, and run the check. It walks the connection through 11 steps, from reaching the server to opening the MCP session, and logs each one. A failure that used to mean guesswork, like a broken OAuth token exchange, shows up at the exact step it happens.

[www.youtube.com](https://www.youtube.com/watch?v=QjoTkonLub4)

## **Connectors** in Workflows and Vibe for Code

**Connectors in Workflows** keep long and scheduled runs from breaking on auth. You declare the connectors a Workflow needs, and the accounts it should run with, right in the Workflow definition; Studio resolves those dependencies when you start a run. Personal credentials stay out of automated work, and a job won't fail halfway because a token expired. A nightly run can pull from a CRM, update a tracker, and read a knowledge base across the whole job without dropping a connection.

[www.youtube.com](https://www.youtube.com/watch?v=hUPcLLti5x0)

**Connectors in Vibe Code** bring the same governed access to the coding agent. Type `/connectors` to pick from local MCP servers and the workspace connectors your admins approved, choose which tools to enable, and start a task. Async agents can reach Outlook, Jira, Notion, Linear, and Confluence under the same rules you set everywhere else.

[www.youtube.com](https://www.youtube.com/watch?v=AR2IedLkEHo)

We're adding connectors continuously. The directory now covers more than 60 integrations, and when the platform you need isn't listed, a custom MCP connector fills the gap.

Knowledge, Data & AI

![connector-Atlassian (black)](https://mistral.ai/_astro/connector-atlassian-black_ZQzuqS.webp?dpl=6ad1d7b6946342e26a4d2b4f)![connector-Atlassian (white)](https://mistral.ai/_astro/connector-atlassian-white_1ntKJl.webp?dpl=6ad1d7b6946342e26a4d2b4f)

Atlassian

![connector-BigQuery (black)](https://mistral.ai/_astro/connector-bigquery-black_2izCJD.webp?dpl=6ad1d7b6946342e26a4d2b4f)![connector-BigQuery (white)](https://mistral.ai/_astro/connector-bigquery-white_ZwxeS4.webp?dpl=6ad1d7b6946342e26a4d2b4f)

BigQuery

![connector-Box (black)](https://mistral.ai/_astro/connector-box-black_1Q5I8m.webp?dpl=6ad1d7b6946342e26a4d2b4f)![connector-Box (white)](https://mistral.ai/_astro/connector-box-white_ZY29ul.webp?dpl=6ad1d7b6946342e26a4d2b4f)

Box

![connector-Google Drive (black)](https://mistral.ai/_astro/connector-google-drive-black_Z1LVse6.webp?dpl=6ad1d7b6946342e26a4d2b4f)![connector-Google Drive (white)](https://mistral.ai/_astro/connector-google-drive-white_s7MW8.webp?dpl=6ad1d7b6946342e26a4d2b4f)

Google Drive

![connector-Notion (black)](https://mistral.ai/_astro/connector-notion-black_cuHVA.webp?dpl=6ad1d7b6946342e26a4d2b4f)![connector-Notion (white)](https://mistral.ai/_astro/connector-notion-white_2ryY7O.webp?dpl=6ad1d7b6946342e26a4d2b4f)

Notion

![connector-SharePoint (black)](https://mistral.ai/_astro/connector-sharepoint-black_Z16dYN9.webp?dpl=6ad1d7b6946342e26a4d2b4f)![connector-SharePoint (white)](https://mistral.ai/_astro/connector-sharepoint-white_18Pgn5.webp?dpl=6ad1d7b6946342e26a4d2b4f)

SharePoint

![connector-Databricks (black)](https://mistral.ai/_astro/connector-databricks-black_2rw3LO.webp?dpl=6ad1d7b6946342e26a4d2b4f)![connector-Databricks (white)](https://mistral.ai/_astro/connector-databricks-white_ZnANPS.webp?dpl=6ad1d7b6946342e26a4d2b4f)

Databricks

![connector-MDN (black)](https://mistral.ai/_astro/connector-mdn-black_w25Yd.webp?dpl=6ad1d7b6946342e26a4d2b4f)![connector-MDN (white)](https://mistral.ai/_astro/connector-mdn-white_Z2j5LDu.webp?dpl=6ad1d7b6946342e26a4d2b4f)

MDN

![connector-Microsoft Learn (black)](https://mistral.ai/_astro/connector-microsoft-learn-black_Z19Pejo.webp?dpl=6ad1d7b6946342e26a4d2b4f)![connector-Microsoft Learn (white)](https://mistral.ai/_astro/connector-microsoft-learn-white_15e1QP.webp?dpl=6ad1d7b6946342e26a4d2b4f)

Microsoft Learn

![connector-Needle (black)](https://mistral.ai/_astro/connector-needle-black_Z1KNjAV.webp?dpl=6ad1d7b6946342e26a4d2b4f)![connector-Needle (white)](https://mistral.ai/_astro/connector-needle-white_tfVzi.webp?dpl=6ad1d7b6946342e26a4d2b4f)

Needle

![connector-Prisma Postgres (black)](https://mistral.ai/_astro/connector-prisma-postgres-black_Z2liqik.webp?dpl=6ad1d7b6946342e26a4d2b4f)![connector-Prisma Postgres (white)](https://mistral.ai/_astro/connector-prisma-postgres-white_Z6ea76.webp?dpl=6ad1d7b6946342e26a4d2b4f)

Prisma Postgres

![connector-Snowflake (black)](https://mistral.ai/_astro/connector-snowflake-black_Z1I9g2B.webp?dpl=6ad1d7b6946342e26a4d2b4f)![connector-Snowflake (white)](https://mistral.ai/_astro/connector-snowflake-white_vU08C.webp?dpl=6ad1d7b6946342e26a4d2b4f)

Snowflake

![connector-Supabase (black)](https://mistral.ai/_astro/connector-supabase-black_ZxwHI3.webp?dpl=6ad1d7b6946342e26a4d2b4f)![connector-Supabase (white)](https://mistral.ai/_astro/connector-supabase-white_1Gwxsb.webp?dpl=6ad1d7b6946342e26a4d2b4f)

Supabase

![connector-Synapse (black)](https://mistral.ai/_astro/connector-synapse-black_Wx9i8.webp?dpl=6ad1d7b6946342e26a4d2b4f)![connector-Synapse (white)](https://mistral.ai/_astro/connector-synapse-white_Z1RzIkz.webp?dpl=6ad1d7b6946342e26a4d2b4f)

Synapse

Machine Learning

![connector-DeepWiki (black)](https://mistral.ai/_astro/connector-deepwiki-black_2Xoiq.webp?dpl=6ad1d7b6946342e26a4d2b4f)![connector-DeepWiki (white)](https://mistral.ai/_astro/connector-deepwiki-white_2i2EtE.webp?dpl=6ad1d7b6946342e26a4d2b4f)

DeepWiki

![connector-Hugging Face (black)](https://mistral.ai/_astro/connector-hugging-face-black_GCvPo.webp?dpl=6ad1d7b6946342e26a4d2b4f)![connector-Hugging Face (white)](https://mistral.ai/_astro/connector-hugging-face-white_Z28ulMj.webp?dpl=6ad1d7b6946342e26a4d2b4f)

Hugging Face

![connector-Jina (black)](https://mistral.ai/_astro/connector-jina-black_Z117SBJ.webp?dpl=6ad1d7b6946342e26a4d2b4f)![connector-Jina (white)](https://mistral.ai/_astro/connector-jina-white_1dVmyu.webp?dpl=6ad1d7b6946342e26a4d2b4f)

Jina

![connector-Pinecone (black)](https://mistral.ai/_astro/connector-pinecone-black_Z3c4Il.webp?dpl=6ad1d7b6946342e26a4d2b4f)![connector-Pinecone (white)](https://mistral.ai/_astro/connector-pinecone-white_2bRbrS.webp?dpl=6ad1d7b6946342e26a4d2b4f)

Pinecone

![connector-Tavily (black)](https://mistral.ai/_astro/connector-tavily-black_ZoFsG7.webp?dpl=6ad1d7b6946342e26a4d2b4f)![connector-Tavily (white)](https://mistral.ai/_astro/connector-tavily-white_1PnMu7.webp?dpl=6ad1d7b6946342e26a4d2b4f)

Tavily

Communication & Scheduling

![connector-Gmail (black)](https://mistral.ai/_astro/connector-gmail-black_O6iwB.webp?dpl=6ad1d7b6946342e26a4d2b4f)![connector-Gmail (white)](https://mistral.ai/_astro/connector-gmail-white_Z211z66.webp?dpl=6ad1d7b6946342e26a4d2b4f)

Gmail

![connector-Google Calendar (black)](https://mistral.ai/_astro/connector-google-calendar-black_4it5T.webp?dpl=6ad1d7b6946342e26a4d2b4f)![connector-Google Calendar (white)](https://mistral.ai/_astro/connector-google-calendar-white_2jmJh8.webp?dpl=6ad1d7b6946342e26a4d2b4f)

Google Calendar

![connector-Outlook (black)](https://mistral.ai/_astro/connector-outlook-black_Z8sKnJ.webp?dpl=6ad1d7b6946342e26a4d2b4f)![connector-Outlook (white)](https://mistral.ai/_astro/connector-outlook-white_26AuMu.webp?dpl=6ad1d7b6946342e26a4d2b4f)

Outlook

![connector-Outlook Calendar (black)](https://mistral.ai/_astro/connector-outlook-calendar-black_ZdtHCW.webp?dpl=6ad1d7b6946342e26a4d2b4f)![connector-Outlook Calendar (white)](https://mistral.ai/_astro/connector-outlook-calendar-white_21zxxh.webp?dpl=6ad1d7b6946342e26a4d2b4f)

Outlook Calendar

![connector-Slack (black)](https://mistral.ai/_astro/connector-slack-black_2nOfv4.webp?dpl=6ad1d7b6946342e26a4d2b4f)![connector-Slack (white)](https://mistral.ai/_astro/connector-slack-white_ZriC7D.webp?dpl=6ad1d7b6946342e26a4d2b4f)

Slack

![connector-Brevo (black)](https://mistral.ai/_astro/connector-brevo-black_2oWJik.webp?dpl=6ad1d7b6946342e26a4d2b4f)![connector-Brevo (white)](https://mistral.ai/_astro/connector-brevo-white_Zqa8kn.webp?dpl=6ad1d7b6946342e26a4d2b4f)

Brevo

![connector-Clockwise (black)](https://mistral.ai/_astro/connector-clockwise-black_oJATP.webp?dpl=6ad1d7b6946342e26a4d2b4f)![connector-Clockwise (white)](https://mistral.ai/_astro/connector-clockwise-white_Z2qngHR.webp?dpl=6ad1d7b6946342e26a4d2b4f)

Clockwise

![connector-Fireflies (black)](https://mistral.ai/_astro/connector-fireflies-black_2vbkYk.webp?dpl=6ad1d7b6946342e26a4d2b4f)![connector-Fireflies (white)](https://mistral.ai/_astro/connector-fireflies-white_ZjVwDn.webp?dpl=6ad1d7b6946342e26a4d2b4f)

Fireflies

Work & Customer Operations

![connector-Asana (black)](https://mistral.ai/_astro/connector-asana-black_Zh1enC.webp?dpl=6ad1d7b6946342e26a4d2b4f)![connector-Asana (white)](https://mistral.ai/_astro/connector-asana-white_1X31MB.webp?dpl=6ad1d7b6946342e26a4d2b4f)

Asana

![connector-Linear (black)](https://mistral.ai/_astro/connector-linear-black_11y4Kc.webp?dpl=6ad1d7b6946342e26a4d2b4f)![connector-Linear (white)](https://mistral.ai/_astro/connector-linear-white_Z1NyMRv.webp?dpl=6ad1d7b6946342e26a4d2b4f)

Linear

![connector-Monday (black)](https://mistral.ai/_astro/connector-monday-black_Jp8Na.webp?dpl=6ad1d7b6946342e26a4d2b4f)![connector-Monday (white)](https://mistral.ai/_astro/connector-monday-white_Z25HIOx.webp?dpl=6ad1d7b6946342e26a4d2b4f)

Monday

![connector-Close (black)](https://mistral.ai/_astro/connector-close-black_ZgLhaV.webp?dpl=6ad1d7b6946342e26a4d2b4f)![connector-Close (white)](https://mistral.ai/_astro/connector-close-white_1XhY0i.webp?dpl=6ad1d7b6946342e26a4d2b4f)

Close

![connector-HubSpot (black)](https://mistral.ai/_astro/connector-hubspot-black_vAhSS.webp?dpl=6ad1d7b6946342e26a4d2b4f)![connector-HubSpot (white)](https://mistral.ai/_astro/connector-hubspot-white_Z2jwzIO.webp?dpl=6ad1d7b6946342e26a4d2b4f)

HubSpot

![connector-Intercom (black)](https://mistral.ai/_astro/connector-intercom-black_1xAJnI.webp?dpl=6ad1d7b6946342e26a4d2b4f)![connector-Intercom (white)](https://mistral.ai/_astro/connector-intercom-white_Z1hw8eY.webp?dpl=6ad1d7b6946342e26a4d2b4f)

Intercom

![connector-Salesforce (black)](https://mistral.ai/_astro/connector-salesforce-black_MUFXw.webp?dpl=6ad1d7b6946342e26a4d2b4f)![connector-Salesforce (white)](https://mistral.ai/_astro/connector-salesforce-white_Z22cbEb.webp?dpl=6ad1d7b6946342e26a4d2b4f)

Salesforce

![connector-ServiceNow (black)](https://mistral.ai/_astro/connector-servicenow-black_Z1P6gME.webp?dpl=6ad1d7b6946342e26a4d2b4f)![connector-ServiceNow (white)](https://mistral.ai/_astro/connector-servicenow-white_oWYnz.webp?dpl=6ad1d7b6946342e26a4d2b4f)

ServiceNow

Payments & Financial Data

![connector-Stripe (black)](https://mistral.ai/_astro/connector-stripe-black_Z1P1Rwo.webp?dpl=6ad1d7b6946342e26a4d2b4f)![connector-Stripe (white)](https://mistral.ai/_astro/connector-stripe-white_p2nDP.webp?dpl=6ad1d7b6946342e26a4d2b4f)

Stripe

![connector-PayPal (black)](https://mistral.ai/_astro/connector-paypal-black_1KQYDS.webp?dpl=6ad1d7b6946342e26a4d2b4f)![connector-PayPal (white)](https://mistral.ai/_astro/connector-paypal-white_Z14fRXO.webp?dpl=6ad1d7b6946342e26a4d2b4f)

PayPal

![connector-Square (black)](https://mistral.ai/_astro/connector-square-black_1qfFax.webp?dpl=6ad1d7b6946342e26a4d2b4f)![connector-Square (white)](https://mistral.ai/_astro/connector-square-white_Z1oRcsa.webp?dpl=6ad1d7b6946342e26a4d2b4f)

Square

![connector-Plaid (black)](https://mistral.ai/_astro/connector-plaid-black_ZzEfLO.webp?dpl=6ad1d7b6946342e26a4d2b4f)![connector-Plaid (white)](https://mistral.ai/_astro/connector-plaid-white_1Ep0op.webp?dpl=6ad1d7b6946342e26a4d2b4f)

Plaid

![connector-Morningstar (black)](https://mistral.ai/_astro/connector-morningstar-black_ZJ9TBD.webp?dpl=6ad1d7b6946342e26a4d2b4f)![connector-Morningstar (white)](https://mistral.ai/_astro/connector-morningstar-white_1uTlyA.webp?dpl=6ad1d7b6946342e26a4d2b4f)

Morningstar

![connector-Kensho (black)](https://mistral.ai/_astro/connector-kensho-black_Z2wCGrM.webp?dpl=6ad1d7b6946342e26a4d2b4f)![connector-Kensho (white)](https://mistral.ai/_astro/connector-kensho-white_Zhyqgy.webp?dpl=6ad1d7b6946342e26a4d2b4f)

Kensho

![connector-Pigment (black)](https://mistral.ai/_astro/connector-pigment-black_ZtOjeP.webp?dpl=6ad1d7b6946342e26a4d2b4f)![connector-Pigment (white)](https://mistral.ai/_astro/connector-pigment-white_1KeVVo.webp?dpl=6ad1d7b6946342e26a4d2b4f)

Pigment

Analytics & Business Intelligence

![connector-Amplitude (black)](https://mistral.ai/_astro/connector-amplitude-black_2im1Tu.webp?dpl=6ad1d7b6946342e26a4d2b4f)![connector-Amplitude (white)](https://mistral.ai/_astro/connector-amplitude-white_ZwKPId.webp?dpl=6ad1d7b6946342e26a4d2b4f)

Amplitude

![connector-Hex (black)](https://mistral.ai/_astro/connector-hex-black_Z1NtJ8.webp?dpl=6ad1d7b6946342e26a4d2b4f)![connector-Hex (white)](https://mistral.ai/_astro/connector-hex-white_2dfLr6.webp?dpl=6ad1d7b6946342e26a4d2b4f)

Hex

![connector-Vantage (black)](https://mistral.ai/_astro/connector-vantage-black_1w4lFA.webp?dpl=6ad1d7b6946342e26a4d2b4f)![connector-Vantage (white)](https://mistral.ai/_astro/connector-vantage-white_Z1j3vW7.webp?dpl=6ad1d7b6946342e26a4d2b4f)

Vantage

Developer Platforms & Infrastructure

![connector-GitHub (black)](https://mistral.ai/_astro/connector-github-black_RaQS3.webp?dpl=6ad1d7b6946342e26a4d2b4f)![connector-GitHub (white)](https://mistral.ai/_astro/connector-github-white_Z1WW0JE.webp?dpl=6ad1d7b6946342e26a4d2b4f)

GitHub

![connector-Cloudflare Developer Platform (black)](https://mistral.ai/_astro/connector-cloudflare-developer-platform-black_ZINN8X.webp?dpl=6ad1d7b6946342e26a4d2b4f)![connector-Cloudflare Developer Platform (white)](https://mistral.ai/_astro/connector-cloudflare-developer-platform-white_1vfs2g.webp?dpl=6ad1d7b6946342e26a4d2b4f)

Cloudflare Developer Platform

![connector-Netlify (black)](https://mistral.ai/_astro/connector-netlify-black_bmhAg.webp?dpl=6ad1d7b6946342e26a4d2b4f)![connector-Netlify (white)](https://mistral.ai/_astro/connector-netlify-white_2qqxLu.webp?dpl=6ad1d7b6946342e26a4d2b4f)

Netlify

![connector-Sentry (black)](https://mistral.ai/_astro/connector-sentry-black_Z1cGtv5.webp?dpl=6ad1d7b6946342e26a4d2b4f)![connector-Sentry (white)](https://mistral.ai/_astro/connector-sentry-white_12mLF9.webp?dpl=6ad1d7b6946342e26a4d2b4f)

Sentry

![connector-Stytch (black)](https://mistral.ai/_astro/connector-stytch-black_eu0mE.webp?dpl=6ad1d7b6946342e26a4d2b4f)![connector-Stytch (white)](https://mistral.ai/_astro/connector-stytch-white_2tygxS.webp?dpl=6ad1d7b6946342e26a4d2b4f)

Stytch

Automation & Integration

![connector-Apify (black)](https://mistral.ai/_astro/connector-apify-black_ZFCNAm.webp?dpl=6ad1d7b6946342e26a4d2b4f)![connector-Apify (white)](https://mistral.ai/_astro/connector-apify-white_1yqrzR.webp?dpl=6ad1d7b6946342e26a4d2b4f)

Apify

![connector-n8n (black)](https://mistral.ai/_astro/connector-n8n-black_W8VGo.webp?dpl=6ad1d7b6946342e26a4d2b4f)![connector-n8n (white)](https://mistral.ai/_astro/connector-n8n-white_Z1RXUVj.webp?dpl=6ad1d7b6946342e26a4d2b4f)

n8n

![connector-Workato (black)](https://mistral.ai/_astro/connector-workato-black_ZnkTEW.webp?dpl=6ad1d7b6946342e26a4d2b4f)![connector-Workato (white)](https://mistral.ai/_astro/connector-workato-white_1QIlvh.webp?dpl=6ad1d7b6946342e26a4d2b4f)

Workato

![connector-Zapier (black)](https://mistral.ai/_astro/connector-zapier-black_ZpOtEW.webp?dpl=6ad1d7b6946342e26a4d2b4f)![connector-Zapier (white)](https://mistral.ai/_astro/connector-zapier-white_1OeLvh.webp?dpl=6ad1d7b6946342e26a4d2b4f)

Zapier

Research, Scientific & Public Data

![connector-BioRender (black)](https://mistral.ai/_astro/connector-biorender-black_Z2dMKnF.webp?dpl=6ad1d7b6946342e26a4d2b4f)![connector-BioRender (white)](https://mistral.ai/_astro/connector-biorender-white_1guMy.webp?dpl=6ad1d7b6946342e26a4d2b4f)

BioRender

![connector-Data.gouv (black)](https://mistral.ai/_astro/connector-datagouv-black_ZsCqM.webp?dpl=6ad1d7b6946342e26a4d2b4f)![connector-Data.gouv (white)](https://mistral.ai/_astro/connector-datagouv-white_2eACJr.webp?dpl=6ad1d7b6946342e26a4d2b4f)

Data.gouv

![connector-PubMed (black)](https://mistral.ai/_astro/connector-pubmed-black_ZxQIVO.webp?dpl=6ad1d7b6946342e26a4d2b4f)![connector-PubMed (white)](https://mistral.ai/_astro/connector-pubmed-white_1Gcwep.webp?dpl=6ad1d7b6946342e26a4d2b4f)

PubMed

![connector-Scholar Gateway (black)](https://mistral.ai/_astro/connector-scholar-gateway-black_Z1ScDUk.webp?dpl=6ad1d7b6946342e26a4d2b4f)![connector-Scholar Gateway (white)](https://mistral.ai/_astro/connector-scholar-gateway-white_lQBfT.webp?dpl=6ad1d7b6946342e26a4d2b4f)

Scholar Gateway

Content & Media Management

![connector-Bria (black)](https://mistral.ai/_astro/connector-bria-black_Z1KnDVJ.webp?dpl=6ad1d7b6946342e26a4d2b4f)![connector-Bria (white)](https://mistral.ai/_astro/connector-bria-white_tFBeu.webp?dpl=6ad1d7b6946342e26a4d2b4f)

Bria

![connector-Bright (black)](https://mistral.ai/_astro/connector-bright-black_Zeykdo.webp?dpl=6ad1d7b6946342e26a4d2b4f)![connector-Bright (white)](https://mistral.ai/_astro/connector-bright-white_20uUWP.webp?dpl=6ad1d7b6946342e26a4d2b4f)

Bright

![connector-Cloudinary (black)](https://mistral.ai/_astro/connector-cloudinary-black_EtdvC.webp?dpl=6ad1d7b6946342e26a4d2b4f)![connector-Cloudinary (white)](https://mistral.ai/_astro/connector-cloudinary-white_Z2aDE75.webp?dpl=6ad1d7b6946342e26a4d2b4f)

Cloudinary

![connector-Mermaid (black)](https://mistral.ai/_astro/connector-mermaid-black_1JtPqa.webp?dpl=6ad1d7b6946342e26a4d2b4f)![connector-Mermaid (white)](https://mistral.ai/_astro/connector-mermaid-white_Z15D2cx.webp?dpl=6ad1d7b6946342e26a4d2b4f)

Mermaid

![connector-Trivago (black)](https://mistral.ai/_astro/connector-trivago-black_L1dFV.webp?dpl=6ad1d7b6946342e26a4d2b4f)![connector-Trivago (white)](https://mistral.ai/_astro/connector-trivago-white_Z246DVL.webp?dpl=6ad1d7b6946342e26a4d2b4f)

Trivago

![connector-Website Publisher (black)](https://mistral.ai/_astro/connector-website-publisher-black_1a6RtE.webp?dpl=6ad1d7b6946342e26a4d2b4f)![connector-Website Publisher (white)](https://mistral.ai/_astro/connector-website-publisher-white_Z1F1093.webp?dpl=6ad1d7b6946342e26a4d2b4f)

Website Publisher

## Start building

Enterprise controls, Connector Credentials, and Connectors in Vibe Code are generally available now. Connectors Playground and Connectors in Workflows are in public preview. All of it is live in Studio.

- [Connectors documentation](https://docs.mistral.ai/studio-api/connectors/)

- [Connectors in Vibe Code documentation](https://docs.mistral.ai/vibe/code/cli/connectors)

- [Connectors in Workflows documentation and cookbooks](https://docs.mistral.ai/studio-api/workflows/building-workflows/connectors)

- [Open the Studio console](https://console.mistral.ai/build/connectors)
