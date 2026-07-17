---
source_title: "🧳 Product Manager ToolKit"
source_page_id: 365dd50dc1fc808cb3bee2b44c26d200
source_url: https://app.notion.com/p/365dd50dc1fc808cb3bee2b44c26d200
parent_page: "Tool Recommendations & Demos (27edd50dc1fc8029a48cd45be132ca4d)"
last_edited: 2026-06-28T20:58:00Z
extracted_at: 2026-07-17T22:20:00Z
extraction_method: Notion MCP page fetch (verbatim)
sections_dropped:
  - "Inline database views (2) — the page fetch returns these as <database> placeholders with no rows. Rows are NOT part of a page export; they require a separate data-source query and are supplied as round2/tools-technologies-masterlist.md. See MANIFEST.md."
---

# 🧳 Product Manager ToolKit

Fractional doesn't require a specific toolkit — but when we surveyed our PMs about what they actually use, a clear default stack emerged. This page **is** that stack: tiered so you know what to set up first, and pulled live from the [Tools & Technologies masterlist](https://app.notion.com/p/f04dd50dc1fc83aca93b8104ae10fab8) (filtered to the PM picks).

> 🧭 **New to the team? Start with the Core Kit.** Set up those tools first — they cover the vast majority of the job. Layer in Recommended Additions as you settle in, and reach for Situational tools when a specific project or client calls for them.

## How this page works

Every tool is tiered:

- **🔵 Core Kit** — the default stack. If you're new, install and connect these first.
- **🟢 Recommended Addition** — strong upgrades once the basics are in place.
- **🟣 Situational** — client-driven or personal-preference tools; adopt when the engagement calls for it.

The two tables below split **tools** (apps you run) from **Claude Skills** (repeatable workflows you've taught Claude), but both use the same tiers.

---

## 🔵 The Core PM Stack

Six things cover the core of the role at Fractional — these are the **Core Kit** tier in the table below:

- **Project management → Linear** — the internal standard (every PM surveyed uses it). Its roadmap view doubles as lightweight roadmapping. Use Jira only when a client mandates it.
- **Docs & knowledge → Notion** — our internal documentation home (this page lives here). Several PMs are actively migrating Google Docs into Notion.
- **Productivity & analysis → Google Workspace** — Docs, Sheets, Gmail, Calendar. Sheets/Excel is the de-facto roadmapping and analysis surface.
- **Communication → Slack** — the team backbone.
- **AI assistant → Claude (Cowork + Code)** — the one tool *every* PM said they couldn't do their job without.
- **Meeting notes → Granola** — automatic capture and summaries for calls.

## 🧰 All PM Tools

Every tool our PMs use, grouped by tier. Filter by **Category** (project management, documentation, diagramming, roadmapping, notetaking, communication…) to find the right tool for a specific job. *Claude Skills are listed separately below.*

[inline database: Tools & Technologies, filtered to PM picks — rows not included in page export]

## 🤝 Working with clients

Our internal default is **Linear + Notion**. On client engagements you'll often inherit *their* stack instead — that's expected:

- **Jira** — client-mandated issue tracking (the team prefers Linear).
- **Confluence** — client / external-facing documentation.
- **Microsoft Office / OneNote** — when the client is a Microsoft shop.

Treat these as **Situational**: use them where the engagement requires, but they don't replace your internal default.

## 🤖 Claude Skills for PMs

Beyond off-the-shelf tools, our PMs are teaching Claude repeatable workflows. These are custom skills people have built for themselves — and the best ones are strong candidates to share and standardize across the team.

> ⭐ **Most-requested:** a **Deck / Presentation Builder** was built independently by four different PMs. That's a clear signal we should converge on one shared, well-maintained version rather than ten private ones.

[inline database: Tools & Technologies, filtered to Claude Skills — rows not included in page export]

### 📥 Using & sharing skills

**Use a shared skill** — in Claude **Cowork**: Customize → Skills → Browse Skills → **Shared** tab → install. This is where org-published skills live (e.g. the recruiting interview-scheduler skills).

**Browse the Claude Code library** — [`plugin-marketplace`](https://github.com/fractional-ai/plugin-marketplace): run `/plugin marketplace add fractional-ai/plugin-marketplace`, then `/plugin`. Includes `fractional-slides`, `fractional-pdf`, `fractional-brand`, `daily-brief`, `linear-hygiene`, and more.

**Share a PM skill you built** — the home for PM skills is [`fractional-pm-skills`](https://github.com/fractional-ai/fractional-pm-skills). Install the whole library in Claude Code with `/plugin marketplace add fractional-ai/fractional-pm-skills` (or clone + `./install.sh`). To contribute one: copy the `_template/` folder, drop in your `SKILL.md`, and open a PR. To share a personal skill *fast* without a PR, run [`agent-gather`](https://github.com/fractional-ai/agent-gather)'s `./sync.sh` — good ones get promoted into the repo. Either way, also publish it to the Cowork **Shared** tab so PMs who live in Cowork can install it.

> 🌱 Several skills above are still private/homegrown (see their notes). If you built one, please add it to [`fractional-pm-skills`](https://github.com/fractional-ai/fractional-pm-skills) — the consolidated PM skills library we're building so we stop maintaining ten private versions of the same thing.

---

## ➕ Contributing

This kit stays useful only if we keep it current. To add or update something:

1. Open the [Tools & Technologies masterlist](https://app.notion.com/p/f04dd50dc1fc83aca93b8104ae10fab8).
2. Add a new row — or edit the existing one. **Don't create duplicates**; a tool shared with engineering should be a single row with both an `Engineering Labels` and a `PM Labels` tier.
3. Set the **Category**, set **Type** (`Tool` or `Claude Skill`), and pick a **PM Labels** tier: `Core Kit`, `Recommended Addition`, or `Situational`.
4. Tag yourself in **Team Contacts** so colleagues know who to ask.
