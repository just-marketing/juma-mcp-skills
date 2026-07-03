# Juma MCP Skills

The agent skill that ships with the **[Juma MCP](https://juma.ai/mcp)**. It turns any connected AI tool
(Claude, Cursor, ChatGPT, Gemini) into a marketing Flow operator — working on your team's brand,
knowledge, and connected apps, and handing back finished, on-brand deliverables.

## Get it

**1. Connect the Juma MCP → the skill comes with it.** No install: once connected, your agent can read
and run the marketing playbook over the connection. Connect in a couple of clicks (paste one URL, sign
in): **https://juma.ai/mcp**

**2. Optional — install as a native skill.** For a first-class Agent Skill in Claude Code / Claude
Desktop, drop the [`juma-mcp-marketing-flows/`](juma-mcp-marketing-flows) folder into your skills
location (e.g. `.claude/skills/`).

→ **Learn more & connect: https://juma.ai/mcp**

## What's inside

[`juma-mcp-marketing-flows/`](juma-mcp-marketing-flows) — `SKILL.md` (the operating guide: rules +
method) and `references/` (per-category Flow playbooks). Public docs only, no secrets.

---

<sub>**Maintainers (Juma team):** this repo is the source of truth the Juma MCP serves as resources. The
server fetches `SKILL.md` + `references/*` from the **`stable`** branch at runtime (cached ~10 min, with
a bundled fallback if unreachable). To ship a change: edit → PR → merge to `stable` → live within the
cache window. Keep it markdown-only and public.</sub>
