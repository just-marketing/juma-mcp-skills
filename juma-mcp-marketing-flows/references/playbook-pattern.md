# Playbook pattern — the shared engine

Every flow in this skill runs on these five steps. The category files assume you've read this and only
add per-flow specifics (inputs, the exact integration query, and the brief).

Tool names below are **bare** — prefix them with your client's Juma-MCP namespace
(`mcp__juma__…`, `mcp__claude_ai_Juma__…`, etc.).

---

## 1. Route

Match the ask to a flow in the SKILL.md routing table and open that category file. If the ask spans
several flows ("audit our SEO and rewrite the weak pages"), chain them: run the diagnostic flow first,
then feed its output into the production flow.

## 2. Ground — this is the differentiator, never skip it

The point of Juma is that the output is _the team's_, not generic. Grounding is what makes that true.

- **Resolve a project.** `list_projects` (optionally `search`) and pick the project that matches the
  client/brand/topic. Hold its `id` — you'll pass it as `projectId` to every generation tool so Juma
  applies that project's **brand profile** (colors, type, logo) **and** its **knowledge base**.
  - No matching project and the work is ongoing? Offer to `create_project` (confirm name + visibility)
    and, for a new client, seed it — `import_brand_profile_from_url` → review → `create_brand_profile`
    → `assign_brand_to_project`, plus `add_knowledge_item` for key facts. (See the workspace-setup
    flow in `strategy-planning.md` / `content.md` cross-refs.)
  - Truly one-off with no project? You can omit `projectId` (Juma uses the workspace default brand),
    but prefer a project whenever one fits.
- **Confirm the voice.** `get_brand` returns the resolved brand/voice for the current context — glance
  at it so your brief's `tone` doesn't fight the brand.
- **Read the knowledge.** `search_knowledge_items` (discovery) then `read_knowledge_items` /
  `get_knowledge` to pull the actual content — ICP notes, past briefs, positioning, examples. Fold the
  key facts into the brief; don't make Juma re-derive what the team already wrote down.

## 3. Pull live data via integrations

Most reporting/SEO/ads/social Flows need real numbers or competitor content. Juma reaches **3,000+
apps** — its first-party integrations (Google, Slack, HubSpot, GA4, Google/Meta Ads, Search Console,
Ahrefs, DataForSEO, LinkedIn, Instagram, PostHog, Mixpanel, Notion, …) **plus the entire Pipedream app
catalog** (Shopify, Salesforce, Airtable, Stripe, Jira, ClickUp, Mailchimp, Klaviyo, Webflow, Typeform,
Calendly, and thousands more). Pipedream apps come through the same MCP catalog, so almost any tool a
Flow needs can be part of it.

Two tools reach the ones the team has **already connected**:

- **Discover:** `search_integration_tools({ query: "<capability>" })` → exact tool `name`s, the owning
  app, and each input schema. The long tail isn't listed up front — always discover by capability first.
- **Execute:** `run_integration_tool({ name: "<exact name>", input: { … } })` — copy the `name`
  verbatim; OAuth + Pipedream auth are handled server-side.

**These search only ALREADY-CONNECTED integrations — they can't connect new accounts.** If the
capability you need returns nothing relevant, the app simply isn't connected yet. When that happens:

1. **Prompt the teammate to connect it.** Point them to **https://app.juma.ai/?integrations=all** and
   name the exact app to find and connect (e.g. "open Integrations, search HubSpot, and connect it").
   It's a click plus an OAuth sign-in; then re-run `search_integration_tools`.
2. **Keep moving meanwhile.** Fall back to `web_search` / `web_scrape`, or ask them to paste/upload an
   export (`upload_knowledge_file`), and **label anything estimated** until the real integration is on.

Don't silently substitute a guess for a missing integration — offer the connect link first; the live
account is the whole point of running the Flow through Juma.

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

`webSearch` / `web_scrape` are the workhorses — most Flows use them even when another integration is
connected, for current facts and public competitor content. YouTube-metadata Flows need no
integration; they research the video and category via the web.

## 4. Run the Juma primitive

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

**Two ways to run a Flow:**

- **Faithful (default).** Take the Flow's input prompt from its playbook, replace every `[BRACKET]`
  with the real client, URLs, period, and specifics, and `create_thread({ prompt, projectId })`. Juma
  plans and builds the whole multi-part deliverable the Flow is designed to produce (e.g. Instagram
  content → competitive analysis table + publish-ready posts + reel scripts + hashtags).
- **Format-forced.** When the teammate wants exactly one asset type, call the matching high-level tool
  (`create_report` doc, `create_presentation` deck, `write_content` copy, `create_web_page` page,
  `analyze_data` charts, `generate_image` image), folding the filled prompt into its `brief`. Same
  grounding, same async contract.
- **A saved Flow (`flow` param).** To run one of the team's own **saved** Flows, find it with
  `list_flows` and pass its **name or id** as `flow` to `create_thread`:
  `create_thread({ flow: "<name or id>", projectId })`. Juma applies the saved recipe itself —
  instructions, input ask-gates, phases, and example outputs — so omit `prompt` to use the Flow's own,
  or pass one to answer its inputs up front. Only the caller's accessible Flows are listable/runnable;
  `planned` Flows pause for `run flow plan`.

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

**Flows pause for plan approval — reply `run flow plan`.** A Flow run often comes back as a short or
**empty** assistant turn that is actually an **agent plan waiting for your go-ahead**, not the finished
work — an empty `completed` turn with no attachments is the tell (don't mistake it for a failed Flow).
To execute it, reply **`run flow plan`** in the thread (`reply_in_thread`), then poll again. The agent
may pause again between steps (e.g. after pulling data, before rendering) and may ask an explicit
question (like output format) — answer the question, then send `run flow plan` for the next step. Keep
going until the deliverable (attachments) comes back.

## 5. Deliver + save

- **Surface the assets.** Give the teammate the attachments — download links and share links — plus a
  2–3 line summary of what's inside. Don't bury the deliverable under process narration.
- **Share properly.** `create_share_link({ threadId, fileUrl, scope })` — `team` by default, `public`
  only when they ask for an external link. AI-generated images come back via `list_deliverables`.
- **Persist to the workspace.** For anything reusable — a research brief, an ICP, a keyword-gap sheet,
  a LinkedIn voice profile — save it with `add_knowledge_item` (`Text`, `Instruction`, or
  `TextExampleOutputs`) into the project so the team and future Juma runs inherit it. This is the
  "shared memory" advantage; use it.
- **Optionally distribute.** If they want it posted/sent, use the relevant integration
  (`run_integration_tool`, e.g. Slack) — after they confirm.

---

## Universal rules (inherited by every playbook)

- Ground first, generate second. `projectId` on every generation call.
- Real data only; label estimates and their source.
- Confirm before credit-heavy runs (`create_campaign`, long reports, many images) and before anything
  that posts externally.
- Keep the teammate in the loop for approvals, but don't ask permission for the read-only discovery
  steps (`list_*`, `get_*`, `search_*`, `read_*`).
