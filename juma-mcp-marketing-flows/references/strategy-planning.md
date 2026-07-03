# Strategy & Planning

Higher-altitude Flows: campaigns, budgets, personas, ICP. **Personas and ICP are foundational
artifacts** — build and save them first; the Ads, Content, Social, and AI-Search Flows all ground on
them. See [`playbook-pattern.md`](playbook-pattern.md). Fill `[BRACKETS]`, pass `projectId`, poll,
deliver + save.

---

## Brainstorm marketing campaigns

**Prompt:** `Our client is [CLIENT NAME] ([CLIENT URL]). They're launching [PRODUCT/INITIATIVE] targeting [AUDIENCE]. Goal is [GOAL, e.g. online purchases, demo requests]. Brainstorm campaign concepts and build an activation plan for the strongest one.`
**Best with:** webSearch. **You'll get:** several distinct campaign concepts (big idea, hook, channel, hero asset) + a full activation plan for the winner.
**Run (faithful):** `create_thread({ prompt, projectId })`; iterate with `reply_in_thread`. Once a
concept is picked, escalate to `create_campaign` to build the assets.

## Plan marketing budget and allocation

**Prompt:** `Our client is [CLIENT NAME] ([CLIENT URL]). Build a marketing budget plan for [TIME PERIOD] to support [GOAL/INITIATIVE]. Total budget is [AMOUNT], targeting [KPIs, e.g. 15,000 weekly visits and 5:1 ROAS].`
**Best with:** webSearch (add GA4/Ads efficiency from [`analytics-reporting.md`](analytics-reporting.md) if useful). **You'll get:** an allocation table ($ and % per channel) weighted by efficiency + strategic goals, with expected outcomes and reasoning.
**Run:** `analyze_data({ objective: <filled prompt>, data: <past performance if available>, projectId })`, or `create_report` for a leadership narrative. Save as the plan of record.

## Plan multi-platform paid media launches

**Prompt:** `Our client is [CLIENT NAME] ([CLIENT URL]). They're launching [PRODUCT/FEATURE] next quarter. Budget is [AMOUNT] for [DURATION] across [PLATFORMS, e.g. LinkedIn, Google Ads, and Meta]. Primary goal: [GOAL, e.g. qualified demo requests from IT directors].`
**Best with:** Google Ads, Meta Ads, webSearch. **You'll get:** a coordinated launch kit — copy per network + landing page + creative — plus the media plan.
**Run:** `create_campaign({ brief, channels: ["Google Ads","Meta Ads","LinkedIn Ads","landing page"], goal, audience, projectId })`. Most credit-heavy Flow — confirm first. Save the kit.

## Build buyer personas

**Prompt:** `Our client is [CLIENT NAME] ([CLIENT URL]). They're [CONTEXT, e.g. expanding into a new market / launching a new product line]. Build buyer personas for [TARGET MARKET].`
**Best with:** webSearch. **You'll get:** 2–4 personas — profile, goals, pains, buying triggers, objections, decision role, channels, and the messaging that lands.
**Run:** `create_report({ brief, researchTheWeb: true, sections: ["Persona 1","Persona 2","…","How to use these"], projectId })`. **Save each persona** with `add_knowledge_item` — foundational, don't leave it in a chat.

## Define your ideal customer profile

**Prompt:** `Our client is [CLIENT NAME] ([CLIENT URL]). Build an ICP for their [PRODUCT/SERVICE] — they need to focus [SALES/MARKETING] efforts on the companies most likely to [GOAL, e.g. adopt org-wide, convert to enterprise plan].`
**Best with:** webSearch, HubSpot. **You'll get:** firmographics, technographics, triggers, disqualifiers, and the value hypothesis.
**Run:** pull best-customer data from knowledge or HubSpot (`search_integration_tools("hubspot contacts deals")`), then `create_report({ brief, sections: ["ICP definition","Fit criteria","Disqualifiers","Where to find them"], projectId })`. **Save the ICP** with `add_knowledge_item` — it's the single most-referenced grounding artifact across this skill; build/refresh it before the persona, ad, content, and AI-Search Flows.

---

### Setting up a new client/brand project first?

If the project doesn't exist: `create_project` → `import_brand_profile_from_url` → review →
`create_brand_profile` → `assign_brand_to_project` → seed `add_knowledge_item`s (ICP, positioning,
proof). See the **Set up your workspace** Flow in [`getting-started.md`](getting-started.md).
