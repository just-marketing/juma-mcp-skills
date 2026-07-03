# Getting started

The onboarding Flows — the first things a new team runs. Great for standing up a workspace and proving
value fast. See [the method in SKILL.md](../SKILL.md) for the shared steps; fill each prompt's
`[BRACKETS]`, pass `projectId` when one exists, run, then deliver + save.

---

## Set up your workspace

**Prompt:** `We're [COMPANY NAME], a [WHAT YOU DO, e.g. digital agency working with e-commerce brands]. Help me set up projects for our team.`
**Best with:** webSearch. **You'll get:** a proposed project structure (one per client/workstream), created in the workspace.
**Run:** `create_thread({ prompt, projectId? })`; Juma proposes projects and creates them with
`create_project` (confirm before creating). For a new client brand, chain: `import_brand_profile_from_url`
→ review → `create_brand_profile` → `assign_brand_to_project`, then seed `add_knowledge_item`s.

## Explore some data

**Prompt:** `Here's [DESCRIPTION OF DATA, e.g. our sales report from last quarter]. Help me understand what's going on.`
**Best with:** none (bring the data). **You'll get:** computed metrics, charts, and the key findings.
**Run:** `analyze_data({ objective: <the prompt>, dataUrl | data, projectId })` — paste small data
inline or pass a `dataUrl` (public URL / Juma `/s/` link) for anything large.

## Analyze your competitors

**Prompt:** `Our client is [COMPANY NAME] ([COMPANY URL]). We need a competitive analysis.`
**Best with:** webSearch. **You'll get:** per-competitor profiles, positioning/messaging breakdown, and gaps to exploit.
**Run:** `create_report({ brief: <filled prompt + which competitors to include>, researchTheWeb: true,
sections: ["Landscape","Competitor profiles","Messaging gaps","Where we can win"], projectId })`. Save
the report to the project — later Strategy/SEO/AI-Search Flows reuse it.

## Create visuals for your brand

**Prompt:** `Our client is [COMPANY NAME] ([COMPANY URL]). Create a few image options for [USE CASE, e.g. social media, a campaign, a presentation].`
**Best with:** webSearch. **You'll get:** several on-brand image options.
**Run:** `generate_image({ prompt: <on-brand visual brief>, orientation, projectId })` — passing
`projectId` applies the project's brand (colors, type). Generate 2–4 variants; surface via
`list_deliverables`.
