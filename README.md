# SyncMetrics Marketing Analyst — Agent Skill

Teach your AI assistant to be an expert on your marketing data.

This skill makes any Claude (or other skill-capable agent) use the
[SyncMetrics](https://syncmetrics.io) MCP server **correctly on the first attempt**:
the right tool for each question, proper date-range comparisons, honest attribution
caveats, sample-size discipline, confirm-before-write safety, and decision-ready
report formatting — distilled from SyncMetrics' 106 tools and 88+ recipes.

**Without the skill:** the AI guesses tool order, mixes attribution models, and needs
several prompt iterations. **With it:** one prompt, first-attempt-correct analysis.

## Prerequisites

1. A [SyncMetrics](https://app.syncmetrics.io) account with at least one platform
   connected (Meta, Google Ads, TikTok, LinkedIn, GA4, Shopify).
2. The SyncMetrics connector added to your AI client —
   [setup guide](https://syncmetrics.io/docs/connect/). Connector URL:
   `https://mcp.syncmetrics.io/mcp` (OAuth sign-in, no tokens).

The skill is instructions only — it contains no credentials and makes no network calls
itself. Your data access still goes through your own SyncMetrics OAuth session.

## Install

### Claude (web / desktop)
1. Download [`skills/syncmetrics-marketing-analyst/SKILL.md`](skills/syncmetrics-marketing-analyst/SKILL.md).
2. In Claude: **Settings → Capabilities → Skills → Upload skill** and select the file.
3. Start a new chat and ask a marketing question — Claude applies the skill automatically.

### Claude Code (CLI)
```bash
git clone https://github.com/shamsuzzaman1/syncmetrics-skill
mkdir -p ~/.claude/skills
cp -r syncmetrics-skill/skills/syncmetrics-marketing-analyst ~/.claude/skills/
```
Restart Claude Code. Verify with: *"What does the syncmetrics-marketing-analyst skill do?"*

### Codex / Cursor / other agents
Copy the same folder into your agent's skills directory
(e.g. `~/.agents/skills/` for Codex, `~/.cursor/skills/` for Cursor), or paste the
SKILL.md body into your agent's custom instructions. Any agent that can call remote
MCP servers with OAuth can use SyncMetrics; the skill just makes it expert at it.

## Try it

> Give me a weekly performance summary across all my connected ad accounts, with
> what changed vs the previous week and what to do next.

> Where am I wasting spend on Meta and Google? Rank by amount and tell me what to pause.

> Forecast what 20 keywords around "running shoes" would cost me next month at a $1.50 max CPC.

More: [88+ ready-made recipes](https://syncmetrics.io/docs/recipes/).

## What's in the skill

- **Tool-selection map** — which of the 106 tools answers which question (cross-platform
  overview first, custom reports with metric discovery, planner tools, exports).
- **Analysis conventions** — explicit date windows, equal comparison periods, raw n next
  to every rate, LOW SAMPLE discipline, ad-platform vs GA4 vs Shopify attribution honesty.
- **Write safety** — Google Ads actions and Sheets writes always show a plan and get an
  explicit yes; new campaigns are created paused.
- **Output standard** — tables + deltas + 3–5 ranked recommendations, with shareable
  dashboard / Sheets export as the closing step.

## Also in this repo: Google Search Builder

[`skills/syncmetrics-google-search-builder`](skills/syncmetrics-google-search-builder/SKILL.md)
takes any business from a brief to running Google Search ads: keyword research with real
volumes and bids, an affordability check against your target cost per result, a campaign
plan (ad groups, exact/phrase keywords, responsive search ads, negatives, budgets), the
build in your own Google Ads account — always **paused** — conversion tracking, and a
weekly review that cuts wasted spend. Nothing is turned on until you say so.

Install it the same way as the analyst skill (upload its `SKILL.md`, or copy the folder
into your agent's skills directory), then try:

> Plan Google Search ads for my furniture store in Dhaka with 30,000 BDT a month.
> Show me the plan before you build anything.

## Links

- Docs: https://syncmetrics.io/docs/skills/
- Connect your AI client: https://syncmetrics.io/docs/connect/
- Tool catalog: https://syncmetrics.io/docs/tools/
- Support: https://syncmetrics.io/support/

## License

MIT — see [LICENSE](LICENSE).
