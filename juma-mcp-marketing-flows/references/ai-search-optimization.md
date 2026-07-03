# AI Search Optimization

Win citations in AI answers (ChatGPT, Perplexity, Google AI Overviews) and answer-engine features
(featured snippets, People Also Ask). Juma has dedicated tools here: **`run_geo_audit`** and
**`check_geo_visibility`**, plus the deep **`geo-audit`** skill. See
[the method in SKILL.md](../SKILL.md). Fill `[BRACKETS]`, pass `projectId`, poll, deliver + save.

---

## Run a generative engine optimization (GEO) audit

**Prompt:** `[BRAND NAME] ([PAGE URL]) needs a generative engine optimization audit. Score the page across the 10 GEO citation dimensions, flag the biggest gaps against the [CATEGORY, e.g. project management] category, and give me prioritized rewrites.`
**Best with:** webSearch. **You'll get:** a 10-dimension GEO score with band, the biggest gaps, and prioritized, paste-ready rewrites — as a branded HTML/PDF report.
**Run:** `run_geo_audit({ url, brand_name })` for the scored `AuditResult`, then render it (or invoke the
`geo-audit` skill for the full report). Save the score to the project to track over time.

## Run an answer engine optimization (AEO) audit

**Prompt:** `Run an AEO audit on [PAGE URL] for the [CATEGORY, e.g. project management] category. Show where featured snippets and PAA boxes are going to competitors, and rewrite the answer blocks for the top three pages we're losing.`
**Best with:** webSearch. **You'll get:** a snippet-gap table (which query triggers a snippet, who wins it, where you rank), paste-ready answer-block rewrites for the top losing pages, and FAQPage/HowTo schema. _(Name the exact buyer queries, not just the topic.)_
**Run (faithful):** `create_thread({ prompt, projectId })` so Juma checks the live SERP features and
writes the rewrites; `check_geo_visibility` can score a page's citability alongside.

## Plan your AI search queries to target

**Prompt:** `[BRAND] ([BRAND WEBSITE URL]) wants to win AI search citations in the [CATEGORY] category. What AI search queries should we target, ranked by opportunity and how hard each one is to win?`
**Best with:** webSearch. **You'll get:** a ranked target-query list (opportunity × difficulty) with the angle to win each.
**Run:** `create_report({ brief, researchTheWeb: true, sections: ["Target queries","Opportunity vs difficulty","How to win each"], projectId })`.

## Run a competitor content positioning audit

**Prompt:** `Our client competes with [COMPETITOR] ([COMPETITOR WEBSITE URL]) in [CATEGORY, e.g. CRM software]. Show how ChatGPT and Perplexity describe [COMPETITOR], where that narrative is beatable, and what content we should write to take the topic.`
**Best with:** webSearch. **You'll get:** how the AI engines currently describe the competitor, the beatable angles, and a content plan to take the topic.
**Run:** `create_thread({ prompt, projectId })` (Juma queries the AI engines and analyzes the narrative)
or `create_report({ researchTheWeb: true })`. Save the content plan to the project.
