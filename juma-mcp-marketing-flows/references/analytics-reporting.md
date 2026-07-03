# Analytics & Reporting

Data-first Flows: **pull real numbers from an integration, then hand them to Juma.** Never let the
model estimate metrics. `analyze_data` computes + charts + explains; `create_report` produces a
stakeholder-ready document. See [`playbook-pattern.md`](playbook-pattern.md). Fill `[BRACKETS]`, pass
`projectId`, paste the pulled rows into the prompt/brief, poll, deliver + save.

Shared move: `search_integration_tools("<capability>")` → `run_integration_tool` to fetch rows → pass
them as `data` (inline) or `dataUrl` (large) to the tool below.

---

## Create a Google Ads performance report

**Prompt:** `Our client is [CLIENT NAME] ([CLIENT WEBSITE URL]). Create a Google Ads performance report for [MONTH YEAR] to share with [AUDIENCE, e.g. the marketing director].`
**Best with:** Google Ads. **You'll get:** spend/conversions/CPA/ROAS, top & worst campaigns, MoM change, recommendations — as a branded PDF/deck.
**Run:** `create_report({ brief, sections: ["Executive summary","Spend & efficiency","Top performers","Underperformers","MoM trend","Recommendations"], projectId })`. Save so next month compares to it.

## Compare how your traffic channels convert

**Prompt:** `Our website is [WEBSITE URL]. Compare how [CHANNEL 1, e.g. paid search] traffic converts versus [CHANNEL 2, e.g. organic search]. Break down the funnel by acquisition channel and show where each one behaves differently.`
**Best with:** GA4, Google Ads, Search Console. **You'll get:** a channel comparison by conversion rate & value/session, plus a reallocation recommendation.
**Run:** `analyze_data({ objective: <filled prompt>, data: <rows>, projectId })`.

## Analyze marketing campaigns

**Prompt:** `Our client is [CLIENT NAME] ([CLIENT WEBSITE URL]). Analyze their [PLATFORM, e.g. Google Ads] campaign performance for [TIME PERIOD, e.g. the last 30 days].`
**Best with:** Google Ads, Meta Ads. **You'll get:** which campaigns drove efficient conversions vs wasted spend, ranked, with 3 actions.
**Run:** `analyze_data({ objective, data: <rows>, notes, projectId })`.

## Find where your Google Ads budget is leaking

**Prompt:** `Our client is [CLIENT NAME] ([CLIENT WEBSITE URL]). Find where their Google Ads budget is leaking over [TIME PERIOD, e.g. the last 90 days].`
**Best with:** Google Ads. **You'll get:** a ranked leak list (keywords/campaigns/search terms — high spend, low/no conversions), $ wasted, and pauses/negatives/bid cuts.
**Run:** `analyze_data({ objective, data: <spend/clicks/conv/search-term rows>, projectId })`. Offer to draft the negative-keyword list next.

## Diagnose your activation funnel

**Prompt:** `Our product is [PRODUCT NAME] ([PRODUCT URL]). Diagnose the activation funnel — from signup through onboarding to [ACTIVATION EVENT, e.g. first project created].`
**Best with:** PostHog, GA4. **You'll get:** step-by-step drop-off, the biggest leak with likely causes, and 3 experiments.
**Run:** `search_integration_tools("posthog funnel events")` → rows → `analyze_data({ objective, data, projectId })`.

## Analyze your conversion funnel

**Prompt:** `Our website is [WEBSITE URL]. Analyze the conversion funnel from [START, e.g. homepage visit] to [END, e.g. purchase]. Show where visitors drop off and what to fix.`
**Best with:** GA4, PostHog. **You'll get:** stage-by-stage conversion vs prior period + the highest-impact fix.
**Run:** `analyze_data({ objective, data: <funnel rows>, projectId })`.

## Generate a client performance report

**Prompt:** `Pull Google Ads, Meta Ads, and GA4 for [CLIENT NAME] ([CLIENT WEBSITE URL]) into a [MONTH YEAR] report.`
**Best with:** GA4, Google Ads, Meta Ads. **You'll get:** cover KPIs, executive summary, per-platform scorecards (Google + Meta), GA4 traffic/landing detail, benchmark comparison, and a prioritized 5-item action plan.
**Run:** the multi-source Flow — `create_thread({ prompt, projectId })` so Juma reads all three
integrations in one pass and builds the report (add "build the slide deck" to the prompt for a deck).
Save to the project.

## Run an A/B experiment in Mixpanel

**Prompt:** `Set up an A/B experiment in our Mixpanel to test whether [CHANGE, e.g. a new onboarding flow] lifts [PRIMARY METRIC, e.g. activation]. Recommend the metrics and sample size, and draft it for us to review before launch.`
**Best with:** Mixpanel. **You'll get:** the experiment design (hypothesis, primary/guardrail metrics, sample size), drafted for review before launch.
**Run:** `search_integration_tools("mixpanel experiment")` to see available actions → `create_thread({ prompt, projectId })`; confirm before anything writes to Mixpanel.
