---
name: juma-mcp-marketing-flows
display-name: Juma MCP Marketing Flows
description: >
  Run Juma's marketing Flows (juma.ai/flows) from any AI tool on the Juma MCP: grounded in the team's
  brand + knowledge and connected integrations, returning finished, on-brand deliverables. Use for ads,
  SEO, social, analytics & reporting, content, campaigns, personas/ICP, GEO/AEO, and saved or custom
  Flows. Triggers on marketing tasks, "use Juma to…", or "/juma-mcp-marketing-flows".
---

# Juma MCP Marketing Flows

Operating guide for marketing work over the **Juma MCP** — reproduce Juma's Flows
([juma.ai/flows](https://juma.ai/flows)) by composing MCP tools + connected integrations, on the team's
brand and knowledge, saved to the workspace. The rules and method apply to any marketing task; the
routing table is the Flow menu.

Requires the Juma MCP connected (`https://mcp.juma.ai/mcp`). Tools are shown here by bare name
(`create_thread`, `run_integration_tool`, …); your client prefixes them (`mcp__juma__…`).

## Rules (every task)

- **Ground first.** Resolve a `projectId` and read its knowledge before generating; pass `projectId`
  to every run so Juma applies the team's brand + knowledge. Ungrounded output is the failure to avoid.
- **Never invent metrics.** Pull them from an integration, upload, or web; label the source. No data →
  say so and offer to connect the integration.
- **Missing integration → connect, don't guess.** Send the teammate to
  **https://app.juma.ai/?integrations=all** (name the app), then re-run.
- **Credits.** Generation costs credits (`create_campaign` most). Confirm before large runs or external
  posts. Read-only steps (`list_*`/`get_*`/`search_*`/`read_*`) run freely — no confirmation.
- **Save reusable outputs** (ICP, personas, voice profiles, briefs) with `add_knowledge_item` so the
  team and future runs inherit them.
- **Let Juma do the deep work.** Write a strong prompt; its agent researches + builds. You route,
  ground, feed live data, deliver.

## Method — five steps

Category files add each Flow's exact input prompt + deliverables.

**1. Route.** Match the ask to a Flow in the routing table; open its category file. Multi-part ask →
chain (diagnose, then produce). No match → run it as a custom Flow (below); never refuse.

**2. Ground.**

- **Project:** `list_projects` → pick the client/brand/topic project; pass its `id` as `projectId`. No
  project + ongoing work → `create_project`; for a new client seed it (`import_brand_profile_from_url`
  → `create_brand_profile` → `assign_brand_to_project` + `add_knowledge_item`). One-off → `projectId`
  optional.
- **Voice + facts:** `get_brand` for the voice; `search_knowledge_items` → `read_knowledge_items` for
  ICP, positioning, past briefs — fold them into the brief; don't make Juma re-derive them.

**3. Pull live data.** Reach GA4, Google/Meta Ads, Search Console, Ahrefs/DataForSEO, LinkedIn,
Instagram, PostHog, Mixpanel, HubSpot, Notion — plus 3,000+ Pipedream apps — via two tools:
`search_integration_tools({ query })` to discover the exact tool by capability, then
`run_integration_tool({ name, input })` to execute (copy `name` verbatim; auth handled server-side).
Only **already-connected** apps are searchable — nothing relevant means it isn't connected (see Rules).
`web_search` / `web_scrape` cover current facts + public competitor content when no integration fits.

| Need                                | query                                               | App                   |
| ----------------------------------- | --------------------------------------------------- | --------------------- |
| Sessions, conversions, channels     | `"google analytics ga4 report"`                     | Google Analytics      |
| Impressions, clicks, CTR, position  | `"search console query performance"`                | Google Search Console |
| Ad spend, campaigns, keywords       | `"google ads campaign performance"`                 | Google Ads            |
| Paid social performance             | `"meta ads campaign insights"`                      | Meta Ads              |
| Backlinks, keywords, competitors    | `"ahrefs keywords backlinks"` / `"dataforseo serp"` | Ahrefs / DataForSEO   |
| Product funnels, events             | `"posthog funnel events"`                           | PostHog               |
| A/B experiments                     | `"mixpanel experiment report"`                      | Mixpanel              |
| LinkedIn / Instagram posts          | `"linkedin posts"` / `"instagram media insights"`   | LinkedIn / Instagram  |
| Create databases/pages              | `"notion create database page"`                     | Notion                |
| CRM contacts, deals                 | `"hubspot contacts deals"`                          | HubSpot               |

**4. Run.** Pick the tool by deliverable shape: `write_content` (copy — set `contentType`) ·
`create_report` (doc/PDF) · `create_presentation` (deck) · `create_web_page` (page) · `analyze_data`
(metrics + charts) · `create_campaign` (multi-asset launch) · `generate_image` (image) · `create_thread`
(full multi-part Flow / open-ended). Three ways to run:

- **Faithful (default):** fill the Flow's input prompt (`[BRACKET]`s → real values) and
  `create_thread({ prompt, projectId })` — Juma builds the whole multi-part deliverable.
- **Format-forced:** want one asset type → call the matching high-level tool above, folding the prompt
  into its `brief`.
- **Saved Flow:** `create_thread({ flow: "<name or id>", projectId })` — find it with `list_flows`.
  Juma applies the saved recipe; omit `prompt` to use the Flow's own. Only the caller's accessible
  Flows are listable/runnable.

Brief = the quality lever: be specific (subject, audience, questions, must-include facts) and **paste
the real data you pulled** — don't reference "the GA4 report". Pass `projectId`; add a `sections`
outline and `researchTheWeb: true` (on `create_report`) where useful.

Async: these return `{ threadId, status }`. Generate your own `threadId` (uuid) and pass it in. If
`running`, poll `read_conversation({ threadId })` every 20–30s until `latestTurnStatus` is `completed`
(read the message + `attachments`) or `failed`. Reuse `idempotencyKey` on a retry.

**A run often pauses as a plan — reply `run flow plan` to execute it.** An empty `completed` turn with
no attachments is a plan awaiting go-ahead, not a failure. Reply `run flow plan` (`reply_in_thread`),
answer any question it asks (e.g. output format), and keep going until the attachments come back.

**5. Deliver + save.** Surface the attachments (download + share links) + a 2–3 line summary.
`create_share_link({ threadId, fileUrl, scope })` (`team` default; images via `list_deliverables`).
Persist reusable artifacts with `add_knowledge_item`. Post/send via `run_integration_tool` only after
they confirm.

## Custom Flows

- **Ad-hoc** (job not listed): write a clear brief and `create_thread` — Juma plans, executes, returns
  the asset. The listed Flows are patterns, not limits.
- **Saved Flows:** discover with `list_flows`, run via the `flow` param (step 4).

## Routing table — 51 Flows

Each Flow's input prompt + deliverables live in its category file. `(app)` = the integration it works
best with.

**Getting started** → [`references/getting-started.md`](references/getting-started.md): Set up your
workspace · Explore some data · Analyze your competitors · Create visuals for your brand

**Strategy & Planning** → [`references/strategy-planning.md`](references/strategy-planning.md):
Brainstorm marketing campaigns · Plan marketing budget and allocation · Plan multi-platform paid media
launches · Build buyer personas · Define your ideal customer profile _(HubSpot)_

**Ads** → [`references/advertising.md`](references/advertising.md): Create Google search ads _(Google
Ads)_ · Create LinkedIn ad campaigns · Create Facebook ad campaigns _(Meta Ads)_ · Find where your Meta
Ads budget is leaking _(Meta Ads)_

**Analytics & Reporting** → [`references/analytics-reporting.md`](references/analytics-reporting.md):
Create a Google Ads performance report _(Google Ads)_ · Compare how your traffic channels convert
_(GA4)_ · Analyze marketing campaigns · Find where your Google Ads budget is leaking _(Google Ads)_ ·
Diagnose your activation funnel _(PostHog)_ · Analyze your conversion funnel _(GA4)_ · Generate a client
performance report _(GA4, Ads)_ · Run an A/B experiment in Mixpanel _(Mixpanel)_

**Content Creation** → [`references/content.md`](references/content.md): Build a landing page · Write a
press release · Write an onboarding email sequence

**Content Planning** → [`references/content.md`](references/content.md): Create a social media calendar
· Repurpose content across platforms · Build a Notion content calendar _(Notion)_

**SEO** → [`references/seo.md`](references/seo.md) _(Search Console / Ahrefs / DataForSEO)_: Compare your
pages against SEO competitors · Diagnose why a page lost traffic · Fix your click-through rates · Find
which content to refresh · Find SEO cannibalization issues · Audit your content for SEO · Build a
keyword gap analysis · Generate meta titles and descriptions with AI

**Social** → [`references/social.md`](references/social.md): Create Instagram content · Plan Instagram
campaigns · Analyze Instagram competitors _(Instagram)_ · Write LinkedIn posts · Extract a LinkedIn
voice profile · Analyze LinkedIn competitors · Reverse-engineer a LinkedIn content strategy · Plan
LinkedIn thought leadership · Audit a LinkedIn company page _(LinkedIn)_ · Generate YouTube video titles
· Write a YouTube video description · Vet creators for brand partnerships _(Instagram)_

**AI Search Optimization** →
[`references/ai-search-optimization.md`](references/ai-search-optimization.md): Run a GEO audit · Run an
AEO audit · Plan your AI search queries to target · Run a competitor content positioning audit

Also: [`references/research-briefing.md`](references/research-briefing.md) — cross-cutting research
building blocks (market/competitive research, audience briefs), used by several Flows.
