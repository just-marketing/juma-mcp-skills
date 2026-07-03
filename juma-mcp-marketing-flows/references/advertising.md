# Ads

Ad copy and campaigns across platforms. Ground in brand voice + the offer; pull account data where an
ad integration is connected so new copy replaces what's actually underperforming. See
[the method in SKILL.md](../SKILL.md). Fill each `[BRACKET]`, pass `projectId`, run, deliver + save.

---

## Create Google search ads

**Prompt:** `Our client is [CLIENT NAME] ([CLIENT WEBSITE URL]). They're running Google Search ads targeting [KEYWORDS, e.g. "LEGO sets for adults"]. Goal is [CONVERSION GOAL, e.g. online purchases].` `Pull their account data and write new RSAs — replace anything that's underperforming.`
**Best with:** Google Ads. **You'll get:** Responsive Search Ads — 12–15 headlines (≤30 chars), 4 descriptions (≤90 chars), pinned to benefits/keywords, plus callout/sitelink ideas.
**Run:** discover Google Ads via `search_integration_tools("google ads campaign performance")` →
`run_integration_tool` for current headline/keyword performance; then `create_thread({ prompt + data,
projectId })` or `write_content({ contentType: "Google Search ads (RSA)", brief, keywords, projectId })`.
Save winners as `TextExampleOutputs`.

## Create LinkedIn ad campaigns

**Prompt:** `Our client is [CLIENT NAME] ([CLIENT WEBSITE URL]). Targeting [AUDIENCE, e.g. office managers at mid-size companies] on LinkedIn, goal is [CONVERSION GOAL, e.g. demo requests]. Write LinkedIn ad copy variants to A/B test.`
**Best with:** webSearch. **You'll get:** 3–5 ad variants (intro ≤150 chars, headline ≤70, CTA) with a different hook each.
**Run:** `write_content({ contentType: "LinkedIn ad copy", brief, audience, projectId })`. Pull the
ICP/value props from knowledge first.

## Create Facebook ad campaigns

**Prompt:** `Generate Facebook ads for [CLIENT NAME] ([CLIENT WEBSITE URL]). They're [CAMPAIGN MOMENT, e.g. launching an outdoor furniture collection in April]. The audience is [AUDIENCE, e.g. homeowners aged 30-50].`
**Best with:** Meta Ads. **You'll get:** primary text + headline + description variants per angle, sized for the Meta feed.
**Run:** `write_content({ contentType: "Facebook/Meta ads", brief, audience, projectId })`; pair with
`generate_image({ projectId })` for creative. If Meta Ads is connected, pull past creative performance
to inform the angle.

## Find where your Meta Ads budget is leaking

**Prompt:** `Our client is [CLIENT NAME] ([CLIENT WEBSITE URL]). Run a Meta Ads audit for [TIME PERIOD, e.g. Q4 2025] and show us where budget was wasted. Target is [SUCCESS METRIC, e.g. CPA under $50].`
**Best with:** Meta Ads. **You'll get:** a ranked leak list (ad sets/creatives/audiences with high spend, low return), $ wasted, and specific pause/cut fixes.
**Run:** `search_integration_tools("meta ads campaign insights")` → `run_integration_tool` for the
period's ad-set/creative rows; then `analyze_data({ objective: <filled prompt>, data: <rows>, projectId })`.
(Google Ads version lives in [`analytics-reporting.md`](analytics-reporting.md).)
