# Onboarding Institutional Memory Agent — Eng 1 Plan

**Date:** 2026-07-17
**Author:** Yury Kamen (Eng 1)
**Aligns to:** [Build Spec — Onboarding Institutional Memory Agent](https://app.notion.com/p/3a0dd50dc1fc80358b41d6c72740566d) (`3a0dd50dc1fc80358b41d6c72740566d`)
**Base repo:** `rosscrooke/institutional-memory` (upstream) · `shirlsgaw/institutional-memory` (origin, our fork)
**Scope:** Eng 1 owns real data substitution into `round1/`/`round2/`, seeding the demo project roster, verifying the contradiction surfaces in the regenerated doc, and running both sessions.
**Status:** Pair locked. Corpus 50% extracted. Roster + persona map open.

> **Related, not this:** [`2026-07-17-pm-onboarding-design.md`](./2026-07-17-pm-onboarding-design.md) specs the **real-ops FDPM onboarding program** sponsored by Alexandra. It is **parked** — the Build Spec puts advisor/HR surfaces explicitly out of scope and is pull-only. Kept because it remains valid for the real FDPM hire. Do not confuse the two.

---

## 1. The locked pair

**Round 1 = Product Manager ToolKit page. Round 2 = the Tools & Technologies masterlist it claims to be pulled from.**

The contradiction: **the ToolKit page's prose disagrees with its own cited source, and nothing marks it.**

| Page's "Core PM Stack" (six) | DB `PM Labels = Core Kit` (six) |
|---|---|
| Linear ✅ | Linear |
| Notion ✅ | Notion |
| Google Workspace ✅ | Google Workspace |
| Slack ✅ | Slack |
| **Claude (Cowork + Code)** — no row in the masterlist | **GitHub** |
| **Granola** — no row in the masterlist | **Deck / Presentation Builder** |

The page asserts Claude is *"the one tool every PM said they couldn't do their job without"* while saying it is *"pulled live from the Tools & Technologies masterlist (filtered to the PM picks)"* — a masterlist containing **no Claude row and no Granola row**.

Secondary, same corpus: **10 rows added 2026-06-28/29 carry no `PM Labels`**, so they are invisible to the ToolKit's PM-filtered views — including **`ask-fdpm`**, a Claude Skill named for the role being onboarded, and `salma-feedback-skill` (Salma owns FDPM Training Materials). Every PM-tiered row predates 2026-06-08. **The ToolKit has been frozen for five weeks while its source grew by 10.**

### 1.1 Why not Deep Atlas

Deep Atlas is real, dated and load-bearing — but **empirically incompatible with an AFDPM/FDPM persona.** The `FDPM Training Materials` DB (`37add50d-c1fc-800e-91a9-000b85066712`) is 16 hand-picked rows:

> Claude 101 · AI Fluency · Intro to MCP · Claude with Bedrock · Claude with Vertex · Building with the Claude API · Mastering Evals as a PM · LLM Evals · A Field Guide to Rapidly Improving AI Products · Eval Driven System Design · Using MCPs with evals · Working Backwards · Product Discovery Basics · Escaping the Build Trap · The Mom Test

**Zero mentions of Deep Atlas. Zero of Anthropic Skilljar.** `"Different set for FDPM"` understates it — the sets are *disjoint*. The engineering-curriculum contradiction cannot appear in a PM's generated doc; round 2 would produce a zero delta. Deep Atlas requires an **Engineer** persona.

### 1.2 Why not a synthesized tier change

The Build Spec's preferred route — *"a tool moving tiers or getting added"* — cannot be evidenced:
- Notion MCP exposes **no page history**; today's tiers are readable, yesterday's are not.
- There is **one** tools DB, not two. `339dd50d…` is titled *"View of Tools & Technologies"* and points at the same `collection://ebbdd50d-c1fc-83f2-b92e-87db83a71f52`. No duplicate to diff.
- Reconstructing round 1 by `createdTime` yields a **zero delta**, because every row added since 2026-06-08 is untiered and therefore filtered out of the artifact we would diff.

So the ToolKit route needs invented data. Ruled out: the demo does not need invented data.

### 1.3 Honest caveat

This pair is **two live sources disagreeing**, not a temporal change. The Build Spec says round 2 is *"a genuinely changed version of one of the above."* This is a different animal. It is arguably a stronger institutional-memory story — the agent reconciles two live, unmarked, disagreeing sources — but it is **not what the spec described**, and the readout should say so rather than blur it.

### 1.4 Also ruled out: Engineer persona

Not merely a persona swap. **No row in the entire masterlist carries an `Engineering Labels` value.** An Engineer's tool-access checklist would render empty. Neither spec anticipated this.

---

## 2. Corpus

### 2.1 Written

| File | Source | Provenance |
|---|---|---|
| `synthetic-data/round1/pm-toolkit.md` | Product Manager ToolKit (`365dd50dc1fc808cb3bee2b44c26d200`), last edited 2026-06-28 | Verbatim page export. Front-matter records the dropped inline-DB placeholders. |
| `synthetic-data/round2/tools-technologies-masterlist.md` | Tools & Technologies (`f04dd50dc1fc83aca93b8104ae10fab8` / `collection://ebbdd50d…`) | Full 40-row SQL export, `has_more: false`. Non-tier columns dropped for context economy. |

**Extraction discipline:** verbatim only; trims are whole-section deletions logged in front-matter; never edit content to sharpen the contradiction.

**The prose/rows split is honest, not manufactured.** The Notion MCP page fetch returns inline databases as `<database>` placeholders with **no rows** — rows require a separate data-source query. Round 1 (page export) and round 2 (DB export) are therefore naturally distinct artifacts, which is exactly why the drift went unnoticed by humans too.

### 2.2 Outstanding

| Need (per Build Spec round 1) | Status |
|---|---|
| Current onboarding docs | Not extracted |
| **Persona map — who to meet, by role and chapter** | **Does not exist as a page.** Must be composed. See §4. |
| One training module | `FDPM Training Materials` DB captured (16 rows); not yet written to `round1/` |
| Product Manager ToolKit | ✅ done |

### 2.3 Legacy synthetic files

`round1/{team-directory,access-policy,onboarding-handbook}.md` and `round2/{team-directory-update,policy-update-2026-05-15}.md` still exist. **Decide:** archive to `synthetic-data/_synthetic-original/` (keep as fallback) before the freeze. Scripts glob `*.md`, so leaving them in place **pollutes both rounds.** This is a live bug, not a nit.

---

## 3. Integration with Shirley's web UI

**Source:** [PR #9 `pivot/onboarding-web`](https://github.com/rosscrooke/institutional-memory/pull/9) — shirlsgaw → rosscrooke, OPEN, 9 files, +1783.
Deployed: `https://web-six-khaki-38.vercel.app` (401 to us — see §3.4).

### 3.1 What it is

A **static HTML directory behind a Google SSO edge gate**, Ode-branded from ode.com's live stylesheet (bone `#fbfbf8`, cola `#3c2e2a`, dove `#625855`, tomato `#cd3e1d`, tea, lilac; ABC Diatype named-not-bundled; derived dark palette).

| File | Role |
|---|---|
| `web/public/index.html` | Front door. *"Welcome to Ode."* → *"Your first week, one page at a time. We'll show you who to meet and what to read — **and when something changes, we'll tell you**."* |
| `web/public/chapter.html` | *"Know your chapter."* Chapter 1 (Joshua Marker fCTO · Eva Morgenstein FDPM Lead, `#chapter_1`); Chapter 2 (Ben Greenberg fCTO · Alexandra Spencer-Wong FDPM Lead, `#chapter_2`). Has *"Quiz — generated from memory"*, a `qstate` chip (*"Session 1 · initial ingest"*), and a memory-file panel marked *"illustrative placeholder data."* |
| `web/public/docs.html` | *"Orientation docs."* The engine: `who` identity input, doc list, reader, **"Your receipts"**, **"Two or three questions"** (quiz), **"Admin (demo)"**, `data-bump` to simulate a change. |
| `web/public/people.html` | *"Who to meet."* |
| `web/middleware.js` | Vercel Edge Middleware: Google ID-token verification, `ALLOWED_DOMAINS=fractional.ai,ode.com`, HMAC `fai_session` cookie, 12h TTL, fail-closed without `SESSION_SECRET`. |

**Her chapter data matches Notion exactly** — Ben Greenberg and Alexandra Spencer-Wong are the verified Chapter 2 leads. Her UI is built on real org data.

### 3.2 The seam: `seed-content.json`

`docs.html` ends with:

```js
fetch('seed-content.json', { cache: 'no-store' })
  .then(res => { if (!res.ok) throw new Error('http ' + res.status); return res.json(); })
  .then(raw => boot(normalize(raw) || PLACEHOLDER))
  .catch(() => boot(PLACEHOLDER));
```

And `web/public/.gitignore` is exactly:

```
seed-content.json
whoisthis.html
```

**Both are gitignored — they are generated artifacts, not source.** `whoisthis.html` is not a broken nav link; it is produced locally. **`seed-content.json` is an unfilled socket, and our agent is the plug.** No redesign, no negotiation over her CSS: we emit a file she already fetches.

Today the socket is empty, so her UI boots `PLACEHOLDER` and self-labels *"Docs — placeholder set (seed content not loaded)"* / *"PLACEHOLDER CONTENT — not a real Ode doc."*

### 3.3 The contract (from `normalize()`)

```json
{
  "docs": [{
    "id":        "pm-toolkit",
    "title":     "Product Manager ToolKit",
    "version":   "<hash>",
    "updated":   "2026-06-28",
    "body":      "<text, \\n\\n paragraphs>",
    "key_facts": ["...", "..."],
    "delta":     { ... },
    "quiz": [{ "q": "...", "options": ["...","..."], "answer": 0 }]
  }]
}
```

Accepted aliases: list key `docs|documents|pages` or a bare array · `body|content|text` · `quiz|questions` · `q|question` · `options|choices` · `answer|answer_index` · `id|doc_id` · `title|name` · `version|version_hash` · `updated|updated_at|last_edited` · `delta|supersession|supersession_delta`.

### 3.4 ⚠️ Integration landmines — silent failure by design

`normalize()` **drops bad data without erroring**, and the UI falls back to placeholder. Every one of these fails *silently on stage*:

| Rule in her code | Consequence if we violate it |
|---|---|
| `if (!String(body).trim() \|\| clean.length < 2) continue;` | **A doc with an empty body or fewer than 2 valid quiz questions is dropped entirely.** |
| quiz item needs `q`, `options` array ≥2, numeric `answer` in range | Malformed question silently discarded — which can push a doc under the 2-question floor and drop the whole doc. |
| `quiz: clean.slice(0, 3)` | **Max 3 questions render per doc.** Emitting 20 wastes tokens and hides 17. |
| `if (!out.length) return null;` → `boot(PLACEHOLDER)` | **If every doc fails validation, the UI shows placeholder content with no error.** The demo dies looking like it worked. |

**Mitigation (Eng 1, before the freeze):** validate `seed-content.json` against these rules locally and assert `placeholder === false` after boot. **Never demo without confirming the `src-label` does not read *"placeholder set."*** This is the single highest-risk item in the integration.

### 3.5 Fitting Pair A into a doc-shaped schema

Her schema is doc-shaped (`title`/`body`/`key_facts`/`quiz`/`delta`); our contradiction is a **tool checklist diff**. There is no tools page in her nav. It fits anyway, with **zero UI change**:

- **Round 1** → one doc, `id: pm-toolkit`, `title: "Product Manager ToolKit"`, `body` = the Core PM Stack prose, `key_facts` = the six named tools, `updated: 2026-06-28`, `quiz` = 2–3 tier questions.
- **Round 2** → same `id`, new `version`, `body` regenerated from the masterlist, and **`delta`** naming the reconciliation: Claude and Granola are absent from the cited source; GitHub and Deck/Presentation Builder are the actual Core Kit; 10 tools added since 6/28 are untiered and invisible.

Her UI then renders our contradiction through copy **she already wrote**: *"Something you read has changed"* and *"What changed since you read it"* — in tomato, as an accent-coloured `h2`. The `data-bump` control drives it live, and `index.html` already promises it: *"when something changes, we'll tell you."*

**This is the strongest available demo surface** — a real Ode-branded UI whose delta slot our real contradiction fills, instead of two terminals of diffed markdown.

### 3.6 Scope call

**Recommend: generate `seed-content.json` only. Do not touch her HTML/CSS.** One file, an existing fetch, an existing delta surface. Adding a tools page, wiring `chapter.html`'s memory panel to a real store, or generating `whoisthis.html` are all **stretch** — none is needed for the delta to land, and each risks a merge conflict with an open PR authored by someone else.

**Also out:** `people.html` currently has only a heading in the diff — do not assume it is populated.

---

## 4. Persona map — must be composed

The Build Spec requires *"Persona map — who to meet, by role and chapter"* in round 1, and success criterion 1 is *"swap the chapter and the people-to-meet list changes."* **No such page exists.** The spec waves this away — *"Chapter-level variation is confirmed… no need to validate that the dimension matters"* — but **confirming the dimension matters is not the artifact existing.** Criterion 1 is unmeetable until this is built.

Real sources to compose from:

| Source | ID |
|---|---|
| Chapters DB | `collection://96b22b98-eeca-4873-92ae-ba4b13127aed` |
| Chapter 2 (leads, 26 members, 16 projects, `#chapter_2`) | `388dd50dc1fc80b3b762d9a7fa6ccd5d` |
| Team Members DB | `collection://63e85552-5e89-4065-9148-c945e59a5f03` |
| Team Skills Directory | `collection://376dd50d-c1fc-8107-a5d6-000b1090383f` |
| How We Organize Teams (Advisor vs Project/Tech Lead split; department heads Dan, Salma, Xerxes) | `38add50dc1fc8000a747dfb0e26351c0` |

**⚠️ Duplicate-effort risk — resolve before building.** The sibling hackathon project **[Know Your Chapter](https://app.notion.com/p/3a0dd50dc1fc8076b727dd2bfcb6f413)** (linked as a child of our own Build Spec as of 22:10 today) builds from *"the Meet the Team page, the Notion projects database, and Slack channels"* and yields *"a decent lightweight 'who works on what' lookup for the chapter."* **That is the persona map.** And `chapter.html` in Shirley's PR is titled *"Know Your Chapter"* — the same idea, already partly built.

The Build Spec's own warning applies verbatim: *"Don't solve it twice or both assume the other did."* **Talk to them first.** Cheapest win available.

---

## 5. Roster

Build Spec assigns Eng 1: *"seed the demo project's roster — who's on it, in what role"*, and lists **live roster data as Out** (hand-seeded; open question whether Jira/Linear carries assignee+role).

**Finding: it may not need seeding.** The Project DB (`collection://1dedd50d-c1fc-803f-a1b9-000bff4dca74`) already carries real per-project staffing: `FDPM`, `Tech Lead`, `FCTO`, `AI Architect`, `Commercial Principal`, `Team` — plus `Status`, `Phase`, `Industry`, `Short Description`, `Slack - Internal`. **That retires a Build Spec "Out" item** and removes the Quarterdeck/Jira dependency.

**⚠️ But `Chapter` is null on every project row sampled**, while Chapter 2's page lists 16 projects via its own `Projects` relation. The link lives on the **Chapter side only**. So: walk Chapter 2's 16 project URLs; **do not filter the Project DB on `Chapter`** — it returns nothing.

**Coordinate:** the Build Spec says *"if either team seeds roster data, both should use it"* — tell the PTO swarm team what we found, so they don't rebuild it.

**Open:** which of Chapter 2's 16 projects is the demo project. `Atlas` (Goldman Sachs, `387dd50dc1fc815f9c43cc29298815a8`, `Phase: Pre-Sales`, `Status: Active`) is one of them. Since Pair A drops Deep Atlas, the **Atlas namespace-collision hazard is moot** — a hazard that only ever existed for the Deep Atlas pair.

---

## 6. The gate

Build Spec first 15 minutes: *"Lock the contradiction… confirm it produces a materially different, correct doc."*

| Test | Store | Source | Expected |
|---|---|---|---|
| **A** | empty (scratch) | round1 | Doc naming Claude + Granola in the Core Kit |
| **B** | empty (scratch) | round2 | Doc naming GitHub + Deck Builder; no Claude, no Granola |
| **C** (Eng 2) | after A | round2 | Delta stating the reconciliation |

**Gate = A and B are materially different and both correct.** Each round tested against an **empty** store, or we are testing data and memory at once and cannot tell which failed. Use a throwaway `memory_stores.create` per run — no dependency on Eng 2's reset script.

**Run B twice.** Criterion 4 is *"repeats cleanly — not a lucky sample."*

**Rubric — must not:** assert Claude/Granola were "removed" or "deprecated" (no document says so; their absence from the masterlist is a fact, the reason is not). Credit the calibrated answer: *"the page lists Claude and Granola as Core Kit; the masterlist it cites contains neither; no page marks this — worth confirming."* **Name who to confirm with.**

---

## 7. Integrating Shirley (the human)

Shirley Gaw — `shirley.gaw@fractional.ai`, Slack `U0ACAPRBRKQ`, Notion `302d872b-594c-8160-807f-00027935233f`, GitHub `shirlsgaw` (owner of our `origin` and author of PR #9).

She holds unwritten ground truth. Three distinct roles — do not mix them:

1. **Oracle for the answer key, never for the corpus.** Write her verbal confirmation down: one line, dated, attributed — **into the rubric, not `round1/`/`round2/`.** The moment it enters the corpus, the contradiction is documented, the agent just reads it, and the demo evaporates.
2. **The escalation target the agent names.** The best answer routes to a human instead of fabricating. Make *"names who to confirm with"* a CREDIT line and let Shirley be the name.
3. **A risk to close before the freeze — 60 seconds, not a meeting.** Her `Basecamp-Exercises` repo (*"AAI Partner Basecamp exercises"*, updated 2026-07-17 16:45) is the verbal briefing she owes Yury; Alexandra's Slack status is `:tent: At Basecamp`. **AAI Partner Basecamp is real and dated.** Ask exactly one question: *"Does anything in Basecamp change what a new PM's Core Kit is?"* No → freeze. Yes → the pair changes, and we need that **before T+35**, not during narration.

Same timebox for **Christine's in-flight doc**: **hard freeze T+35**; anything later is a post-demo amendment.

---

## 8. Eng 1 / Eng 2 contract

**Eng 1 → Eng 2 (one message at T+60):** frozen corpus at git tag `corpus-frozen` · provenance/trims/exclusions · the shared prompt-variable constants · the rubric incl. the must-nots · gate evidence (`outputs/gate-A-round1.txt`, `gate-B-round2.txt`, graded) · curation targets:

- `PM Core Kit: page says Linear/Notion/GWS/Slack/Claude/Granola (2026-06-28) → masterlist says Linear/Notion/GWS/Slack/GitHub/Deck Builder; Claude and Granola absent from masterlist; unmarked`
- `10 tools added 2026-06-28/29 untiered → invisible to ToolKit; includes ask-fdpm`
- **Expected to be DROPPED by curation:** brand/CSS commentary, contribution instructions, client-stack section

**Eng 2 → Eng 1 (needed by T+35):** reset script (test infra, not demo tooling — critical path) · confirmation `create_agent.py` + `inspect_memory.py` run against the current API · the doc-generation prompt, so Eng 1 knows what shapes the docs being graded.

**⚠️ The repo implements Q&A, not doc-gen.** `run_session_*.py` ask a baseline question and capture an answer; the Build Spec requires a **generated doc with a delta section**. That rework is real, unscoped, on the critical path, and shared. Raise at T+0.

**Merge-conflict seam:** extract shared constants (`config.py`) in Phase 0. Eng 1 owns `config.py` + data loading; Eng 2 owns the prompt + `inspect_memory.py`.

---

## 9. Risks

| # | Risk | Sev | Mitigation |
|---|---|---|---|
| R1 | **`seed-content.json` fails `normalize()` → UI silently shows placeholder** | **High** | §3.4 validation + assert `placeholder === false` before demoing |
| R2 | Legacy synthetic `*.md` still globbed into both rounds | **High** | Archive to `_synthetic-original/` before freeze |
| R3 | Persona map doesn't exist → criterion 1 unmeetable | **High** | §4; coordinate with Know Your Chapter first |
| R4 | Repo is Q&A-shaped; Build Spec needs doc-gen | **High** | Raise at T+0; shared with Eng 2 |
| R5 | Pair A is disagreement, not temporal change | Med | State it plainly in the readout (§1.3) |
| R6 | Duplicate effort with Know Your Chapter / PTO swarm | Med | Talk to both before building |
| R7 | Dirty memory store voids the baseline | Med | Scratch store per gate run |
| R8 | Agent invents "Claude was removed" rationale | Med | Explicit must-not in rubric |
| R9 | Shirley's Basecamp briefing changes the pair | Med | One question before T+35 |
| R10 | Merge conflict with open PR #9 | Med | Generate `seed-content.json` only; don't touch her HTML |
| R11 | Build Spec drifts under us (it changed at 22:10 today) | Low | Body was byte-identical; only a child link was added. Re-diff before freeze |

---

## 10. Open decisions

1. **Which of Chapter 2's 16 projects is the demo project?** Atlas/Goldman Sachs is a candidate (Pre-Sales, Active).
2. **Ping the Know Your Chapter team before composing the persona map?** Recommend yes.
3. **Archive the legacy synthetic files, or delete?** Recommend archive to `_synthetic-original/`.
4. **Does the readout amend success criteria 1–2?** They are written against chapter/project swaps; Pair A's delta is the tool checklist. Criterion 1 still needs the persona map either way.
5. **Tell the PTO swarm team the Project DB already carries roster data?** Recommend yes — retires their dependency too.

---

## 11. Corrections to prior analysis

Recorded so the next reader doesn't inherit the errors.

- **The 401 is not Vercel deployment protection.** It is Shirley's own Google SSO edge middleware. Authenticating the Vercel MCP would **not** have unblocked it — it needs an `@fractional.ai` Google session cookie. Reading PR #9 is the correct route and strictly better than scraping the rendered UI.
- **The "duplicate masterlist" does not exist.** `339dd50d…` is a *view* ("View of Tools & Technologies") over the same `collection://ebbdd50d…`. There is one tools DB.
- **"Ode" is resolved** (closes `data-and-sessions.md` §9 Q5): Alexandra's Slack profile reports `Organization Name: Ode`; the workspace is `odewithanthropic.slack.com`; and `middleware.js` states *"ode.com is the public website; Workspace email is still @fractional.ai as of 2026-07-17."* Ode is the org/public brand; fractional.ai is the Workspace domain.
- **Alexandra's standing is confirmed.** She is **Alexandra Spencer-Wong, Chapter 2 FDPM Lead** — verified in both the Chapters DB and Shirley's `chapter.html`. The Build Spec's recommended Chapter 2 is her chapter.
- **`Default Coding ToolKit` (`35edd50d…`) carries real doc-vs-schema rot:** its body references a *"Core Kit"* tier that `Engineering Labels` does not define (it defines **Core Stack**), and instructs readers to *"pick a Coding Kit Tier"* — a property absent from the schema. Not used for the pair (static rot, not a change), but it corroborates the thesis. Separately, `33add50d…` (Default Coding Toolkit, linked from the Technologies masterlist) **404s to the Notion integration** — a second blind spot in the same class as `Engineer Onboarding Checklist` (`15add50d…`).
