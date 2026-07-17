# Providing Data to Shirley's UI — `seed-content.json` Plan

**Date:** 2026-07-17
**Author:** Yury Kamen (Eng 1)
**Consumer:** [PR #9 `pivot/onboarding-web`](https://github.com/rosscrooke/institutional-memory/pull/9) — shirlsgaw → rosscrooke, OPEN, 9 files, +1783
**Deployed:** `https://web-six-khaki-38.vercel.app` (401 to us — see §8)
**Parent plan:** [`2026-07-17-onboarding-agent-hackathon-plan.md`](./2026-07-17-onboarding-agent-hackathon-plan.md) §3
**Status:** Contract fully reverse-engineered from her source. Producer not built.

---

## 1. Scope

**Produce one file: `web/public/seed-content.json`.** Nothing else.

**Non-goals — do not do these:**
- Do not modify her HTML, CSS, or `middleware.js`. PR #9 is **open** and authored by someone else; edits invite a merge conflict at the worst moment.
- Do not build a tools page, a `people.json`, or a `chapter.json`. **They do not exist as sockets** (§2).
- Do not generate `whoisthis.html`. Gitignored, hers, out of scope.
- Do not attempt to serve, deploy, or authenticate against the live app (§8).

**Why this is the whole integration:** her UI already fetches this file, already renders a delta, and already ships the copy for it. We fill an empty socket. One file, zero negotiation.

---

## 2. There is exactly one socket

Every `fetch()` in the entire PR:

| Call | Where | Ours? |
|---|---|---|
| `fetch('seed-content.json', { cache: 'no-store' })` | `web/public/docs.html` | ✅ **This is the integration** |
| `fetch('https://www.googleapis.com/oauth2/v3/certs')` | `web/middleware.js` — Google JWKS for ID-token verification | ❌ auth internals |

**`people.html` and `chapter.html` are hardcoded.** No data contract, no socket. `chapter.html`'s memory panel self-labels *"Chaptermate memory shown is illustrative placeholder data"* and its quiz chip is a static string (*"Session 1 · initial ingest"*). Wiring those to real data means **editing her HTML** — explicitly out of scope (§1).

And `web/public/.gitignore` is exactly:

```
seed-content.json
whoisthis.html
```

**The file is gitignored — it is a generated artifact by design.** She built the socket and left it for a producer. That producer is us.

Today the socket is empty, so the UI boots `PLACEHOLDER` and self-labels *"Docs — placeholder set (seed content not loaded)"* and *"PLACEHOLDER CONTENT — not a real Ode doc."*

---

## 3. The contract

Reverse-engineered from `normalize()` and `normalizeDelta()` in `docs.html`.

```json
{
  "docs": [
    {
      "id":        "pm-toolkit",
      "title":     "Product Manager ToolKit",
      "version":   "a1b2c3",
      "updated":   "2026-06-28",
      "body":      "First paragraph.\n\nSecond paragraph.",
      "key_facts": ["Core Kit is the day-one stack.", "..."],
      "delta":     { "summary": "...", "changes": ["...", "..."] },
      "quiz": [
        { "q": "...", "options": ["A", "B", "C"], "answer": 1 }
      ]
    }
  ]
}
```

### 3.1 Accepted aliases (her `normalize` is forgiving)

| Canonical | Also accepted |
|---|---|
| root `docs` | `documents`, `pages`, or a **bare array** |
| `body` | `content`, `text` |
| `quiz` | `questions` |
| `q` | `question` |
| `options` | `choices` |
| `answer` | `answer_index` |
| `id` | `doc_id` → else `seed-<i>` |
| `title` | `name` → else `'Untitled doc'` |
| `version` | `version_hash` → else `newHash()` |
| `updated` | `updated_at`, `last_edited` → else `''` |
| `delta` | `supersession`, `supersession_delta` |

**Emit the canonical names.** Aliases are a compatibility surface, not a licence to be sloppy.

### 3.2 `delta` shape

```js
function normalizeDelta(delta) {
  if (!delta) return null;
  if (typeof delta === 'string') return { summary: delta, changes: [] };
  var changes = delta.changes || delta.items || delta.points || [];
  return {
    summary: delta.summary || delta.what_changed || 'This doc changed.',
    changes: Array.isArray(changes) ? changes : []
  };
}
```

A bare string is legal but wastes the `changes` list — **emit the object.** `changes` aliases: `changes` | `items` | `points`.

---

## 4. ⚠️ Silent failure modes — the highest risk in this integration

`normalize()` **discards bad data without erroring** and the UI falls back to placeholder. Every one of these fails *silently, on stage*:

| Rule in her code | Consequence |
|---|---|
| `if (!String(body).trim() \|\| clean.length < 2) continue;` | **A doc with an empty body OR fewer than 2 valid quiz questions is dropped entirely.** |
| Quiz item requires `q`/`question`, `options`/`choices` array with **≥2** entries, and a **numeric** `answer` in range | Malformed question silently discarded — which can push a doc under the 2-question floor and **drop the whole doc**. |
| `quiz: clean.slice(0, 3)` | **Only the first 3 questions render.** Emitting 20 wastes tokens and hides 17. |
| `if (!out.length) return null;` → `boot(PLACEHOLDER)` | **If every doc fails, the UI shows placeholder content with no error.** The demo dies looking like it worked. |
| `if (!raw \|\| typeof raw !== 'object') return null;` | Malformed JSON → placeholder, silently. |

**Hard requirement per doc:** non-empty `body` **and** ≥2 fully-valid quiz questions. **There is no partial credit and no error message.**

**Acceptance gate (§7):** after loading, `src-label` must read **`"Docs"`**, not `"Docs — placeholder set (seed content not loaded)"`. That string is the canary. **Never demo without checking it.**

---

## 5. The staleness mechanic — what actually fires the delta

This is the crux, and it is version-driven:

```js
// A doc's live version = seed version, unless admin has bumped it.
function liveVersion(doc) { var b = bumps[doc.id]; return b ? b.version : doc.version; }
function liveDelta(doc)   { var b = bumps[doc.id]; if (b && b.delta) return b.delta; return doc.delta || null; }
```

- A **receipt** records the `version` the reader read. Receipts are **`localStorage` only** — client-side, per-browser, no server.
- If `liveVersion(doc)` ≠ the receipt's version → the doc is **stale** → the UI renders ***"Something you read has changed"*** in the list and ***"What changed since you read it"*** in the reader, both in tomato (`var(--accent)`).
- `who` (the reader's name, `LS_WHO`) just labels receipts. `''` → *"anonymous"*.

### 5.1 Two ways to fire it — we should use the first

**(a) Regenerate the file with a bumped `version` + a `delta`.** `liveVersion` returns `doc.version`; it no longer matches the receipt; stale fires; **our** `delta` renders. **This is the real mechanic and what our agent should drive.**

**(b) Her Admin "Simulate push → {title}" button** (`data-bump`):

```js
bumps[doc.id] = {
  version: newHash(),
  delta: liveDelta(doc) || {
    summary: 'Someone edited this doc (simulated push from ' + shortHash(prev) + ').',
    changes: ['Content changed. In the real system this list comes from the ' +
              'supersession delta captured at edit time.']
  }
};
```

**Read that fallback string.** *"In the real system this list comes from the supersession delta captured at edit time."* **We are the real system.** If our doc carries a `delta`, her simulate-push renders **ours** instead of that apology. If it doesn't, the demo shows a placeholder confessing the data is fake.

### 5.2 🔴 `bumps` persist in localStorage — reset between runs

Once admin clicks Simulate push, `liveVersion` returns the **bump** forever and **ignores our regenerated file**. A second demo run against a bumped browser shows a random `newHash()` and stale state that has nothing to do with our round 2.

**This is the same class of bug as the missing memory-store reset** in the parent plan (`data-and-sessions.md` §6.2): persisted state silently voids the baseline. **Mitigation: clear `localStorage` (or use a fresh profile / incognito — noting middleware blocks incognito until Google login) before every run. Do not click Simulate push during a version-driven demo.**

---

## 6. Our payload — Pair A mapped to a doc-shaped schema

Her schema is **doc-shaped**; our contradiction is a **tool-checklist diff**. It fits with **zero UI change**: one doc, `id: pm-toolkit`, regenerated across rounds.

### 6.1 Round 1 — from `round1/pm-toolkit.md`

```json
{ "docs": [{
  "id": "pm-toolkit",
  "title": "Product Manager ToolKit",
  "version": "r1-toolkit-2026-06-28",
  "updated": "2026-06-28",
  "body": "Fractional doesn't require a specific toolkit...\n\nSix things cover the core of the role...",
  "key_facts": [
    "Core Kit is the day-one stack: install these first.",
    "The six Core Kit tools are Linear, Notion, Google Workspace, Slack, Claude (Cowork + Code) and Granola.",
    "This page states it is pulled live from the Tools & Technologies masterlist, filtered to the PM picks."
  ],
  "delta": null,
  "quiz": [
    { "q": "You're new. Which tier do you set up first?",
      "options": ["Situational", "Core Kit", "Recommended Addition"], "answer": 1 },
    { "q": "Per this page, which tool did every PM say they couldn't do their job without?",
      "options": ["Linear", "Granola", "Claude (Cowork + Code)"], "answer": 2 },
    { "q": "A client mandates Jira. What does the page say?",
      "options": ["Use Jira only when a client mandates it; Linear stays the internal default",
                  "Switch the team to Jira", "Refuse — Linear is required"], "answer": 0 }
  ]
}] }
```

### 6.2 Round 2 — from `round2/tools-technologies-masterlist.md`

**Same `id`. New `version`. `delta` populated.** That is what fires §5.

```json
{ "docs": [{
  "id": "pm-toolkit",
  "title": "Product Manager ToolKit",
  "version": "r2-masterlist-2026-07-17",
  "updated": "2026-07-17",
  "body": "<regenerated from the masterlist's live PM Labels>",
  "key_facts": [
    "Current PM Labels = Core Kit: Linear, Notion, Google Workspace, Slack, GitHub, Deck / Presentation Builder.",
    "The masterlist contains no row for Claude and no row for Granola.",
    "10 tools added 2026-06-28/29 carry no PM Labels, so they don't appear in the ToolKit's PM-filtered views."
  ],
  "delta": {
    "summary": "The ToolKit page and the masterlist it cites disagree about your Core Kit, and nothing marks it.",
    "changes": [
      "The page lists Claude (Cowork + Code) and Granola as Core Kit. The masterlist it says it is 'pulled live from' contains no row for either.",
      "The masterlist's actual PM Core Kit includes GitHub and Deck / Presentation Builder, which the page's Core PM Stack never mentions.",
      "10 tools were added to the masterlist on 2026-06-28/29 with no PM tier set — including ask-fdpm, a skill named for your role — so they are invisible to this page.",
      "Every PM-tiered row predates 2026-06-08: the ToolKit has been frozen for five weeks while its source grew.",
      "No page records this. Absence from the masterlist is a fact; the reason is not — confirm with Shirley Gaw."
    ]
  },
  "quiz": [
    { "q": "The page says your Core Kit includes Claude and Granola. What does the masterlist it cites say?",
      "options": ["The same six", "Neither tool has a row in it", "They're Situational"], "answer": 1 },
    { "q": "Why doesn't ask-fdpm appear on the ToolKit page?",
      "options": ["It was rejected", "It has no PM Labels tier, so the PM-filtered view excludes it",
                  "It's engineering-only"], "answer": 1 },
    { "q": "What can you correctly conclude about Claude's absence from the masterlist?",
      "options": ["Claude was deprecated", "Claude is absent and no page explains why — it needs confirming",
                  "The page is right and the masterlist is wrong"], "answer": 1 }
  ]
}] }
```

### 6.3 Rubric alignment — the must-not

Quiz option ordering matters: the **wrong** answers encode the failure modes. Q3 in §6.2 makes *"Claude was deprecated"* the **trap**, matching the parent plan's rubric: **absence is a fact, the reason is not.** The `delta` ends by naming who to confirm with — Shirley — which is the CREDIT line in the parent plan §7. **The UI carries the epistemics, not just the content.**

### 6.4 Second doc — optional, only if it costs nothing

`round1` also needs *"one training module"*. `FDPM Training Materials` (16 rows) could ship as a second doc for realism. **It has no contradiction and no delta** — it exists to prove role-filtering, not reconciliation. **Skip unless the primary doc passes the gate first.** One doc with a real delta beats two where one is filler.

---

## 7. Producer

**Where:** a script in this repo writing to `web/public/seed-content.json`. Gitignored → never committed → no conflict with PR #9.

**Two candidate shapes:**

| Option | For | Against |
|---|---|---|
| **(a) Agent emits it** — the Managed Agent writes the JSON as its generated artifact | This *is* the Build Spec's *"agent writes the file"*; the doc-gen output becomes the payload | Agent output must satisfy §4 exactly or fail silently |
| **(b) Deterministic script** — reads `round1/`/`round2/` md, emits JSON | Cannot fail §4; fast; testable | The agent isn't generating the doc → **fails Build Spec success criterion 2** |

**Recommend (a), with a (b)-shaped validator in front of it.** The agent generates; a validator enforces §4 before the file is written. **Never let unvalidated agent output reach the socket** — the failure is silent and looks like success.

### 7.1 Acceptance checklist — run every time

- [ ] JSON parses
- [ ] Every doc: `body` non-empty **and** ≥2 fully-valid quiz questions (`q`, `options` ≥2, numeric in-range `answer`)
- [ ] `normalize()` returns non-null → **`src-label` reads `"Docs"`**, not `"placeholder set"`
- [ ] Round 2's `version` **differs** from round 1's for the same `id`
- [ ] Round 2's `delta.changes` is non-empty
- [ ] `localStorage` cleared; no `bumps` present (§5.2)
- [ ] Reading round 1 then loading round 2 fires *"Something you read has changed"*

**The `src-label` check is the single most important line in this document.**

---

## 8. Why we can't test against the live app

`https://web-six-khaki-38.vercel.app` returns **401** — and **not** from Vercel deployment protection. It is her own `web/middleware.js`:

> *"Runs before any file is served, so curl / View Source / incognito are all blocked until the visitor proves an @fractional.ai Google login."*

`ALLOWED_DOMAINS = fractional.ai,ode.com` · HMAC `fai_session` cookie · 12h TTL · fail-closed without `SESSION_SECRET`. **No MCP connector defeats this** — it needs a Google `@fractional.ai` ID token. Authenticating the Vercel MCP would **not** have helped.

**Consequence: test locally.** Serve `web/public/` statically without the middleware and load `docs.html`. The middleware is a deploy-time concern; the socket is a static fetch.

---

## 9. Risks

| # | Risk | Sev | Mitigation |
|---|---|---|---|
| S1 | Output fails `normalize()` → placeholder renders, no error, demo dies looking fine | **High** | §7 validator + `src-label` canary |
| S2 | `bumps` persisted in localStorage override our regenerated file | **High** | Clear localStorage per run; don't click Simulate push (§5.2) |
| S3 | Round 2 reuses round 1's `version` → stale never fires → **no delta, no demo** | **High** | Acceptance checklist |
| S4 | We edit her HTML → merge conflict with an open PR we don't own | Med | §1 non-goals |
| S5 | Agent emits >3 quiz questions and assumes all render | Low | `slice(0,3)` documented; emit exactly 3 |
| S6 | Agent asserts "Claude was deprecated" in `delta` | Med | Rubric must-not (§6.3); it's the quiz trap |
| S7 | PR #9 merges/changes and the contract moves | Med | Contract read at 2026-07-17; re-diff before the demo |

---

## 10. Open decisions

1. **(a) or (b) for the producer** (§7). Recommend (a) + validator.
2. **Ship the training-module second doc?** (§6.4). Recommend no, unless the primary passes first.
3. **Does Shirley want this file, or does she have her own producer in flight?** `seed-content.json` being gitignored means **she may already be generating it locally** — in which case we are duplicating her work. **Ask before building.** Same class as the Know Your Chapter overlap.
4. **Do we tell her about the `whoisthis.html` nav link?** `index.html` links it; it's gitignored, so it 404s for anyone who clones. Probably intentional; worth a one-line heads-up.

---

## 11. What this buys

Instead of two terminals of diffed markdown, the demo is **a real Ode-branded UI** — real palette, real wordmark, real Google SSO — where a new hire reads their ToolKit, the source changes underneath them, and the page tells them **in Shirley's own words**: *"Something you read has changed."*

Her `index.html` already promises exactly this: *"Your first week, one page at a time. We'll show you who to meet and what to read — **and when something changes, we'll tell you**."*

**Nothing in her app makes that true today.** The socket is empty and the fallback text apologises for itself. **We are the thing that makes her copy honest.** That is the demo.
