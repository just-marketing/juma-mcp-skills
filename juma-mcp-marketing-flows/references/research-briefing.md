# Research & Briefing (cross-cutting building blocks)

> **Not a Flows category on juma.ai.** In the live Flow library, competitor research lives under
> **Getting started** ("Analyze your competitors") and **AI Search Optimization** ("Run a competitor
> content positioning audit"); audience work lives under **Strategy & Planning** (buyer personas, ICP).
> These two building blocks are kept here because many Flows depend on them — run and **save** them
> first, then the Ads/Content/Social/SEO Flows ground on the result.

See [`playbook-pattern.md`](playbook-pattern.md). Fill `[BRACKETS]`, pass `projectId`, poll, deliver + save.

---

## Market & competitive research

**Prompt:** `Our client is [CLIENT NAME] ([CLIENT URL]). Research the [MARKET/CATEGORY]. Answer: [QUESTIONS — sizing? competitors? trends? pricing?]. Profile these competitors: [LIST]. What does it mean for us?`
**Best with:** webSearch. **You'll get:** market overview, competitor profiles, trends, and implications — as a branded document.
**Run:** pull the team's existing notes with `search_knowledge_items` so Juma extends rather than
repeats; `web_scrape` specific competitor pages; then `create_report({ brief, researchTheWeb: true,
length: "in-depth", sections: ["Executive summary","Market overview","Competitor profiles","Trends","Implications for us"], projectId })`. **Save it** — reused by every campaign, brief, and persona.

## Campaign Audience Brief

**Prompt:** `Build a Campaign Audience Brief for [CAMPAIGN/PRODUCT], goal [GOAL], targeting [SEGMENT(S)]. Ground it in our ICP/personas.`
**Best with:** webSearch. **You'll get:** audience snapshot, jobs & pains, messaging angles, objections & proof, and channel recommendations.
**Run:** read the ICP/personas from knowledge (`search_knowledge_items` → `read_knowledge_items`), then
`create_report({ brief, sections: ["Audience snapshot","Jobs & pains","Messaging angles","Objections & proof","Channels"], projectId })`. **Save it** and hand it to the production Flows (ad copy, landing page, social) so they all pull the same audience definition.

---

Need the reusable audience artifacts these briefs cite (buyer personas, ICP)? Build them first via
[`strategy-planning.md`](strategy-planning.md).
