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

The operating guide for using Juma well from a connected AI tool (Claude, Claude Code, Cursor, ChatGPT
— anything on the **Juma MCP**). This skill doesn't add tools; it teaches you to reproduce Juma's
**Flows** — the press-play marketing workflows at [juma.ai/flows](https://juma.ai/flows) — by composing
the Juma MCP primitives + the team's connected integrations, _on the team's brand, grounded in their
knowledge, saved back to the shared workspace._ The routing table lists the common Flows, but **the
rules and method below apply to any Juma marketing task — listed, saved, or ad-hoc.**

> **What a Flow is:** name your client + a few inputs → Juma researches (integrations + web), plans,
> and builds a finished deliverable (report, deck, calendar, page, image, scorecard). Your job is to
> map the ask to the right approach, fill the prompt, run it through the MCP, and deliver the result.

> **The model can already write and build. What it can't do alone is know your brand, read your
> knowledge base, or reach your GA4 and ad accounts.** Every task leads with grounding and live data,
> then hands finished work back to the workspace.

## Prerequisite

The team's **Juma MCP** must be connected (`https://mcp.juma.ai/mcp`). Tool names are prefixed by the
client — `mcp__juma__create_report` in Claude Code, `mcp__claude_ai_Juma__create_report` on claude.ai.
This skill uses the **bare names** (`create_thread`, `create_report`, `run_integration_tool`, …); call
them with whatever prefix your client assigns.

## Rules — apply to every Juma task

These hold whether you're running a listed Flow, a saved Flow, or an ad-hoc ask. Internalize them; the
method below is how you execute them.

- **Ground before you generate.** Resolve a `projectId` and read the relevant knowledge first, then
  pass `projectId` to the run so Juma applies the team's brand + knowledge. An ungrounded deliverable
  is the failure mode this skill exists to prevent.
- **Never invent metrics.** Numbers come from an integration, an upload, or the web — labelled with
  their source. No data? Say so and offer to connect the integration.
- **Prompt to connect missing integrations.** A task can use almost any of the **3,000+ apps** Juma
  reaches — first-party integrations _and_ the full **Pipedream** catalog — but
  `search_integration_tools` finds only already-connected ones. If you need one that isn't connected,
  send the teammate to **https://app.juma.ai/?integrations=all** to connect it (name the exact app),
  then re-run — don't silently fall back to a guess.
- **Respect credits.** Generation runs cost credits; `create_campaign` / multi-asset launches are the
  most expensive. Confirm before large runs and before anything that posts externally.
- **Don't gate the read-only steps.** `list_*` / `get_*` / `search_*` / `read_*` are safe discovery —
  run them freely; reserve confirmation for writes, generation, credit-heavy runs, and external posts.
- **Save the outcome.** Persist reusable artifacts (ICP, personas, voice profiles, briefs, gap sheets)
  with `add_knowledge_item` so the team and future runs inherit them — the shared-memory advantage.
- **Delegate the deep work to Juma.** Fill the prompt and let the Juma agent research + build; it
  already carries the platform Flow skills. Your job is routing, grounding, live data, delivery.

## The method — five steps, every task

Listed Flow or not, work runs the same five steps. This is the engine; the per-category files in the
routing table only add each Flow's exact input prompt, integrations, and deliverables.

### 1. Route

Match the ask to a Flow in the routing table below and open its category file. If the ask spans several
Flows ("audit our SEO and rewrite the weak pages"), chain them: run the diagnostic Flow first, then feed
its output into the production Flow. No match at all? Run it as a **custom Flow** (see the section
below) — never refuse.

### 2. Ground — the differentiator, never skip it

The point of Juma is that the output is _the team's_, not generic. Grounding is what makes that true.

- **Resolve a project.** `list_projects` (optionally `search`) and pick the project that matches the
  client/brand/topic. Hold its `id` — you'll pass it as `projectId` to every generation tool so Juma
  applies that project's **brand profile** (colors, type, logo) **and** its **knowledge base**.
  - No matching project and the work is ongoing? Offer to `create_project` (confirm name + visibility)
    and, for a new client, seed it — `import_brand_profile_from_url` → review → `create_brand_profile`
    → `assign_brand_to_project`, plus `add_knowledge_item` for key facts. (See the workspace-setup
    Flow in `strategy-planning.md` / `content.md` cross-refs.)
  - Truly one-off with no project? You can omit `projectId` (Juma uses the workspace default brand),
    but prefer a project whenever one fits.
- **Confirm the voice.** `get_brand` returns the resolved brand/voice for the current context — glance
  at it so your brief's `tone` doesn't fight the brand.
- **Read the knowledge.** `search_knowledge_items` (discovery) then `read_knowledge_items` /
  `get_knowledge` to pull the actual content — ICP notes, past briefs, positioning, examples. Fold the
  key facts into the brief; don't make Juma re-derive what the team already wrote down.

### 3. Pull live data via integrations

Most reporting/SEO/ads/social work needs real numbers or competitor content. Juma reaches **3,000+
apps** — its first-party integrations (Google, Slack, HubSpot, GA4, Google/Meta Ads, Search Console,
Ahrefs, DataForSEO, LinkedIn, Instagram, PostHog, Mixpanel, Notion, …) **plus the entire Pipedream app
catalog** (Shopify, Salesforce, Airtable, Stripe, Jira, ClickUp, Mailchimp, Klaviyo, Webflow, Typeform,
Calendly, and thousands more). Pipedream apps come through the same MCP catalog, so almost any tool you
need can be part of it.

Two tools reach the ones the team has **already connected**:

- **Discover:** `search_integration_tools({ query: "<capability>" })` → exact tool `name`s, the owning
  app, and each input schema. The long tail isn't listed up front — always discover by capability first.
- **Execute:** `run_integration_tool({ name: "<exact name>", input: { … } })` — copy the `name`
  verbatim; OAuth + Pipedream auth are handled server-side.

**These search only ALREADY-CONNECTED integrations — they can't connect new accounts.** If the
capability you need returns nothing relevant, the app simply isn't connected yet: point the teammate to
**https://app.juma.ai/?integrations=all** to connect it, then re-run `search_integration_tools`. Keep
moving meanwhile with `web_search` / `web_scrape` or a pasted export (`upload_knowledge_file`), and
**label anything estimated**. Don't silently substitute a guess for a missing integration — the live
account is the whole point of running the work through Juma.

Common discovery queries:

| Need                                        | `search_integration_tools` query                    | Typical app           |
| ------------------------------------------- | --------------------------------------------------- | --------------------- |
| Sessions, conversions, channels             | `"google analytics ga4 report"`                     | Google Analytics      |
| Impressions, clicks, CTR, position          | `"search console query performance"`                | Google Search Console |
| Ad spend, campaigns, keywords               | `"google ads campaign performance"`                 | Google Ads            |
| Paid social performance                     | `"meta ads campaign insights"`                      | Meta Ads              |
| Backlinks, keywords, competitors            | `"ahrefs keywords backlinks"` / `"dataforseo serp"` | Ahrefs / DataForSEO   |
| Product funnels, events, retention          | `"posthog funnel events"`                           | PostHog               |
| A/B experiments, event analytics            | `"mixpanel experiment report"`                      | Mixpanel              |
| Company/profile posts                       | `"linkedin posts profile"`                          | LinkedIn              |
| Instagram media & insights                  | `"instagram media insights"`                        | Instagram             |
| Create databases/pages (calendars, portals) | `"notion create database page"`                     | Notion                |
| CRM contacts, deals                         | `"hubspot contacts deals"`                          | HubSpot               |
| Post the result                             | `"slack send message"`                              | Slack                 |

`webSearch` / `web_scrape` are the workhorses — most work uses them even when another integration is
connected, for current facts and public competitor content.

### 4. Run the Flow

Pick the tool by the _shape of the deliverable_, not the topic:

| You want…                                                  | Tool                                |
| ---------------------------------------------------------- | ----------------------------------- |
| Copy/text (posts, ads, emails, threads)                    | `write_content` (set `contentType`) |
| A written document / analysis write-up / brief (PDF)       | `create_report`                     |
| A slide deck                                               | `create_presentation`               |
| A rendered web/landing page                                | `create_web_page`                   |
| Metrics + charts computed from a dataset                   | `analyze_data`                      |
| A multi-asset campaign (page + emails + social + creative) | `create_campaign`                   |
| A single image                                             | `generate_image`                    |
| The full multi-part Flow deliverable / open-ended work     | `create_thread`                     |

**Three ways to run a Flow:**

- **Faithful (default).** Take the Flow's input prompt from its playbook, replace every `[BRACKET]`
  with the real client, URLs, period, and specifics, and `create_thread({ prompt, projectId })`. Juma
  plans and builds the whole multi-part deliverable the Flow is designed to produce.
- **Format-forced.** When the teammate wants exactly one asset type, call the matching high-level tool
  (`create_report`, `create_presentation`, `write_content`, `create_web_page`, `analyze_data`,
  `generate_image`), folding the filled prompt into its `brief`. Same grounding, same async contract.
- **A saved Flow (`flow` param).** To run one of the team's own **saved** Flows, find it with
  `list_flows` and pass its **name or id** as `flow` to `create_thread`:
  `create_thread({ flow: "<name or id>", projectId })`. Juma applies the saved recipe itself —
  instructions, input ask-gates, phases, and example outputs — so omit `prompt` to use the Flow's own,
  or pass one to answer its inputs up front. Only the caller's accessible Flows are listable/runnable.

Writing the brief (the biggest quality lever):

- Be specific: subject, audience, the questions to answer, the angle, must-include facts, and any
  numbers/quotes you pulled in step 3. Paste the real data — don't reference "the GA4 report".
- Pass **`projectId`** (from step 2). For reports/pages, pass a `sections` outline when you know the
  shape. Set `researchTheWeb: true` on `create_report` for anything needing current external facts.
- Set `audience`, `tone`, `length`/`slideCount` to match the brand and the deliverable.

The async contract (all `create_*` / `write_content` / `analyze_data` tools):

- They return `{ threadId, status, … }`. **Generate your own `threadId` (a uuid) and pass it in** so
  you can track the job even if the call times out.
- If `status: "completed"`, read the reply + `attachments` (the finished files/share links) now.
- If `status: "running"`, poll `read_conversation({ threadId })` every **20–30s** until
  `latestTurnStatus` is `completed` (read the newest assistant message + attachments) or `failed`.
- On a retry of the _same_ call, reuse the same `idempotencyKey` so Juma resumes instead of duplicating.

**Flows pause for plan approval — reply `run flow plan`.** A run often comes back as a short or
**empty** assistant turn that is actually an **agent plan waiting for your go-ahead**, not the finished
work — an empty `completed` turn with no attachments is the tell (don't mistake it for a failed Flow).
To execute it, reply **`run flow plan`** in the thread (`reply_in_thread`), then poll again. The agent
may pause again between steps (e.g. after pulling data, before rendering) and may ask an explicit
question (like output format) — answer it, then send `run flow plan` for the next step. Keep going
until the deliverable (attachments) comes back.

### 5. Deliver + save

- **Surface the assets.** Give the teammate the attachments — download and share links — plus a 2–3
  line summary of what's inside. Don't bury the deliverable under process narration.
- **Share properly.** `create_share_link({ threadId, fileUrl, scope })` — `team` by default, `public`
  only when they ask for an external link. AI-generated images come back via `list_deliverables`.
- **Persist to the workspace.** For anything reusable — a research brief, an ICP, a keyword-gap sheet,
  a LinkedIn voice profile — save it with `add_knowledge_item` (`Text`, `Instruction`, or
  `TextExampleOutputs`) into the project so the team and future Juma runs inherit it.
- **Optionally distribute.** If they want it posted/sent, use the relevant integration
  (`run_integration_tool`, e.g. Slack) — after they confirm.

## Custom Flows — the long tail and the team's saved Flows

Two kinds of custom Flow, beyond the ~51 headline ones this skill details:

- **Ad-hoc** — the ask is a marketing job not listed. Write a clear brief (client + URL, inputs, the
  deliverable you want) and `create_thread` with it — Juma turns it into a step-by-step plan, executes
  it, and returns the asset. Treat the listed Flows as patterns, not limits.
- **The team's _saved_ Flows** — teammates save a chat as a reusable Flow (its own prompt, recipe,
  inputs, phases, and example outputs). Discover them with **`list_flows`** and run one via the `flow`
  param on `create_thread` (see step 4's "A saved Flow"). Only Flows the caller can access (their own +
  those pinned to their projects) are listed/runnable.

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

## Reference files

The per-category playbooks (each Flow's exact input prompt + deliverables):

- [`references/getting-started.md`](references/getting-started.md)
- [`references/strategy-planning.md`](references/strategy-planning.md)
- [`references/advertising.md`](references/advertising.md) — Ads
- [`references/analytics-reporting.md`](references/analytics-reporting.md)
- [`references/content.md`](references/content.md) — Content Creation + Content Planning
- [`references/seo.md`](references/seo.md)
- [`references/social.md`](references/social.md)
- [`references/ai-search-optimization.md`](references/ai-search-optimization.md)
- [`references/research-briefing.md`](references/research-briefing.md) — cross-cutting research building blocks (not a Flows category)
