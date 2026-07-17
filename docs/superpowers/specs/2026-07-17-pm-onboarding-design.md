# FDPM Onboarding — Design

**Date:** 2026-07-17
**Author:** Yury Kamen
**Sponsor:** Alexandra (`alexandra@fractional.ai`, `U09J0K96CSD`) — *agreement asserted by Yury, not independently verified; see R2/R7*
**Status:** Design, pending sponsor ratification. Phases 3 and 5 blocked on open inputs (§4.2).
**Scope:** Onboarding for one incoming **FDPM** (full level, not Associate).

---

## 1. Problem

Onboarding a new FDPM currently requires them to navigate a corpus that contradicts itself, points at pages the org has disowned, and is partially invisible to the tooling we would build on. There is no definition of "onboarded."

The ask was: MCP access, a reading list, per-doc quizzes, progress tracking, a feedback channel, resume→project matching, and a list of people to talk to.

**This design deliberately does not build most of that.** The reasoning is in §3 and §9.

## 2. What already exists

Established by direct inspection of the live workspace on 2026-07-17.

### 2.1 The reading list and quiz already exist

`Hackathon NOT REAL: New PM Onboarding: Reading List + Quiz` (`3a0dd50dc1fc816f8b67c4d820cc053c`, updated 2026-07-17 21:55) contains a complete 4-section ordered reading list and a 20-question quiz with answer keys, split across best-practices and content-recall questions.

**Its answers are unverified.** This design treats verification, not authoring, as the work.

Cited docs: `First Principles`, `Your Project Team`, `Types of Customer Projects`, `Organizing Work`, `Delivery Playbook: Soft Start`, `Delivery Playbook: Foundations Phase`, `Foundations Phase (8-Week) Deliverables + RACI`, `Delivery Playbook: Evals`, `Project Reviews`, `Delivery Playbook: Mid Point Reviews`.

### 2.2 The PM corpus is rich and coherent

`FDPM Training Materials`, `Overview of FDPM`, `Product Manager ToolKit` (+ `github.com/fractional-ai/fractional-pm-skills`), `FDPM Career Framework`, `FDPM: Competency Matrix`, `Levels & Tracks`, `Associate FDPM`, `Overview of AFDPM`, `The Dual Mandate`, the Delivery Playbooks, `Best Practices` (Claude), `Best practices for AI product design`.

### 2.3 Known corpus defects

| Defect | Evidence |
|---|---|
| `Engineer Onboarding Checklist` **404s to the Notion integration** | `15add50dc1fc8057bf28fdbe684f33a5` |
| Archive **children carry no superseded marker**; parent's "do not use for onboarding new hires" callout does not inherit | `Onboarding Archive (superseded)` → `First Day Checklist`, `New Hire Welcome Packet`, `Engineer Onboarding — Action Checklist` |
| `NYC New Hire Guide` links to the **archived** `First Day Checklist` as canonical | `383dd50dc1fc802bb9e0d9f25c5ba429` |
| `[DEPRECATED] Delivery Playbook: Phase 1 Onsite` lives **inside live** `Playbook: AI Development` | `2b9dd50dc1fc8083a2cbcac47293ce80` |
| SF office relocates to **315 Montgomery, 11th Floor on 2026-07-20** | `399dd50dc1fc81aca7cdc3ed9dcfc6db` |

### 2.4 MCP access reality

- **Notion MCP respects integration permissions.** Proof: §2.3's 404. The canonical checklist is already invisible to the tool.
- **Slack MCP search is scoped to channel membership.** A day-1 hire belongs to almost no channels, so Slack-derived people-finding returns near-nothing *for the person who most needs it*.
- **Connectors only attach to a *new* message after linking** — observed publicly in `#principals-office-hours` (2026-07-09).

**Consequence:** any feature depending on broad search must run under a buddy's or manager's credentials, not the hire's.

## 3. Principles

| # | Principle | Rationale |
|---|---|---|
| **P1** | **Generator over snapshot.** Reading list and quiz derive from live docs at read time. | A frozen answer key is the most rot-prone artifact possible. §2.3 proves this corpus drifts silently. A static key would become the next contradiction. |
| **P2** | **Judgment over trivia.** Validation is by output, not recall. | `Forward Deployed PM Interview Loop` requires FDPMs to *"fully own product definition, tradeoff decisions, and ambiguous product problems independently."* Recall of a color legend does not predict that. |
| **P3** | **Buddy-run, not hire-run.** Broad-search features run under someone with access. | §2.4. The features degrade to useless precisely for new hires. |
| **P4** | **Net page count must not increase.** Publishing means deleting or redirecting something. | The failure mode is additive: each well-meant artifact becomes another unmarked, competing source of truth. |
| **P5** | **Staffing constraint dominates fit.** Matching is a constrained assignment problem, not similarity search. | A perfect-fit project with no open PM slot is a non-match. |
| **P6** | **Honest labels.** Unratified content ships as "draft, unowned." | Sponsor is unavailable (§R7). Silent authority is how this corpus got here. |

## 4. Decisions

### 4.1 Resolved

| # | Decision | Value | Consequence |
|---|---|---|---|
| D1 | Sponsor | **Alexandra** — agreement asserted by Yury | Slack status `:tent: At Basecamp, slow to respond`; no title set on profile. Ratification will lag → Phase 0 proceeds without her; Phases 1/3 ship as draft (P6). |
| D3 | Level | **FDPM** (not AFDPM) | *"FTPM" appears nowhere in the corpus — read as a typo for FDPM.* Per `Levels & Tracks` an FDPM *"runs a Build engagement end-to-end… your manager isn't backstopping."* → **the coach-capacity hard gate drops out of the matcher** (§6) and becomes a soft signal. |
| D5 | "Approved application" | **Approved tool list** | Phase 2 links `Product Manager ToolKit` (+ `fractional-pm-skills`), `Default Coding ToolKit`, `Tool Recommendations & Demos`, `Tool Access`. No new page (P4). |
| D6 | Validation | **Both** — quiz *and* deliverable | Quiz = **ungated private self-check, no tracking**. Deliverable = the real validation. Because the quiz doesn't gate, it is **generated from live docs (P1)** rather than maintained as a frozen key — this is what makes "keep the quiz" compatible with "don't build the disease." |

### 4.2 Open — blocking

| # | Decision | Blocks | Note |
|---|---|---|---|
| D2 | Hire name + start date | Phases 3, 5 | If start is inside ~2 weeks, **Phase 0 is the only phase that lands in time**; the rest is post-hoc. |
| D4 | Resume location | Phase 5 entirely | Ashby MCP is **unauthenticated** in this session. If the resume lives only in Ashby, it must be connected or the resume pasted. |

## 5. Phases

### Phase 0 — Fix the pointers *(unconditional; start immediately)*

Independent of every open decision. Nothing downstream can read a clean corpus until this lands.

- Resolve the `Engineer Onboarding Checklist` 404-to-integration (permissions or moved)
- Mark the `Onboarding Archive (superseded)` **children** superseded individually
- Repoint `NYC New Hire Guide` off the archived `First Day Checklist`
- Resolve `[DEPRECATED] Delivery Playbook: Phase 1 Onsite` inside live `Playbook: AI Development`
- Verify the Notion integration can read the full PM corpus

**Exit:** a hire following the live trail cannot land on a disowned page.
**Cost:** hours-to-days. **Highest value per hour in this design.**

### Phase 1 — Promote the existing page, do not rebuild it

- Verify each of the 20 answers against its live source; record source page ID + last-edited date per answer
- Retitle (drop "Hackathon NOT REAL"), home under `Onboarding`, assign owner
- Redirect or delete overlapping artifacts (P4)
- Label draft/unowned until Alexandra ratifies (P6)

**Why verify a static page we intend to replace with a generator (Phase 2)?** Two reasons, and without them Phase 1 is waste:

1. **Interim cover.** If the hire arrives before Phase 2 ships (D2-dependent), this page is what they actually read. It must be correct.
2. **Golden set.** The verified answers become the fixture Phase 2's generator is validated against. A generator with nothing to check its output against is untestable.

If D2 shows the hire is far out *and* Phase 2 is cheap, Phase 1 collapses to retitle + rehome + redirect, and verification happens once, inside Phase 2's test suite instead.

### Phase 2 — Reading-list + quiz generator

A skill that queries live Notion and emits *today's* ordered reading list and self-check questions, each with citation + last-edited date. **The maintained artifact is the generator, not the content** (P1). Includes the D5 approved tool list links.

Reuses the pattern already in this repo (`institutional-memory`).

**Output discipline (P4):** each run **replaces** the single canonical generated page — it must not append, version, or spawn per-hire copies. A generator that accumulates output is a contradiction factory with better throughput than a human.

**Validated against** the Phase 1 golden set.

### Phase 3 — Validation *(blocked on D2)*

- **Deliverable (the real gate):** ~day 3, the FDPM drafts a real Soft Start page for an actual project; TL reviews against the Foundations RACI. Aligns with *"Get everyone into an Apprenticeship right away"* (`Onboarding & Guidebook Feedback`, 2026-06-23) and *"curriculum becomes optional/encouraged"* (`Joshua's Other Notes`, 2026-06-08).
- **Quiz (self-check):** generated per Phase 2. Ungated, untracked, private to the hire.

### Phase 4 — MCP access, scoped and gated

| When | Access |
|---|---|
| Day 1 | Onboarding Notion teamspace + public Slack channels only |
| After Drata security training is complete | Broader workspace read |
| Never provisioned as part of onboarding | Ashby (candidate data), comp, `#infra-security`, active pre-sales (e.g. `Atlas`/Goldman Sachs, `Status: Active`) |

- `First Day Checklist` already places **"Acknowledge company policies & complete security awareness training in Drata"** on day 1. Provisioning broad MCP before it completes inverts the control.
- **"How to use Claude with MCP"** → append to the existing `Best Practices` page, which invites it: *"Anyone (including Claude) can append a bullet here."* No new page (P4).
- Document the connector footgun (§2.4).

### Phase 5 — Project matcher *(blocked on D2, D4)*

See §6. Runs under **buddy credentials** (P3).

### Phase 6 — Tracking & Q&A

- Progress in a **Notion DB**, not per-person page copies. The current `First Day Checklist` model (*"please make a copy of this page for yourself"*) makes progress invisible and unauditable.
- Q&A in an **existing** channel. A new `#pm-onboarding` starts with one member — the person with no answers. Q&A needs answerers, not a channel.
- **Freshness monitor (stretch):** scheduled agent flagging deprecated-inside-live, unmarked archive children, live pages citing changed pages, integration 404s. Role-agnostic, compounding — arguably the highest-value component in this document.

## 6. Project matcher design

**Framing:** constrained assignment, not similarity search (P5).

### 6.1 Hard gates — evaluated in order; failing any is a non-match regardless of fit

1. **An open PM slot exists.** Filter first.
2. **Phase fit.** Pre-Sales / Soft Start demands a client-facing shaper; mid-delivery demands execution.
3. **Conflict of interest.** Resume from a client's competitor is a stop.

*(The coach-capacity gate is absent by D3. Were the hire an AFDPM, `Overview of AFDPM` would require a **designated primary coach, ideally their manager**, making the unit of assignment `(project + coach with capacity)`.)*

### 6.2 Soft signals

- Domain / industry overlap (e.g. fintech background → `Atlas` / Goldman Sachs, `Phase: Pre-Sales`, `Status: Active`)
- AI depth vs. what the phase demands — a gap is a **coaching input, not a disqualifier** (`FDPM Training Materials`: *"different levels of AI experience. That's okay!"*)
- Geography / travel — onsites are 3 days; offices SF / NYC / Durham / Dubai
- **Deliberate stretch** — sometimes the worst-fit project is the right call for growth. A similarity-maximizing matcher will never propose this; surface it for a human.

### 6.3 Clarifying questions

Max **3**, asked only where the resume is ambiguous **on a gate**. Example: *"You list 'ML platform' — did you own ground-truth and eval design, or consume it?"* — decision-relevant because the Foundations RACI makes the **AI Architect accountable** for Ground Truth and Eval Strategy, while the **PM must run the mandatory North Star session**.

### 6.4 Output

Ranked shortlist + evidence per signal + confidence + open questions. **Never an auto-assignment** — staffing carries politics and context no agent sees.

### 6.5 People to talk to

Derived from the matched project: its TL, fCTO, PE Principal, and the PM of the most similar prior engagement. Supplemented by Slack signal (who actually answers in the relevant channel; who authored the playbooks). **Runs under buddy credentials** (P3).

## 7. Out of scope

- An LMS
- A new Slack channel
- Auto-assignment of the hire to a project
- **Canonicalizing the engineering curriculum** (Deep Atlas ↔ Anthropic Skilljar). Real, but a different project with different ratifiers. See §9.1.
- Location-specific content authored before the **2026-07-20** SF move

## 8. Risks

| # | Risk | Sev | Mitigation |
|---|---|---|---|
| R1 | Answer key rots; hire learns stale facts confidently | High | P1 — generate, never freeze. Cite + date every answer |
| R2 | Sponsor agreement unverified | Med | Recorded as asserted, not ratified (§4.1). Confirm async |
| R3 | Day-1 broad MCP = data exposure (comp, Ashby, `#infra-security`, active pre-sales) | High | Phase 4 scoping + Drata gate |
| R4 | Slack/Notion MCP blind for new hires | High | P3 buddy-run + Phase 0 integration fix |
| R5 | Quiz measures trivia; gives false assurance | High | D6 — quiz ungated/untracked; deliverable is the gate (P2) |
| R6 | N=1 hire rate → LMS is absurd ROI | Med | Generator, not LMS. Manual-with-Claude until it hurts 3+ times |
| R7 | **Sponsor unavailable now** (`At Basecamp, slow to respond`) → ratification stalls | Med | Phase 0 needs no ratification. Timebox; unratified ships as draft (P6) |
| R8 | We add a 4th competing onboarding artifact | High | P4 — net page count must not increase |
| R9 | Content authored this week is stale on arrival | Low | §7 — no location content before 07-20 |
| R10 | No definition of "onboarded" exists | Med | Phase 3 deliverable *is* the definition: a TL-accepted Soft Start page |

## 9. Corrections to prior analysis

Recorded so the next reader does not inherit the errors.

### 9.1 Canonicalization was wrongly identified as the blocker

An earlier draft declared the PM course blocked on resolving the Deep Atlas ↔ Anthropic Skilljar contradiction, and recommended canonicalization as the first sub-project.

**This was wrong.** The contradiction documented in `data-and-sessions.md` (F1–F11) is **engineering-curriculum-specific**. The PM reading list (§2.1) cites **none** of the contested pages. The error was importing findings from an unrelated audit and applying them without checking domain overlap — the same failure that audit itself warns against (its F2).

The PM corpus is rich and coherent (§2.2). `Onboarding & Guidebook Feedback`'s *"Different set for FDPM"* substantially understates what exists.

### 9.2 "Ode" — open question resolved

`data-and-sessions.md` §9 Q5 asks what "Ode" refers to, noting it *"appears nowhere in the corpus."* Alexandra's Slack profile reports `Organization Name: Ode`, and the workspace is `odewithanthropic.slack.com`. **Ode is the org/workspace name.** Q5 can be closed.

### 9.3 The feature arrow pointed the wrong way

An earlier draft called resume-matching and people-finding "highest leverage" for the hire and MCP provisioning the unblocker. Both wrong — §2.4 shows these features are **least** functional for a day-1 hire. They are buddy-run tools producing an artifact *for* the hire.

## 10. Open questions

1. **D2** — hire name + start date. Blocks Phases 3, 5. Determines whether anything beyond Phase 0 is achievable before arrival.
2. **D4** — resume location. Blocks Phase 5. Ashby MCP unauthenticated.
3. Does Alexandra own onboarding, or is she sponsoring something she doesn't own? Her profile carries no title; observed activity is product/testing in `#internal_kenect`.
4. Who fixes the §2.3 corpus defects — is there an owner for the Onboarding Notion tree?
5. **Who is the buddy?** P3 makes them the operator of the matcher and people-finder, and they are currently unnamed. `Onboarding Buddies` exists as a page; nobody has checked whether buddies have capacity for the work P3 hands them.
6. What is the actual FDPM hire rate? If ~1/quarter, Phases 2 and 6 may not clear ROI and Phase 0 + manual-with-Claude is the whole answer.

---

## 11. Implementation slicing

**This document is a program spec, not a single implementation plan.** Phases 0–6 span Notion ops work, a generator skill, IT provisioning, a matcher, a tracking DB, and a scheduled monitor — different owners, different tech, different lifecycles. Attempting them as one plan would produce a plan that is wrong about all of them.

Each phase gets its own plan → implementation cycle. Recommended order and rationale:

| Order | Phase | Why here | Needs code? | Blocked on |
|---|---|---|---|---|
| 1 | **Phase 0** — fix the pointers | Unconditional; unblocks everything; highest value per hour | No | Nothing |
| 2 | **Phase 1** — promote the page | Interim cover + golden set | No | D1 ratification for the owner field only |
| 3 | **Phase 4** — scoped MCP + Drata gate | The hire needs access on day 1 regardless | No (config) | Nothing |
| 4 | **Phase 5** — matcher | Highest per-hire value once inputs exist | Yes | **D2, D4** |
| 5 | **Phase 2** — generator | Replaces Phase 1's maintenance burden | Yes | Phase 1 golden set |
| 6 | **Phase 3** — deliverable validation | Needs a real project, which needs Phase 5 | No | D2, Phase 5 |
| 7 | **Phase 6** — tracking + freshness monitor | Compounding, but nothing above depends on it | Yes | Nothing |

**Phases 0–3 in that order require no code at all.** If D2 shows a near-term start date, ship those and stop; revisit 4–7 after the hire is actually onboarded and we know what hurt.
