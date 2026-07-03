# Social

Instagram, LinkedIn, and YouTube Flows — two sub-types: **produce** content (backed by engagement data) and **analyze** competitors/strategy.

---

## Create Instagram content

**Prompt:** `Our client is [CLIENT NAME] ([CLIENT URL]). We need [NUMBER] Instagram posts for [PRODUCT/LINE]. Mix of carousels, single images, and reels.`
**Best with:** Instagram, webSearch. **You'll get:** a competitive analysis table (engagement/formats/themes), publish-ready posts with captions + hashtag sets + visual direction, and carousel/reel breakdowns.
**Run (faithful):** `create_thread({ prompt, projectId })` pulls live engagement data; add visuals with `generate_image({ orientation: "square"|"vertical", projectId })`.

## Plan Instagram campaigns

**Prompt:** `[CLIENT NAME] ([CLIENT URL]) is launching [PRODUCT/EVENT] in [TIMEFRAME]. Build the Instagram campaign.`
**Best with:** Instagram, webSearch. **You'll get:** campaign concept, content pillars, a post calendar with formats/hooks, and hashtag strategy.
**Run:** `create_thread({ prompt, projectId })` or `create_report` for the plan doc; `create_campaign({ channels: ["Instagram"] })` when they want the assets built too.

## Analyze Instagram competitors

**Prompt:** `Our client is [CLIENT NAME] ([CLIENT URL]). Analyze their Instagram and their top competitors. What formats get engagement, what topics perform, and what are people saying in the comments?`
**Best with:** Instagram, webSearch. **You'll get:** a competitor teardown (formats, topics, comment sentiment) + concrete moves for us.
**Run:** `create_report({ brief, sections: ["What they do","What works","Our opportunities"], projectId })`.

## Write LinkedIn posts

**Prompt:** `We need [NUMBER] LinkedIn posts for [CLIENT NAME] ([LINKEDIN URL]). Mix of thought leadership, educational, and storytelling posts.`
**Best with:** LinkedIn, webSearch. **You'll get:** posts across the three styles, each with a strong first-line hook and soft CTA.
**Run:** `write_content({ contentType: "LinkedIn post", brief, projectId })`. If a **voice profile** exists in knowledge, read it and pass it into the brief.

## Extract a LinkedIn voice profile

**Prompt:** `Our client is [ROLE] at [COMPANY NAME]. Build a LinkedIn voice profile from their posting history: [LINKEDIN PROFILE URL]`
**Best with:** LinkedIn, webSearch. **You'll get:** a reusable voice profile (tone, rhythm, formatting habits, themes, hook patterns, do/don't).
**Run:** `create_report({ brief, sections: ["Voice summary","Do","Don't","Hook patterns","Example openers"], projectId })`. **Save it** with `add_knowledge_item({ contentType: "Instruction", name: "<Person> LinkedIn voice profile" })` — every later "Write LinkedIn posts" run grounds on it.

## Analyze LinkedIn competitors

**Prompt:** `Our client is [CLIENT NAME] ([LINKEDIN URL]). Analyze their LinkedIn against their top competitors. What's working, what's different, and where are the gaps?`
**Best with:** LinkedIn, webSearch. **You'll get:** a landscape teardown + differentiation angles to own.
**Run:** `create_report({ brief, sections: ["Landscape","Patterns","Gaps we can own"], projectId })`.

## Reverse-engineer a LinkedIn content strategy

**Prompt:** `Reverse-engineer [COMPANY NAME]'s LinkedIn content strategy ([LINKEDIN URL]). What do they post about, what works, and what are they missing?`
**Best with:** LinkedIn, webSearch. **You'll get:** the decoded strategy (topic mix, formats, cadence, hook archetypes) as a replicable playbook.
**Run:** `create_report({ brief, projectId })` (or `analyze_data` if engagement numbers are available).

## Plan LinkedIn thought leadership

**Prompt:** `Our client is [ROLE] at [COMPANY NAME] ([LINKEDIN URL]). Build a LinkedIn thought leadership plan for [TIME PERIOD], with positioning, topic pillars, and the first month of posts.`
**Best with:** LinkedIn, webSearch. **You'll get:** positioning, 4–6 content pillars, a post calendar, and month-one post ideas.
**Run:** `create_report({ brief, sections: ["Positioning","Pillars","Calendar","Month-one posts"], projectId })`; feed the ideas into "Write LinkedIn posts".

## Audit a LinkedIn company page

**Prompt:** `Audit our client's LinkedIn company page: [CLIENT NAME] ([LINKEDIN URL]). What's working, what's falling flat, and what should they change first?`
**Best with:** LinkedIn, webSearch. **You'll get:** a snapshot, what's working/failing, and a prioritized improvement list.
**Run:** `create_report({ brief, sections: ["Snapshot","What's working","Issues","Action plan"], projectId })`.

## Generate YouTube video titles

**Prompt:** `Write 10 YouTube title variants for [CHANNEL OR CLIENT NAME]'s new video: [YOUTUBE URL]. The audience is [AUDIENCE, e.g. product builders and ops teams] and the video's goal is [GOAL, e.g. drive signups].`
**Best with:** webSearch (no integration needed). **You'll get:** 10 title variants across hook styles, tuned for CTR.
**Run:** `write_content({ contentType: "YouTube titles", brief, projectId })`.

## Write a YouTube video description

**Prompt:** `Write a YouTube description for our client [CLIENT NAME]'s new video: [YOUTUBE URL]`
**Best with:** webSearch. **You'll get:** an SEO-aware description with hook, summary, timestamps/links placeholders, and CTA.
**Run:** `write_content({ contentType: "YouTube description", brief, projectId })`.

## Vet creators for brand partnerships

**Prompt:** `Our client [CLIENT NAME] ([CLIENT WEBSITE URL]) is shortlisting these creators for a [CAMPAIGN, e.g. spring home decor] campaign: [@HANDLE1], [@HANDLE2], [@HANDLE3]. Vet them for engagement reality, audience signals from comments, and brand-safety risks.`
**Best with:** Instagram, webSearch. **You'll get:** a single scorecard — one row per creator with a go/no-go flag, engagement-vs-follower reality, audience signals, brand-safety flags, and fit. _(Vet 3–5 per run.)_
**Run:** `create_thread({ prompt, projectId })` — pulls comment/engagement signals for the scorecard.
