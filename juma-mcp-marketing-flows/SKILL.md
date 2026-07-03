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

Turn a connected AI tool (Claude, Claude Code, Cursor, ChatGPT — anything on the **Juma MCP**) into a
Juma Flow operator. This skill doesn't add tools; it teaches you to reproduce Juma's **Flows** — the
press-play marketing workflows at [juma.ai/flows](https://juma.ai/flows) — by composing the Juma MCP
primitives + the team's connected integrations, _on the team's brand, grounded in their knowledge,
saved back to the shared workspace._

> **What a Flow is:** name your client + a few inputs → Juma researches (integrations + web), plans,
> and builds a finished deliverable (report, deck, calendar, page, image, scorecard). This skill maps
> the ask to the right Flow, fills its input prompt, runs it through the MCP, and delivers the result.

> **The model can already write and build. What it can't do alone is know your brand, read your
> knowledge base, or reach your GA4 and ad accounts.** Every Flow below leads with grounding and live
> data, then hands finished work back to the workspace.

## Prerequisite

The team's **Juma MCP** must be connected (`https://mcp.juma.ai/mcp`). Tool names are prefixed by the
client — `mcp__juma__create_report` in Claude Code, `mcp__claude_ai_Juma__create_report` on claude.ai.
This skill uses the **bare names** (`create_thread`, `create_report`, `run_integration_tool`, …); call
them with whatever prefix your client assigns.

## The universal method

Every Flow runs the same five steps. **Read [`references/playbook-pattern.md`](references/playbook-pattern.md) once** — the shared engine
(grounding, integration discovery, the async polling contract, delivery, and the two ways to run a
Flow). The category files only add each Flow's exact input prompt, integrations, and deliverables.

1. **Route** — match the ask to a Flow below and open its playbook. No match? Run it as a **custom
   Flow** (see below) — never refuse.
2. **Ground** — resolve the `projectId` (so Juma applies that project's **brand + knowledge**), read
   the relevant knowledge, check the brand with `get_brand`. Never skip this.
3. **Pull live data** — discover the right integration with `search_integration_tools`, run it with
   `run_integration_tool`. Juma reaches **3,000+ apps** (first-party + Pipedream), but only
   **already-connected** ones are searchable. If the one you need isn't connected, point the teammate
   to **https://app.juma.ai/?integrations=all** to connect it, then fall back to `web_search` / a
   pasted export meanwhile (label estimates).
4. **Run the Flow** — fill the Flow's input prompt (replace every `[BRACKET]`), then either
   `create_thread` with that prompt (faithful — Juma plans + builds the full multi-part deliverable)
   or the matching high-level tool (`create_report`, `write_content`, …) when you want one specific
   output format. Pass `projectId`. These are **async**: generate a `threadId`, poll `read_conversation`
   every 20–30s until complete. If the run pauses with an agent **plan** (often an empty `completed`
   turn), reply **`run flow plan`** to execute it — then keep polling.
5. **Deliver + save** — surface the files/share links, `create_share_link` for the team, and persist
   reusable outputs (personas, ICP, voice profiles, briefs) with `add_knowledge_item`.

## Custom Flows — the long tail and the team's saved Flows

Two kinds of custom Flow, beyond the ~51 headline ones this skill details:

- **Ad-hoc** — the ask is a marketing job not listed. Write a clear brief (client + URL, inputs, the
  deliverable you want) and `create_thread` with it — Juma turns it into a step-by-step plan, executes
  it, and returns the asset. Treat the listed Flows as patterns, not limits.
- **The team's _saved_ Flows** — teammates save a chat as a reusable Flow (with its own prompt, recipe,
  inputs, phases, and example outputs). Discover them with **`list_flows`**, then run one by passing its
  **name or id** as the **`flow`** param to `create_thread` — e.g.
  `create_thread({ flow: "Weekly SEO recap", projectId })`. Juma applies the saved recipe automatically;
  omit `prompt` to use the Flow's own, or pass one to answer the Flow's inputs up front. `planned` Flows
  pause for `run flow plan`. Only Flows the caller can access (their own + those pinned to their
  projects) are listed/runnable.

## Routing table — the 51 Flows by category

Each Flow's exact input prompt + "you'll get" live in its playbook file. Integrations shown are what
each Flow works best with (discover them via `search_integration_tools`). Prefix tools per your client.

### Getting started → [`references/getting-started.md`](references/getting-started.md)

Set up your workspace · Explore some data · Analyze your competitors · Create visuals for your brand

### Strategy & Planning → [`references/strategy-planning.md`](references/strategy-planning.md)

Brainstorm marketing campaigns · Plan marketing budget and allocation · Plan multi-platform paid media
launches _(Google Ads, Meta Ads)_ · Build buyer personas · Define your ideal customer profile _(HubSpot)_

### Ads → [`references/advertising.md`](references/advertising.md)

Create Google search ads _(Google Ads)_ · Create LinkedIn ad campaigns · Create Facebook ad campaigns
_(Meta Ads)_ · Find where your Meta Ads budget is leaking _(Meta Ads)_

### Analytics & Reporting → [`references/analytics-reporting.md`](references/analytics-reporting.md)

Create a Google Ads performance report _(Google Ads)_ · Compare how your traffic channels convert
_(GA4, Search Console)_ · Analyze marketing campaigns _(Google/Meta Ads)_ · Find where your Google Ads
budget is leaking _(Google Ads)_ · Diagnose your activation funnel _(PostHog, GA4)_ · Analyze your
conversion funnel _(GA4, PostHog)_ · Generate a client performance report _(GA4, Google Ads, Meta Ads)_
· Run an A/B experiment in Mixpanel _(Mixpanel)_

### Content Creation → [`references/content.md`](references/content.md)

Build a landing page · Write a press release · Write an onboarding email sequence

### Content Planning → [`references/content.md`](references/content.md)

Create a social media calendar · Repurpose content across platforms · Build a Notion content calendar
_(Notion)_

### SEO → [`references/seo.md`](references/seo.md)

Compare your pages against SEO competitors · Diagnose why a page lost traffic · Fix your click-through
rates · Find which content to refresh · Find SEO cannibalization issues · Audit your content for SEO ·
Build a keyword gap analysis · Generate meta titles and descriptions with AI _(all: Search Console /
Ahrefs / DataForSEO)_

### Social → [`references/social.md`](references/social.md)

Create Instagram content _(Instagram)_ · Plan Instagram campaigns _(Instagram)_ · Analyze Instagram
competitors _(Instagram)_ · Write LinkedIn posts _(LinkedIn)_ · Extract a LinkedIn voice profile
_(LinkedIn)_ · Analyze LinkedIn competitors _(LinkedIn)_ · Reverse-engineer a LinkedIn content strategy
_(LinkedIn)_ · Plan LinkedIn thought leadership _(LinkedIn)_ · Audit a LinkedIn company page _(LinkedIn)_
· Generate YouTube video titles · Write a YouTube video description · Vet creators for brand
partnerships _(Instagram)_

### AI Search Optimization → [`references/ai-search-optimization.md`](references/ai-search-optimization.md)

Run a generative engine optimization (GEO) audit · Run an answer engine optimization (AEO) audit ·
Plan your AI search queries to target · Run a competitor content positioning audit

## Hard rules

- **Ground before you generate.** Resolve a `projectId` and read the relevant knowledge first, then
  pass `projectId` to the run — an ungrounded deliverable is the failure mode this skill prevents.
- **Never invent metrics.** Numbers come from an integration, an upload, or the web — labelled with
  their source. No data? Say so and offer to connect the integration.
- **Prompt to connect missing integrations.** A Flow can use almost any of the **3,000+ apps** Juma
  reaches — first-party integrations _and_ the full **Pipedream** catalog — but
  `search_integration_tools` finds only already-connected ones. If a Flow needs one that isn't
  connected, send the teammate to **https://app.juma.ai/?integrations=all** to connect it (name the
  exact app), then re-run — don't silently fall back to a guess.
- **Respect credits.** Flow runs cost credits; `create_campaign` / multi-asset launches are the most
  expensive. Confirm before large runs and before anything that posts externally.
- **Save the outcome.** Persist reusable artifacts (ICP, personas, voice profiles, briefs, gap sheets)
  with `add_knowledge_item` so the team and future runs inherit them — the shared-memory advantage.
- **Delegate the deep work to Juma.** Fill the Flow prompt and let the Juma agent research + build; it
  already carries the platform Flow skills. Your job is routing, grounding, live data, delivery.

## Reference files

- [`references/playbook-pattern.md`](references/playbook-pattern.md) — the shared engine. **Read first.**
- [`references/getting-started.md`](references/getting-started.md)
- [`references/strategy-planning.md`](references/strategy-planning.md)
- [`references/advertising.md`](references/advertising.md) — Ads
- [`references/analytics-reporting.md`](references/analytics-reporting.md)
- [`references/content.md`](references/content.md) — Content Creation + Content Planning
- [`references/seo.md`](references/seo.md)
- [`references/social.md`](references/social.md)
- [`references/ai-search-optimization.md`](references/ai-search-optimization.md)
- [`references/research-briefing.md`](references/research-briefing.md) — cross-cutting research building blocks (not a Flows category)
