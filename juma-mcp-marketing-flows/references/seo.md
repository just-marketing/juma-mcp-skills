# SEO

SEO Flows need real ranking/traffic data — **Search Console** (your impressions, clicks, CTR, position)
and **Ahrefs / DataForSEO** (keywords, backlinks, competitor SERPs). Pull the data, then let Juma
diagnose or write it up on brand. See [the method in SKILL.md](../SKILL.md). Fill `[BRACKETS]`,
pass `projectId`, poll, deliver + save.

> Every SEO Flow works best with the SEO integrations connected. If they aren't, say which to connect
> (Search Console + Ahrefs or DataForSEO) and fall back to `web_scrape` for public SERP/competitor data,
> labelling anything estimated.

---

## Compare your pages against SEO competitors

**Prompt:** `Our client is [CLIENT NAME] ([CLIENT URL]). They used to rank well for "[KEYWORD]" but competitors now outrank them. Compare both pages and tell us what needs to change.`
**Best with:** Search Console, Ahrefs, DataForSEO. **You'll get:** a side-by-side gap analysis (depth, intent, backlinks, schema) and a prioritized fix plan.
**Run:** `create_report({ brief, researchTheWeb: true, sections: ["Where we stand","Gap analysis","Why they win","Fix plan"], projectId })`.

## Diagnose why a page lost traffic

**Prompt:** `Our client's page [PAGE URL] has been losing traffic over the last [TIME PERIOD, e.g. two months]. Diagnose what's happening and tell us what to fix.`
**Best with:** Search Console, Ahrefs, DataForSEO. **You'll get:** ranked causes (lost rankings / impressions / CTR / backlinks) with the fix.
**Run:** pull before/after Search Console + Ahrefs rows → `analyze_data({ objective, data, notes: "before/after windows", projectId })`. Chain a rewrite if the fix is content.

## Fix your click-through rates

**Prompt:** `Our client is [CLIENT NAME] ([CLIENT URL]). They rank for thousands of keywords but CTR is below average on most pages. Find the biggest CTR opportunities and fix the SERP presentation for the top 10.`
**Best with:** Search Console. **You'll get:** the top CTR-upside pages (high impressions, low CTR, good position) with rewritten titles + metas.
**Run:** pull query rows → `write_content({ contentType: "SEO title tags & meta descriptions", brief, projectId })`. Present as a before/after table ranked by impressions.

## Find which content to refresh

**Prompt:** `Our client is [CLIENT NAME] ([CLIENT URL]). They have [NUMBER]+ blog posts, some published over [TIME, e.g. two years] ago. Find which ones have lost ground and tell us what needs refreshing on each one.`
**Best with:** Search Console, Ahrefs. **You'll get:** a refresh priority list ranked by recovery potential, with the specific update each page needs.
**Run:** `create_report({ brief, sections: ["Refresh priority list","Per-page actions"], projectId })`. Save it as the content backlog.

## Find SEO cannibalization issues

**Prompt:** `Our client is [CLIENT NAME] ([CLIENT URL]). They have a large [CONTENT TYPE, e.g. blog] covering overlapping topics. Check if any pages are cannibalizing each other and tell us what to consolidate.`
**Best with:** Search Console. **You'll get:** cannibalization clusters (multiple URLs competing per query) + a keep/merge/redirect recommendation each.
**Run:** pull query→page rows → `analyze_data({ objective, data, projectId })`.

## Audit your content for SEO

**Prompt:** `Our client is [CLIENT NAME] ([CLIENT URL]). They have [NUMBER]+ blog posts but organic traffic concentrates on a handful of pages. Audit their content library and find the pages worth fixing first.`
**Best with:** Search Console, Ahrefs, DataForSEO. **You'll get:** a content inventory, thin/duplicate/decaying flags, keyword coverage vs opportunity, and a prioritized action plan.
**Run:** `create_report({ brief, researchTheWeb: true, sections: ["Summary","Content inventory","Issues","Opportunities","Action plan"], projectId })`.

## Build a keyword gap analysis

**Prompt:** `Our client is [CLIENT NAME] ([CLIENT URL]). Their main competitors are [COMPETITOR 1] ([URL]) and [COMPETITOR 2] ([URL]). Find the keywords they rank for that our client doesn't.`
**Best with:** Ahrefs, DataForSEO, Search Console. **You'll get:** the keyword-gap list, opportunity scoring by volume × difficulty, content recommendations, and a priority order by traffic potential. _(Name 2–3 competitors for best results.)_
**Run:** `create_report({ brief, sections: ["Top opportunities","By cluster","Recommended pages"], projectId })`. Hand the top clusters to the content team.

## Generate meta titles and descriptions with AI

**Prompt:** `Write 3 meta title and meta description variants for [PAGE URL] targeting the query "[TARGET QUERY, e.g. project management software]". Show the live SERP-winning competitor's snippet so I can see what each variant is competing against.`
**Best with:** Search Console, webSearch. **You'll get:** 3 title+meta variants per page, shown against the current SERP-winning snippet.
**Run:** `write_content({ contentType: "SEO title tags & meta descriptions", brief, projectId })` (or
`create_thread` to have Juma pull the live SERP first).
