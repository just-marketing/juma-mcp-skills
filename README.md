# Juma MCP Skills

Public source of truth for the agent **skills** that the [Juma MCP](https://mcp.juma.ai/mcp) serves to
connected AI tools (Claude, Cursor, ChatGPT, Gemini, …). Edit here → it goes live on the MCP without a
code deploy.

Currently ships one skill:

```
juma-mcp-marketing-flows/
├── SKILL.md              # entry point — routing + method
└── references/           # per-category Flow playbooks
    ├── getting-started.md
    ├── strategy-planning.md
    ├── advertising.md
    ├── analytics-reporting.md
    ├── content.md
    ├── seo.md
    ├── social.md
    ├── ai-search-optimization.md
    └── research-briefing.md
```

## How it reaches the live MCP

The Juma MCP server exposes this skill as **MCP resources** (`skill://juma-mcp-marketing-flows/SKILL.md`
+ `references/*`, index at `.../_manifest`). It fetches the content from **this repo's raw URLs at
runtime**, caches it (~10 min), and falls back to a bundled snapshot if this repo is ever unreachable —
so an outage here never breaks the MCP.

The server is pointed here by one env var (set at deploy):

```
JUMA_MARKETING_SKILL_SOURCE_URL=https://raw.githubusercontent.com/just-marketing/juma-mcp-skills/stable/juma-mcp-marketing-flows
```

It fetches `${JUMA_MARKETING_SKILL_SOURCE_URL}/SKILL.md` and `${…}/references/<file>.md`.

## Editing workflow ("update on the go")

The server reads from the **`stable`** branch (a promote-able ref), not `main` — so edits aren't live
until you promote them:

1. Edit on a branch (or `main`) and open a PR.
2. Review the change (protect `stable` so nothing ships unreviewed — a bad edit is instantly live to
   every connected agent otherwise).
3. Merge/fast-forward into **`stable`**.
4. Live within the cache TTL (~10 min).

> **Point the server at `main` instead** if you'd rather every edit be immediately live with no promote
> step — just change the ref in `JUMA_MARKETING_SKILL_SOURCE_URL`. `stable` is the safer default.

## Keep it public & clean

- **Public docs only.** This is product-usage guidance — no secrets, keys, or internal-only content.
- **Markdown only.** `SKILL.md` + `references/*.md`. The MCP serves them verbatim as `text/markdown`.
- **Don't rename files** without updating the server's expected paths (the bundled fallback's file list
  is the canonical set).

## Optional: also ship as a Claude Code plugin

The same repo can double as a **Claude Code plugin marketplace** so Claude Desktop / Claude Code users
get a one-click "connect MCP + install skill" (`/plugin marketplace add just-marketing/juma-mcp-skills`).
That needs a `.claude-plugin/marketplace.json` + a plugin definition bundling this skill and the MCP
server config — see the current Claude Code plugin docs (https://code.claude.com/docs/en/skills and
anthropics/claude-plugins-official) for the exact schema before adding it. Not required for the
MCP-resource delivery above; it's a second, Claude-only distribution channel.
