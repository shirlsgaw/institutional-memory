# Eng 1 — Data & Sessions

**Owner:** Yury (Eng 1)
**Scope:** Real-page corpus for round 1 / round 2, the baseline question, and the first-15-minutes gate.
**Source docs:** [Build Spec](https://app.notion.com/p/3a0dd50dc1fc80358b41d6c72740566d) · [Demo Write-Up](https://app.notion.com/p/3a0dd50dc1fc818ab901e5aa7e862b2f)
**Status:** Plan + audit. Desk-check of the contradiction is **complete and passing** (see §2). Empirical gate not yet run.

---

## 1. Verdict up front

The contradiction is **real, dated, and load-bearing**. I verified it against the live Notion pages rather than taking the write-up's word for it, and the core claim holds: March 2026 explicitly retires Anthropic Skilljar and mandates Deep Atlas; the July 8 curriculum page is entirely Anthropic Skilljar with Deep Atlas absent. That spine is sound and I'd build on it.

But the write-up has **four problems that change my work**, and two of them are load-bearing enough to resolve before anyone splits:

| # | Finding | Severity | Blocks |
|---|---|---|---|
| F1 | Build Spec and Demo Write-Up specify **two incompatible deliverables** (generated doc vs. two answers) | **Critical** | What round1/round2 must contain |
| F2 | The answer key **demands an assertion the corpus cannot support** | **Critical** | Rubric / grading the gate |
| F3 | Round 2 changes **two variables**; the key grades one | High | Rubric |
| F4 | Both named round-1 supporting pages are **curation-ignore categories** | High | Corpus selection |
| F5 | The obvious prescriptive round-1 page is **already contaminated** with round-2 content | High | Corpus selection |
| F6 | Both "retrieval hazards" **cannot occur** in this architecture | Medium | Narrative honesty |
| F7 | README/code disagree on Files API; my task's wording is ambiguous | Medium | Mechanism |
| F8 | Model/dependency/API surface is actively shifting | Medium | Gate can't start |
| F9 | Persona: write-up's open question is right; corpus backs Engineer | Low-Med | Shared decision |
| F10 | "Deep Atlas is absent" is only true at page level — one hop away it's live | Low-Med | Rubric nuance |
| F11 | Open dependency (Christine Callinan doc) has no cutoff | Low | Freeze timing |

Detail in §3. Plan in §5. Eng2 interaction in §6. Corpus map in §7.

---

## 2. Desk-check: the contradiction (evidence)

I pulled every page the write-up names. All exist; all timestamps check out. **Verbatim** evidence:

### Round 1 anchor — `Onboarding Changes — March 2026` (2026-03-27)
> **Curriculum locked in as Deep Atlas.**
> We have replaced our optimistic ad hoc/apprenticeship training approach with Deep Atlas, a structured one-week applied AI/ML curriculum. **This is now the core of every new engineer's first week.**
> **The old model — Anthropic Skilljar courses, self-directed study, etc — is retired.**
> **Before day one:** Send the new hire Deep Atlas access credentials…
> **Week one:** The engineer works through Deep Atlas as their primary activity… **They should reach out to Deep Atlas on Monday to begin.**

### Round 2 anchor — `GenAI Onboarding Curriculum` (2026-07-08)
> If you aren't already working in GenAI (or even if you are) **you may want to** study through the basics. You have time during your first week and **we encourage you to use it.**
> **Anthropic's Curriculum** covers the courses tagged for their Claude Certified Architect certification…
> **Anthropic:** Self-enroll at anthropic.skilljar.com…
> By end of week one, you are familiar with everything in the Anthropic coursework.

**Deep Atlas: zero mentions.** Confirmed by full-text read, not search.

### Round 2 corroboration — `Onboarding & Guidebook Feedback` (2026-06-23)
> **AI Dev Curriculum is Anthropic, if they want it – pending further changes with them.**
> **Get everyone into an Apprenticeship right away.** Could be real client work, could be a practice project, could be both.
> Different set for FDPM.

### Intermediate state — `Youssef Engineer Onboarding Checklist` (2026-04-21)
> **Choose and begin your curriculum (Deep Atlas or Anthropic)**

Three-stage evolution confirmed exactly as the write-up claims: **Atlas mandatory (Mar) → either/or (Apr) → Anthropic only (Jul)**.

### Additional corroboration the write-up missed
`Joshua's Other Notes` (2026-06-08):
> Curriculum – **becomes optional/encouraged**; expectation is that you know the basics; provide several ways to accomplish that, curriculum being one.

**Verdict: the spine holds. Proceed.** The remaining gate work is empirical (does the agent actually answer differently), not evidentiary.

---

## 3. Audit findings

### F1 — Critical: the two specs describe different products

This is the single biggest issue and nobody has flagged it.

The Demo Write-Up opens with *"That spec stands — repo, scope, success criteria, 2-hour shape."* **It does not stand.** The write-up silently replaces the deliverable:

| | Build Spec | Demo Write-Up | Repo (`run_session_*.py`) |
|---|---|---|---|
| **Output** | Generated onboarding doc (5 sections) | An answer to a question | An answer to a question |
| **Inputs** | role + chapter + project | baseline question | baseline question |
| **Demo** | "two **docs** side by side with a stated delta, **not two answers side by side**" | "session 1 answer, session 2 answer" | `outputs/session1.txt`, `session2.txt` |
| **Criterion 1** | doc specific to role+chapter+project; swap chapter → people change | states a reconciliation | — |
| **Round 1 needs** | onboarding docs + **persona map** + training module + **PM ToolKit** | curriculum pages | 3 synthetic md files |
| **Eng 1 owns** | data + **seed the project roster** | data + sessions + gate | — |

The Build Spec's own words — *"not two answers side by side"* — are a direct negation of the write-up's run of show. These are not compatible readings.

**This determines my scope.** Under the Build Spec I owe a persona map, a PM ToolKit, and a seeded project roster. Under the write-up I owe four curriculum pages. My assigned task list names **none** of the roster/ToolKit work, and the repo implements the write-up's shape.

**Recommendation: adopt the Write-Up's Q&A shape and formally retire the Build Spec's doc-gen scope.** Reasons: (1) it's what my task says, (2) it's what the repo already does, (3) it's what the answer key can actually grade, (4) doc-gen + roster + ToolKit + persona map is not a 2-hour build on top of a corpus swap. **This needs to be said out loud and agreed, not assumed** — Eng2's entire prompt surface depends on which shape we're building, and the Build Spec's success criteria are what a reviewer will grade us against unless someone amends them.

**Knock-on:** the Build Spec explicitly warns *"if either team seeds roster data, both should use it. Don't solve it twice."* If doc-gen is dropped, roster is out entirely → **tell the PTO swarm team we are not seeding it**, so they don't wait on us.

### F2 — Critical: the answer key demands an unsupportable assertion

The write-up is proud that the deprecation is *"written nowhere. It lives in people's heads."* Ground truth is *"Deep Atlas has been deprecated since the Anthropic acquisition. Shirley confirmed this verbally."*

Then it requires session 2 to **"Name Deep Atlas as no longer in use"** and **fails** it for *"hedg[ing] into presenting both as live options."*

**These are in tension.** If the deprecation is written nowhere, then no correct, well-calibrated agent can assert it *as fact* from round-2 documents. What round 2 actually supports:

- ✅ **Supportable:** "Current (Jul 8) guidance is Anthropic Skilljar, self-enroll. This reverses the March page. Deep Atlas is not mentioned in current guidance."
- ⚠️ **Inference, should be marked as such:** "Deep Atlas appears to be out of use."
- ❌ **Unsupportable from the corpus:** "Deep Atlas is deprecated **because of the Anthropic acquisition**" — the rationale exists only in Shirley's head.

As written, the rubric **rewards overconfident inference and punishes correct epistemic humility** — in a demo whose entire pitch is trustworthy institutional memory. If an agent says "Atlas was deprecated post-acquisition," it got the right answer by luck, not evidence, and that's the failure mode enterprise buyers are most afraid of.

**Also:** the closing line *"nobody wrote this down — the agent worked it out"* overclaims slightly. The agent can work out the **supersession** (that's genuinely inferable and genuinely unwritten). It cannot work out the **reason**. Narrate the supersession, not the reason.

**Proposed fix — tiered rubric (§4).** Distinguish *must-state* (supersession, direction, current path) from *must-not* (assert acquisition rationale as fact; present both as equally live) from *credit* (flags the inference as an inference; asks for confirmation).

### F3 — High: round 2 moves two variables; the key grades one

March: curriculum is **mandatory**, **Atlas**, **one week**, **primary activity**, reach out Monday.
July: **Anthropic**, **self-enroll**, and **optional/encouraged** — *"you may want to"*, *"we encourage you"*. Plus apprenticeship immediately (Feedback page), plus *"becomes optional/encouraged"* (Joshua's Notes).

So the reversal has **three axes**: vendor (Atlas→Anthropic), obligation (mandatory→optional), and shape (curriculum-as-primary-activity→apprenticeship-first).

The key only grades axis 1. **An answer saying "Anthropic Skilljar is your mandatory week-one curriculum" passes every stated criterion and is wrong against the source.** That's a rubric hole big enough to produce a false pass at the gate.

### F4 — High: the named supporting pages are exactly what curation says to discard

The write-up nominates `Deep Atlas — Company Research & Valuation Assessment` or `Deep Atlas - Agentic Systems Module Review` as the round-1 supporting page.

Its own curation instructions, four paragraphs later:
> **Ignore: rationale essays, vendor evaluations, valuation assessments.** Keep: what a new hire must do, who owns it, when it changed.

A *valuation assessment* and a *module review* are a valuation assessment and a vendor evaluation. The write-up nominates as round-1 material precisely the two categories it tells the agent to throw away.

Compounding: I read the Module Review in full — it's **~6,000 words** of syllabus, timestamps, and reviewer ratings, with essentially **zero** week-one prescription. As context it's expensive; as evidence for "what do I do Monday" it's nearly empty.

**This is salvageable and even useful — but only if named.** Two defensible readings:
- **(a) Deliberate negative test.** Include it *so that* curation visibly discards it. Then `inspect_memory.py` shows the agent kept "Atlas mandatory wk1 (Mar)" and dropped 6k words of vendor review. That's a genuinely good demo beat — but it must be *stated as the intent*, and Eng2 must know.
- **(b) It's corroboration, trimmed.** Keep intro + summary + reviewer verdict (~600 words) purely to establish "Atlas read as live and endorsed in March."

I recommend **(a) framed as (b)'s trim** — trimmed to ~600 words, included, and expected to be discarded by curation. If it survives into memory as a vendor eval, that's a curation bug worth catching.

**Consequence:** round 1 has **no clean prescriptive supporting page** (see F5). Effectively round 1 = the March page carrying the whole load. That weakens "materially different answers against each **set**" — it's really "against each anchor." Worth knowing before we claim otherwise.

### F5 — High: the obvious round-1 page is contaminated (live trap)

`Your First Week at Fractional Engineering` is the page the March doc designates: *"The new hire's guide is here."* It is the natural round-1 supporting page.

**It was updated 2026-07-15 and now reads:**
> We encourage you to take the **Anthropic curriculum** unless you're already working in GenAI…
> Everything you need… is here: **GenAI Onboarding Curriculum**
> **Apprenticeship** — You'll be associated with one of our ongoing engagements for 1-2 weeks…

Deep Atlas: **gone**. Apprenticeship: **back**.

**Pulling this page "because March links to it" would leak round-2 content into round 1 and collapse the contradiction.** Anyone doing the naive thing — follow the March page's own link, grab the guide — walks straight into it. This is the trap I'd most expect us to fall into under time pressure.

Handling: **exclude it**, or pull the March-era revision via page history (costs time, adds provenance complexity). I recommend exclude, and note it in the manifest as excluded-for-contamination.

Separately: `Engineer Onboarding Checklist` (`15add50d…`) — the checklist the March page says it updated, and the canonical "what to do" artifact — **returns 404 to the Notion integration**. Either permissions or it moved. It's the one page that would have been ideal for round 1. **Access gap to resolve or accept.**

*This finding is itself the thesis.* Every Atlas-era prescriptive page has either silently drifted to Anthropic or become unreachable, with no supersession marker anywhere. The corpus proves the write-up's point better than the write-up does — but it also means round 1 is thinner than advertised.

### F6 — Medium: neither "retrieval hazard" can occur in this build

The write-up lists two hazards "to handle in the prompt":
1. Namespace collision — Deep Atlas (dead vendor) vs. Atlas (live Goldman Sachs pre-sales engagement). **Collision is real** — I confirmed `Atlas`, `Project Name: Atlas`, `Client: Goldman-Sachs`, `Phase: Pre-Sales`, `Status: Active`, updated 2026-07-15/16, with `Atlas Risk Register` / `Atlas Open Questions` live beneath it.
2. Unmarked archive — `Onboarding Archive (superseded)` has a red *"Do not use for onboarding new hires"* callout at the **parent** level; children (`New Hire Welcome Packet`, `First Day Checklist`, `Engineer Onboarding — Action Checklist`) carry no marker. **Also confirmed.**

**Both hazards require search/retrieval over the workspace. This build has none.** `run_session_*.py` inlines a hand-picked file set into the user message. The agent has no Notion access, no workspace search, no retrieval step. With a curated 4-file corpus, **neither hazard can manifest** unless I deliberately seed it.

So: *"An agent doing naive retrieval today sends a new engineer to a dead vendor"* is a true and excellent claim about a **system we are not building.**

**Decision needed:** either
- **(a) Seed the collision.** Add the GS `Atlas` FCTO brief to round 2 as a distractor. Cheap (one file), and it's the **only** way hazard 1 becomes real and demonstrable. The agent must keep "Atlas = live GS engagement" and "Deep Atlas = dead vendor" apart in the same memory store. Good beat, genuinely tests curation.
- **(b) Cut both hazards from the narration.** Claiming to "handle in the prompt" a hazard that cannot arise is a credibility risk the moment someone in the room asks "so what happens if it searches?"

I recommend **(a) for hazard 1** (high value, one file) and **(b) for hazard 2** (the archive can't be reached without retrieval; mention it as *future* risk, honestly labelled).

### F7 — Medium: Files API — README lies, and my task is ambiguous

README: *"Uploads the docs from `synthetic-data/round1/` **via the Files API**"*.
`run_session_1.py:330` — `load_docs_as_context()` reads `*.md` and **string-concatenates them into the user message**. No Files API anywhere in the repo.

My task says *"Pull the real pages **via Files API** rather than re-skinning `synthetic-data/`."* Two readings:
- **(i)** Use Anthropic's Files API to upload real page exports (mechanism).
- **(ii)** "Get the real pages in, don't hand-edit synthetic markdown" (intent) — with "Files API" loose talk inherited from the README's own inaccuracy.

Note Notion→local **cannot** use the Anthropic Files API; that's the Notion API/MCP. So reading (i) is at best "Notion→disk→Files API upload."

**Recommendation: keep inlining.** It works today, it's zero-risk, and the contrast that actually matters is *real pages vs. re-skinned synthetic*, which I satisfy either way. Switching to Files API is a mechanism change with no demo benefit and a live API surface (F8) I'd rather not poke inside 2 hours. **Flag it, get a one-line ruling, don't let it eat the gate.** If Eng2's prompt needs file handles, revisit.

### F8 — Medium: the API surface underneath us is moving

- `create_agent.py:166` pins `model="claude-sonnet-4-6"` — **verify this exists and is enabled on the hackathon workspace.**
- **Dependency sources disagree:** `requirements.txt` → `anthropic>=0.116.0` (comment cites `agent-memory-2026-07-22` beta); `pyproject.toml` → `anthropic>=0.117.0` + `requires-python>=3.14`. Two floors, two files.
- Recent commit: *"Fix memory-stores list call for 2026-07-22 API behavior change"* — and `inspect_memory.py:263` carries a comment that `order_by` is no longer honored. **This API changed under someone within the last week.**
- `stretch_memory_curator.py` is **inconsistent** with the session scripts: it sets `anthropic-beta: managed-agents-2026-04-01` by hand and reads `agent.message_delta`/`text_delta`, while `run_session_*.py` read `agent.message`. One of these is wrong against the current API.
- `memory_backend.py` is dead code that raises `ImportError` on import. Delete it.
- `main.py` / `pyproject.toml` are untracked `uv init` scaffolding ("Hello from institutional-memory!") layered over a `pip`/`requirements.txt` repo. **Two package managers, no decision.** Pick one.

**None of this is my deliverable, but all of it blocks my gate** — I can't verify answers if `create_agent.py` won't run. **Smoke-test at T+0, in parallel with Eng2, before touching data.** (§5 Phase 0.)

### F9 — Low-Med: persona — the write-up is right, and the corpus agrees

The write-up recommends Engineer over the Build Spec's AFDPM. **Concur, and the corpus backs it:** the contradiction is an *engineering* curriculum, and `Onboarding & Guidebook Feedback` says *"Different set for FDPM"* — i.e. the FDPM curriculum diverges and is thinly documented. Choosing AFDPM points the demo at the **least-documented side of the very contradiction we're demoing.**

Unresolved: the Build Spec says *"Chapters are mini-companies within **Ode**"*. This workspace is **Fractional AI**. `Ode` appears nowhere in the corpus I searched. `Chapter` **is** a real property on the Project Database (the GS Atlas project has one), so chapters exist — but the "Ode" reference needs an owner. Possibly inherited from a template or another company's spec. **Flag; don't guess.**

### F10 — Low-Med: "Deep Atlas is absent" is true only at page level

Round 2's own anchor links to *Supplementary Learning Materials* → the **GenAI Training Materials** database → which **still contains a live `Deep Atlas` entry** (`Format: Hands-on`, `Hours (est): 95 hrs`, `Notes: Fundamental-heavy`, linking deepatlas.ai/syllabus).

So the July page doesn't mention Atlas, but it is **one hop** from a live Atlas entry. Strengthens the thesis (nothing is marked superseded, anywhere) — and slightly undercuts the flat claim "Deep Atlas is absent" in round 2.

Minor bonus inconsistency: March says *"one-week"* curriculum; the DB entry says **95 hrs**; `Q4 Holiday Plans` says *"two weeks material"*. Nobody agrees how long Atlas is. Don't include the DB unless we want that distractor.

### F11 — Low: open dependency has no cutoff

An onboarding doc from **Christine Callinan** is in flight and *"may change the round-2 set."* There is no cutoff rule. **Propose: hard freeze at T+35.** Anything landing after that is a post-demo amendment, not a scramble. If it lands before, I evaluate it as a round-2 candidate on the same criteria as everything else.

Also noted, out of scope but relevant to me personally: *"Shirley is briefing **Yury** verbally on a separate onboarding issue involving Anthropic Basecamp."* That's me. If that briefing touches curriculum, it may be a **second** unwritten ground truth — worth 60 seconds of checking that it doesn't contradict our answer key.

---

## 4. The gate, and how I'll grade it

### 4.1 Reframing the gate (important)

The spec's gate: *"Verify the baseline question produces materially different correct answers against each set before we split."*

The subtlety: to test whether **the data** carries the contradiction, each set must be tested **independently, against an empty memory store**. Otherwise I'm testing data and memory at once and can't tell which one failed.

This decouples cleanly and it's the key to us working in parallel:

| Test | Store | Docs | Expected | Tests |
|---|---|---|---|---|
| **A** | empty (scratch) | round1 only | Atlas path | data |
| **B** | empty (scratch) | round2 only | Anthropic path (optional, self-enroll, apprenticeship) | data |
| **C** (later, Eng2) | after A | round2 | reconciliation naming the reversal | memory |

**Gate = A and B are materially different and both correct.** That's mine, it needs no prompt tuning, and it does not wait on Eng2.
**C is the demo** and belongs to Eng2's prompt work, against a **frozen** corpus.

I use a **throwaway memory store per gate run** (`memory_stores.create`) — so I don't block on Eng2's reset script, and A/B stay hermetic.

**Note:** session 1 is only a true baseline against an **empty** store. Nothing in the repo clears it today (§6 dependency).

### 4.2 Baseline question (unchanged, both sessions)

> "I'm a new engineer starting Monday. What curriculum do I do in week one, and how do I get started?"

Single source of truth — extracted to a shared constant (§6.3), not duplicated across two files as it is today.

### 4.3 Round 1 expected answer (test A)

- ✅ Deep Atlas, one week, **primary activity** of week one
- ✅ Credentials arrive **before day one**
- ✅ **Reach out to Deep Atlas Monday** to begin
- ✅ Alongside: account setup, GenAI self-evaluation, sit in on client meetings
- ✅ Anthropic Skilljar is **retired**
- ✅ Week two: start contributing to a project

### 4.4 Round 2 expected answer (test B) — tiered, fixing F2 + F3

**MUST state (all three axes — fixes F3):**
- ✅ **Vendor:** Anthropic Skilljar, self-enroll at anthropic.skilljar.com, Claude Certified Architect courses
- ✅ **Obligation:** **optional / encouraged** — *not* mandatory
- ✅ **Shape:** apprenticeship / real project engagement from the start; escalate to `#engineering` for Claude dev access; `#engineering_onboarding` for help

**MUST NOT:**
- ❌ Present Atlas and Anthropic as **equally live options** (that was April's answer, not July's)
- ❌ Say Anthropic Skilljar is **mandatory** (fails F3 — passes the write-up's key while being wrong)
- ❌ Assert **"deprecated because of the Anthropic acquisition"** as established fact (fixes F2 — unsupported by any document; if it says this, it guessed)

**In session 2 (test C), MUST additionally:**
- ✅ **State that this reverses session 1** — silent correctness is a fail (criterion 1)
- ✅ Name Deep Atlas as **not in current guidance**

**CREDIT (the calibrated answer, per F2):**
- ⭐ Marks "Atlas is out of use" as an **inference from absence + corroboration**, not a documented fact
- ⭐ Notes no page anywhere carries a supersession marker
- ⭐ Says who to confirm with

**Grading note:** an agent that says *"current guidance is Anthropic-only; Atlas is absent from it and appears retired, though I find no page stating so — worth confirming"* is the **best** answer, and under the write-up's rubric as written it risks being scored a hedge. My rubric scores it top. I'd rather ship the honest agent and narrate the honesty.

---

## 5. Implementation plan

Budget: ~60 min of my 2h, front-loaded. The desk-check (§2) is **already done**, which is what makes a "first 15 minutes" gate plausible at all — extraction is the only real work left.

### Phase 0 — T+0→10 · Decisions + smoke test (parallel with Eng2)

- [ ] **Get rulings on F1 (deliverable shape) and F7 (Files API).** Two questions, one message. F1 is blocking — I cannot pick corpus content without it.
- [ ] Confirm persona = **Engineer** (F9).
- [ ] Decide: seed GS Atlas distractor? (F6)
- [ ] **Eng2 in parallel:** `create_agent.py` smoke test — model `claude-sonnet-4-6` valid? memory store creates? `inspect_memory.py` lists? (F8). This de-risks my gate without blocking me.
- [ ] Resolve dependency floor: `requirements.txt` vs `pyproject.toml` (F8). Pick one manager.
- [ ] `rm memory_backend.py` (dead code, raises on import).

**Exit:** shape agreed, API confirmed working. **If `create_agent.py` fails, that is the whole team's problem, immediately.**

### Phase 1 — T+10→35 · Corpus extraction

**Extraction discipline (non-negotiable — this is what "real pages, not re-skinned" means):**
- **Verbatim text only.** No paraphrase, no rewording, no smoothing.
- Trimming = **whole-section deletion only**, every deletion logged in the manifest.
- Every file carries provenance front-matter: page ID, URL, title, last-edited timestamp, extraction timestamp, sections dropped.
- **Never edit content to sharpen the contradiction.** The contradiction is real; if I "help" it, the demo becomes a lie and the one question I can't survive is *"did you edit these?"*

**round1/ — March 2026 state**

| File | Source | ID | Treatment |
|---|---|---|---|
| `2026-03-27-onboarding-changes-march-2026.md` | Onboarding Changes — March 2026 | `32edd50dc1fc81488f03e09c7efc4438` | **Full, verbatim.** The anchor. |
| `2026-03-19-deep-atlas-agentic-systems-module-review.md` | Deep Atlas - Agentic Systems Module Review | `322dd50dc1fc80bfb97becc125eb9059` | **Trim** to intro + summary + reviewer verdicts (~600 of ~6,000 words). Establishes Atlas read as live+endorsed in March. Expected to be **discarded by curation** (F4). |

**Excluded from round 1, on purpose:**
- ❌ `Your First Week at Fractional Engineering` — **contaminated**, updated 2026-07-15 to the Anthropic path (F5). The trap.
- ❌ `Engineer Onboarding Checklist` (`15add50d…`) — **404 to the integration** (F5).
- ❌ `Youssef Engineer Onboarding Checklist` — April **either/or** state; that's the intermediate, not round 1.
- ❌ `Deep Atlas — Company Research & Valuation Assessment` — valuation assessment; curation-ignore, and the Module Review already covers "Atlas was endorsed."

**round2/ — July 2026 state**

| File | Source | ID | Treatment |
|---|---|---|---|
| `2026-07-08-genai-onboarding-curriculum.md` | GenAI Onboarding Curriculum | `343dd50dc1fc813bab53ccf3d8526ee8` | **Full, verbatim.** The anchor. |
| `2026-06-23-onboarding-and-guidebook-feedback.md` | Onboarding & Guidebook Feedback | `356dd50dc1fc802f9d1dd72c0419abba` | **Trim** to Rationale + Conclusions & Proposal. Drop NHO scheduling lists / Deep Dive DB (noise). Carries *"Anthropic, if they want it"* + *"Apprenticeship right away"*. |
| `2026-07-15-atlas-goldman-sachs-fcto-brief.md` | Atlas (GS) | `387dd50dc1fc815f9c43cc29298815a8` | **Conditional on F6(a).** Distractor. Trim to FCTO brief + key properties. |

**Note:** round 2 deliberately does **not** contain the March page. Round 1's content reaches session 2 **only through memory** — that's the entire mechanic.

- [ ] Extract via Notion MCP → `synthetic-data/round1/`, `round2/` as `.md` (scripts glob `*.md`)
- [ ] Write `synthetic-data/MANIFEST.md` — provenance, trims, exclusions **and why** (F4/F5 reasoning lives here, so the next person doesn't undo it)
- [ ] Archive the original synthetic files to `synthetic-data/_synthetic-original/` (don't delete — they're the fallback if the real corpus somehow fails the gate)
- [ ] **Hard freeze T+35** (F11)

### Phase 2 — T+35→55 · Run the gate

- [ ] Extract `TEST_QUESTION` to a shared constant (§6.3) — kills the duplication and the merge conflict
- [ ] Run **test A**: scratch store + round1 → `outputs/gate-A-round1.txt`
- [ ] Run **test B**: scratch store + round2 → `outputs/gate-B-round2.txt`
- [ ] Grade both against §4.3 / §4.4. Record verdict + verbatim excerpts.
- [ ] **Run B twice.** Criterion 4 is "not a lucky sample" — one run proves nothing about repeatability, and B is the one carrying three axes.

**Gate exit criteria:**
- A lands the Atlas path; B lands Anthropic **+ optional + apprenticeship**
- The two are **materially different** to a reader with no context
- Neither invents the acquisition rationale
- **If B misses the optionality axis (F3): that's a data problem, and it's mine.** Likely fix: the optionality signal is soft (*"you may want to"*) — consider adding `Joshua's Other Notes` (*"becomes optional/encouraged"*, `36cdd50dc1fc80e290f7ee39f4e4efe9`) to round 2 as explicit corroboration. **Held in reserve, not in the initial set.**

### Phase 3 — T+55→60 · Freeze + handoff

- [ ] Tag the corpus commit (`corpus-frozen`) — Eng2 tunes against an immovable target
- [ ] Post handoff (§6.1): tag, manifest, question, rubric, gate evidence, verdict
- [ ] **Then** Eng2 owns test C

---

## 6. Interaction with Eng 2

Eng2 owns: curation prompt, `inspect_memory.py` legibility, reset script, side-by-side narration.

### 6.1 The contract

**Eng1 → Eng2 (at T+60, one message):**
1. **Frozen corpus** at git tag `corpus-frozen` — `round1/` + `round2/`
2. **`MANIFEST.md`** — provenance, trims, exclusions and why
3. **`TEST_QUESTION`** — one shared constant, not two copies
4. **Tiered rubric** (§4.4) — what "correct" means, including the F2/F3 fixes
5. **Gate evidence** — `outputs/gate-A-round1.txt`, `gate-B-round2.txt`, graded
6. **Curation targets** — the concrete facts I expect to survive into memory:
   - `curriculum: Deep Atlas mandatory week 1 (Mar 2026) → SUPERSEDED by Anthropic Skilljar self-paced/optional (Jul 2026)`
   - `Atlas (Deep Atlas) = training vendor, out of current guidance` **≠** `Atlas = live Goldman Sachs pre-sales engagement` (if F6(a))
   - **Expected to be DROPPED:** the 6k-word Module Review, the valuation assessment (F4)

**Eng2 → Eng1 (needed by T+35, before my gate ideally):**
1. **Reset script** — *see 6.2, this is a real dependency*
2. Confirmation that `create_agent.py` + `inspect_memory.py` work against the current API (F8)
3. System prompt / curation instructions — so I know what's shaping the answers I'm grading

### 6.2 The dependency nobody has written down

**There is no reset script, and nothing in the repo clears the memory store.** `create_agent.py` creates a store; `run_session_1.py` attaches it `read_write`. Re-run session 1 against a dirty store and **session 1 is no longer a baseline** — it answers partly from memory of a previous session 1. Criterion 4 ("repeats cleanly — not a lucky sample") is **unmeetable** without it, and so is my gate.

The write-up assigns the reset script to Eng2 as demo tooling. **It isn't demo tooling — it's test infrastructure, and it's on the critical path for the gate.**

**My mitigation (so I don't block):** scratch memory store per gate run. But Eng2 still needs it for C and for the live demo. **Raise at T+0, not T+50.**

### 6.3 Merge-conflict hazard (concrete)

`TEST_QUESTION` and the `user_message` scaffolding live in **the same two files** we will both edit:
- I need `DOCS_DIR`, `load_docs_as_context()`, `TEST_QUESTION`
- Eng2 needs the `instructions` string, `user_message` prompt, `SYSTEM_PROMPT`

Both in `run_session_1.py` / `run_session_2.py`. Guaranteed conflict, at exactly the moment we can least afford one.

**Proposal — 5 minutes, do it in Phase 0:** extract `config.py`:
```python
TEST_QUESTION = "I'm a new engineer starting Monday. What curriculum do I do in week one, and how do I get started?"
ROUND1_DIR = Path("synthetic-data/round1")
ROUND2_DIR = Path("synthetic-data/round2")
```
**I own `config.py` + data loading. Eng2 owns `SYSTEM_PROMPT` + prompt scaffolding + `inspect_memory.py`.** Clean seam, no overlap.

### 6.4 Sequencing

```
T+0 ──────────── T+10 ─────────── T+35 ────────── T+55 ── T+60
Eng1:  decisions │ extract corpus  │ gate A/B      │ freeze │ handoff ──┐
                 │                 │ (scratch store)│        │           │
Eng2:  smoke test│ reset script    │ prompt+curation│  ...   │ ◄─────────┘
       (F8)      │ inspect_memory  │ (vs frozen)    │        │ test C, narration
```

**Critical path is mine.** Eng2 cannot meaningfully tune curation until the corpus is frozen — tuning against a moving corpus is wasted work. So Eng2's first 35 minutes should be **smoke test + reset script + `inspect_memory.py` legibility**, all of which are corpus-independent and all of which I or the demo need. That's the parallelism that actually works here; anything else has Eng2 tuning prompts against data I'm still changing.

### 6.5 Shared decisions — must be agreed before splitting

| # | Decision | My recommendation |
|---|---|---|
| 1 | **F1: doc-gen or Q&A?** | **Q&A.** Formally retire Build Spec's doc-gen/roster/ToolKit. Tell PTO team roster is unowned. |
| 2 | **F2/F3: rubric wording** | Adopt tiered rubric (§4.4). Don't fail calibrated hedging; do fail invented rationale. |
| 3 | **F6: seed Atlas distractor?** | **Yes** for the GS collision (1 file, high value). Drop the archive hazard from narration. |
| 4 | **F9: persona** | **Engineer.** |
| 5 | **F7: Files API** | **Inline.** Revisit only if Eng2's prompt needs file handles. |
| 6 | **F11: freeze time** | **T+35.** |
| 7 | **Narration honesty** | Narrate the **supersession** the agent inferred, not the acquisition rationale it can't know. |

---

## 7. Context found in the workspace

Everything the write-up references, verified to exist, with IDs. All timestamps confirmed by fetch.

### Round 1 / Round 2 principals

| Page | ID | Updated | Role |
|---|---|---|---|
| Onboarding Changes — March 2026 | `32edd50dc1fc81488f03e09c7efc4438` | 2026-03-27 | **R1 anchor** |
| Deep Atlas - Agentic Systems Module Review | `322dd50dc1fc80bfb97becc125eb9059` | 2026-03-19 | R1 support (trim) |
| Deep Atlas — Company Research & Valuation Assessment | `32edd50dc1fc816a940af8d8353eb09e` | 2026-03-25 | Excluded (F4) |
| GenAI Onboarding Curriculum | `343dd50dc1fc813bab53ccf3d8526ee8` | **2026-07-08** | **R2 anchor** |
| Onboarding & Guidebook Feedback | `356dd50dc1fc802f9d1dd72c0419abba` | 2026-06-23 | R2 support (trim) |

### Corroboration / intermediate states

| Page | ID | Updated | Why it matters |
|---|---|---|---|
| Youssef Engineer Onboarding Checklist | `348dd50dc1fc80968c63d9bfdde02d9a` | 2026-04-21 | **April either/or** — "Deep Atlas or Anthropic". Confirms 3-stage evolution. Optional session 3. |
| Joshua's Other Notes | `36cdd50dc1fc80e290f7ee39f4e4efe9` | 2026-06-08 | *"Curriculum – becomes optional/encouraged"*. **Reserve** for F3 optionality. |
| GenAI Training Curriculum Proposal | `326dd50dc1fc816caa0dfcf47ee91758` | 2026-03-19 | Rationale for choosing Atlas. Curation-ignore. |
| Q4 Holiday Plans | `2ccdd50dc1fc803984f6ca69e86b65e2` | 2025-12-18 | *"two weeks material"* — Atlas duration inconsistency (F10) |
| Anthropic's Claude Certified Architect | `34add50dc1fc80c78462f27dc3ba0944` | 2026-04-22 | Anthropic path detail |
| Anthropic Skilljar Courses Notes | `34add50dc1fc804981b1f50ea94f353c` | — | Linked from R2 anchor |

### Traps and hazards (all verified)

| Item | ID | Updated | Hazard |
|---|---|---|---|
| **Your First Week at Fractional Engineering** | `32edd50dc1fc8105a051fd16b3b150d8` | **2026-07-15** | ⚠️ **Contaminated.** March designates it; now says "Anthropic curriculum". **Do not put in round 1.** (F5) |
| Engineer Onboarding Checklist | `15add50dc1fc8057bf28fdbe684f33a5` | — | ⚠️ **404 to integration.** The ideal R1 page, unreachable. (F5) |
| **Atlas** (Goldman Sachs) | `387dd50dc1fc815f9c43cc29298815a8` | 2026-07-15/16 | ⚠️ **Namespace collision.** Live, `Phase: Pre-Sales`, `Status: Active`. Distractor candidate. (F6) |
| Onboarding Archive (superseded) | `37cdd50dc1fc81599d02f20faeb968d2` | 2026-06-12 | ⚠️ Red callout at **parent only**; children unmarked. (F6) |
| Engineer Onboarding — Action Checklist | `379dd50dc1fc819b831ad6c2d2b186ef` | 2026-06-09 | Archived but linked from live `Onboarding Items` |
| Deep Atlas (GenAI Training Materials DB) | `2d1dd50dc1fc80f183b8c0e19c1c2b37` | 2026-04-15 | ⚠️ **Still live**, `95 hrs`. One hop from R2 anchor. (F10) |

### Discovered, not in the write-up

| Page | ID | Note |
|---|---|---|
| Onboarding (hub) | `80e9a5860a5b4505be79da801d78902b` | Parent of the curriculum pages; updated 2026-07-16 |
| Onboarding Vision | `37cdd50dc1fc801ca1abe73c42956cc8` | *"Standardize FDPM-specific onboarding items"* — supports Engineer persona (F9) |
| A Guide to Onboarding | `2515a03e0a5849d3bad097883e486a69` | Company-wide, 2026-06-22 |
| AI Architect - Onboarding Guide | `2ebdd50dc1fc801a9ba6c1a78f8f8a71` | Role-specific template |
| Onboarding To-Dos | `379dd50dc1fc81dcb1cadb128503fd80` | The live replacement for the archive |
| Onboarding Buddies | `2ccdd50dc1fc80daa02bcc0846f51181` | — |

### Repo components (as-is)

| File | Role | Owner |
|---|---|---|
| `create_agent.py` | Agent + Environment + Memory Store; holds `SYSTEM_PROMPT` | **Eng2** |
| `synthetic-data/round1/`, `round2/` | Corpus | **Eng1** |
| `run_session_1.py` | round1 → inline → session → `outputs/session1.txt`; holds `TEST_QUESTION` | **Split** → `config.py` (6.3) |
| `run_session_2.py` | round2 + memory → `outputs/session2.txt` | **Split** |
| `inspect_memory.py` | Memory store listing; projector-legibility | **Eng2** |
| `stretch_memory_curator.py` | Curator subagent (stretch); **inconsistent event API** (F8) | Eng2 |
| `memory_backend.py` | **Dead.** Raises `ImportError`. Delete. | — |
| *(missing)* | **Reset script** — test infra, critical path (6.2) | **Eng2** |
| `main.py`, `pyproject.toml` | Untracked `uv` scaffolding over a `pip` repo (F8) | — |

### Workflow (as-is)

```
create_agent.py ─→ agent + environment + memory store (.agent_id/.environment_id/.memory_store_id)
      │
      ▼
run_session_1.py ─→ round1/*.md inlined ─→ session (store read_write) ─→ session1.txt + memory writes
      │
      ▼  inspect_memory.py
      │
run_session_2.py ─→ round2/*.md inlined + memory ─→ NEW session, same store ─→ session2.txt (reconciliation)
      │
      ▼  inspect_memory.py  →  diff session1.txt session2.txt   ← the demo
```

**Note:** no retrieval anywhere. Corpus is hand-picked and inlined. This is why F6 matters.

---

## 8. Risks

| Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|
| **F1 unresolved; we build different products** | Med | **Critical** | Ruling at T+0. Blocking. |
| **API surface broken (F8)** | Med | **Critical** | Eng2 smoke test at T+0, parallel |
| **`Your First Week` leaks into round 1 (F5)** | Med | **High** | Manifest records exclusion **and why**; it's the natural mistake |
| **Round 2 misses optionality (F3)** | Med | High | `Joshua's Other Notes` held in reserve |
| **Dirty memory store voids the baseline (6.2)** | Med | High | Scratch store per gate run; Eng2 reset script |
| **Agent invents "post-acquisition" rationale (F2)** | Low-Med | Med | In rubric as explicit fail; narrate supersession only |
| **Round 1 is effectively single-page (F4/F5)** | High | Med | Accept + state. Don't claim a richer set than exists. |
| Christine doc lands late (F11) | Med | Low | T+35 freeze; post-demo amendment |
| Merge conflicts in `run_session_*.py` (6.3) | High | Low | `config.py` seam, Phase 0 |
| "So what if it searches?" in Q&A (F6) | Med | Med | Seed the distractor, or say plainly it's not built |

---

## 9. Open questions

1. **F1 — Which deliverable?** Blocking. My corpus content depends on it.
2. **F2 — Does the room want the calibrated agent or the confident one?** I recommend calibrated, narrated as such. This is a positioning call, not a technical one.
3. **F7 — Files API or inline?** One-line ruling.
4. **F5 — Can anyone unblock `Engineer Onboarding Checklist` (`15add50d…`, 404)?** Permissions or moved. Ideal round-1 page.
5. **F9 — What is "Ode" in the Build Spec?** Chapters are real here; "Ode" isn't. Needs an owner.
6. **F11 — Does Shirley's Anthropic Basecamp briefing to me touch curriculum?** If so, second unwritten ground truth. 60 seconds to check, and I'm the one being briefed.
7. **Amend the Build Spec, or supersede it?** Given F1 — and the irony of an institutional-memory demo whose own two specs contradict each other with no supersession marker — I'd mark the Build Spec superseded explicitly. Otherwise we're the thing we're demoing.
