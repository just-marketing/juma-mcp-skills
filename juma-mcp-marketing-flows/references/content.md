# Content Creation & Content Planning

Two Flow categories. **Content Creation** produces individual assets (pages, releases, email
sequences); **Content Planning** organizes and repurposes content across platforms. On-brand voice +
the team's knowledge do the heavy lifting — ground before you generate. See
[the method in SKILL.md](../SKILL.md). Fill `[BRACKETS]`, pass `projectId`, poll, deliver + save.

---

## Content Creation

### Build a landing page

**Prompt:** `Our client is [CLIENT NAME] ([CLIENT WEBSITE URL]). They're running a [CAMPAIGN TYPE] campaign targeting [AUDIENCE]. The goal is [CONVERSION GOAL, e.g. demo requests, purchases, free trial signups].` `Build a landing page.`
**Best with:** webSearch. **You'll get:** a rendered, on-brand page — hero, benefits, proof, FAQ, CTA — as a hosted share link + file.
**Run:** `create_web_page({ brief, goal, callToAction, sections: ["hero","benefits","how it works","social proof","FAQ","CTA"], projectId })`. Copy-only? `write_content({ contentType: "landing page copy" })`.

### Write a press release

**Prompt:** `[CLIENT NAME] ([CLIENT WEBSITE URL]) is announcing [THE NEWS, e.g. a new recycled-down jacket collection] next month. Write the press release.`
**Best with:** webSearch. **You'll get:** a standard-format release — dateline, headline, subhead, quotes, boilerplate.
**Run:** `write_content({ contentType: "press release", brief, projectId })` (or `create_report` for a formatted PDF).

### Write an onboarding email sequence

**Prompt:** `Create an onboarding email sequence for new signups to [PRODUCT NAME] ([PRODUCT URL]). The activation moment is [FIRST KEY ACTION, e.g. create a first page].`
**Best with:** webSearch. **You'll get:** a multi-email sequence driving to the activation moment — subject lines, body copy, CTAs, and send timing.
**Run:** `write_content({ contentType: "onboarding email sequence", brief, projectId })`. Save the sequence to the project for reuse.

---

## Content Planning

### Create a social media calendar

**Prompt:** `Our client is [CLIENT NAME] ([CLIENT WEBSITE URL]). Create a social media calendar for [MONTH] covering [PLATFORMS, e.g. LinkedIn, X, and Instagram].`
**Best with:** webSearch. **You'll get:** a calendar (date · platform · pillar · hook · format · CTA per slot), balanced across pillars and built around key dates.
**Run:** `create_report({ brief, sections: ["Overview & pillars","Week-by-week calendar","Hooks bank"], projectId })`. Save as the working plan; each row can be produced with `write_content`/`generate_image`.

### Repurpose content across platforms

**Prompt:** `Our client is [CLIENT NAME]. Here's a [CONTENT TYPE, e.g. blog post, webinar, case study] they published: [URL OR PASTE CONTENT]` `Repurpose it into publish-ready content for [PLATFORMS, e.g. LinkedIn, X, and email].`
**Best with:** webSearch. **You'll get:** one publish-ready asset per platform, adapted (not truncated) to each format.
**Run:** `web_scrape` the source if it's a URL, then `write_content({ contentType: "multi-platform repurpose pack", brief, projectId })`.

### Build a Notion content calendar

**Prompt:** `Build [BRAND NAME] ([BRAND URL]) a [TIME PERIOD, e.g. 30-day] content calendar across [PLATFORMS, e.g. LinkedIn, Instagram, and TikTok]. Deploy it as a Notion database with columns for date, platform, content pillar, post copy, status, and assignee.`
**Best with:** Notion. **You'll get:** a live Notion database (date · platform · pillar · post copy · status · assignee), populated.
**Run:** `create_thread({ prompt, projectId })` so Juma builds the plan and deploys it via the Notion
integration; discover the action with `search_integration_tools("notion create database page")`. Confirm before it writes to Notion.
