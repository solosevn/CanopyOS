# CanopyOS — Session S2004 Recap

**Date:** May 26, 2026
**Architect:** David Solomon
**Cowork agent:** Claude Sonnet 4.6 via Cowork mode
**Next session:** S2005
**Live document:** https://solosevn.github.io/CanopyOS/
**Live URL state at close:** Sections 01 + 02 live and unchanged. Section 03 built locally, merged draft ready — NOT yet pushed as index.html.

---

## Session Objective

Build the Phase 3 interactive architecture diagram (`canopyos-architecture.html`) and complete all agent hover content. Create a merged single-scroll HTML (`index-merged.html`) combining Phases 1, 2, and 3 into one document ready for review and eventual push as the new `index.html`.

---

## Work Completed

### 1. `canopyos-architecture.html` — Full Build (New File)

An interactive HTML visualization was built from scratch. It features:
- A dark navy "arena" (860×1140px, portrait) with a PCB/CPU chip representing the CMC at center
- Nine orbit agents rotating around the CMC on a 34-second cycle
- Hover tooltips for each agent with skill breakdowns, CMC interactions, and sub-agent lists
- A click-open modal system for the CMC bands (Source Truth, Compilation, Retrieval bands)
- Canopy Agent box at top with gold border, two vertical cyan lines connecting it to CMC
- Document Ingestion strip + Field Users / System Output side panels
- Fully interactive: agents pause orbit on hover, tooltips appear above/below depending on position

**Arena geometry (current, locked for S2005):**
- `width: 860px; height: 1140px`
- `CX = 430, CY = 700, ORBIT_R = 300, HALF = 46`
- CMC wrapper: `top: 380px; left: 50%; width: 640px; height: 640px`
- CMC chip inside wrapper: `top: 141px; left: 141px; width: 358px; height: 358px`
- Canopy Agent: `top: 180px; left: 50%; transform: translateX(-50%)`
- Orbit ring SVG: `cx="430" cy="700" r="300"`

### 2. Tooltip System

Tooltip is `position: fixed` (not relative to any container), positioned via JS `getBoundingClientRect()` on each agent node at hover time. This was required because `overflow: hidden` on parent containers was clipping the tooltip.

- Width: 580px
- `TIP_W = 580` constant in JS
- Shows above agent when agent Y > 200px viewport, below otherwise
- Each tooltip has: header (agent name, icon), short description paragraph, then a breakdown section with Skill, CMC, and Sub-agents rows

### 3. Agent Tooltips Completed

All four agents below had full tooltip content written and confirmed by David:

**RFI Agent:**
- Description: Monitors the drawing set, specs, and project docs for gaps, conflicts, and unanswered questions — and manages the lifecycle of every RFI from identification through response and close-out
- Skills: `rfi-lifecycle`, `drawing-gap-analysis`, `spec-cross-reference`, `response-evaluation`
- CMC: Reads drawings, specs, prior RFIs; writes resolved answers with source citations
- Sub-agents: Drawing Gap Scanner, Spec Cross-Reference Agent, Response Evaluator, RFI Drafter, PCO Trigger Agent

**Change Management Agent:**
- Description: Owns the full change event lifecycle — from the first signal that scope may be changing through pricing, approvals, TIA analysis, and contract closeout
- Skills: `change-event-triage`, `pco-drafting`, `tia-analysis`, `log-management`, `closeout`
- CMC: Reads contract, schedule, drawings, prior change orders; writes change event records, PCO drafts, approved change orders
- Sub-agents: Change Event Monitor, PCO Drafter, TIA Agent, Owner Change Log Manager, Commitment Change Tracker

**Document Agent:**
- Description: Foundation layer of CanopyOS — receives, versions, indexes, and monitors every project document, and alerts other agents when documents change in ways that affect their domain
- Skills: `document-ingestion`, `version-control`, `change-detection`, `alert-routing`, `source-binding`
- CMC: Primary curator — ingests all documents into Source Truth Band; maintains version history; triggers recompilation when documents change
- Sub-agents: Ingestion Agent, Version Control Agent, Change Detection Agent, Alert Router, Source Binder

**Constraint Agent:**
- Description: The Constraint Agent vets possible constraints — searches the lookahead schedule and active work plans, then identifies true constraints vs normal predecessor work vs items already managed through their own process (CIL, submittals, RFIs). Owns the Constraint Log, calculates Date Needed By through backwards chain math, routes to the right meeting (Make-Ready, Constraint Meeting, OAC, OM Huddle), sends notices to responsible parties, and tracks KPIs: identification performance (>4 weeks out?), closure performance (closed by Date Needed By?), and urgent constraints (<2 weeks).
- Skills: `constraint-identification`, `date-needed-by-calc`, `log-management`, `meeting-routing`, `kpi-tracking`
- CMC: Reads schedule (lookahead + contract), CIL, RFIs, open change events; writes constraint log entries, Date Needed By records, escalation notices
- Sub-agents: Lookahead Scanner, Date Chain Calculator, Constraint Log Manager, Meeting Router, Notice Dispatcher

**Agents still needing tooltips (S2005):**
- CIL Agent, Production Agent, Code Agent, Safety Agent, Scheduling Agent, Canopy Agent (hover content)

### 4. Visual Design Updates

- **Portrait layout:** Resized from landscape to portrait (860×1140) with updated CX/CY/CMC positions
- **Removed dashed connector lines:** All dashed arrows between Canopy Agent and arena were removed
- **Canopy Agent box styling:** Gold border, dark background, italic sub-name, natural language tagline. Final text: "🌳 Canopy Agent / (each project will give it a name) / Two-way communication · Natural language"
- **Document Ingestion strip:** Top area of arena with 6 pills: Drawings, Specifications, RFIs, Submittals, Field Photos, Transcripts
- **Field Users / System Output:** Side panels at same vertical level as Canopy Agent

### 5. Two Cyan Vertical Lines (Canopy Agent → CMC)

Two `<line>` elements in the arena SVG:
- IDs: `canopyLine1` (x=425) and `canopyLine2` (x=435)
- Color: `#00ccee`, `stroke-width="1.5"`, `opacity="0.45"` at rest
- On Canopy Agent hover: opacity jumps to 1 + CSS drop-shadow glow via `.lit` class
- Coordinates: `y1="285"` (canopy agent bottom) → `y2="521"` (CMC chip top)

### 6. Hover Interaction System

**Orbit agent hover:**
- CMC wrapper gets class `agent-hover` → orange PCB drop-shadow + orange chip box-shadow
- Orbit ring gets class `orbit-glow` (if defined)
- Fixed tooltip appears

**Canopy Agent hover:**
- CMC wrapper gets class `canopy-hover` → **cyan** PCB drop-shadow + cyan chip box-shadow
- `canopyLine1` and `canopyLine2` get class `lit` → glow to full opacity
- JS wired in `DOMContentLoaded` block

### 7. Modal System

Click on any CMC band (Source Truth, Compilation, Retrieval) opens a full-screen modal with detailed content. The Retrieval Band modal includes a RAG explanation paragraph in `#0C447C` mid-navy italic styling. Modal body rendering was fixed: paragraphs starting with `<` use `innerHTML`; others use `textContent`.

### 8. `index-merged.html` — Draft Created

A merged single-scroll HTML was built combining `index.html` (Phases 1 & 2) and `canopyos-architecture.html` (Phase 3):
- **File:** `index-merged.html` in working folder
- **Size:** 1,685 lines
- `index.html` was NOT modified — it remains the live document
- The Phase 3 section is wrapped in `<div id="how-it-works" ...>` with `background: #EEF2F8`
- Architecture CSS is appended to the existing style block (scoped where needed)
- Architecture JS appended before `</body>`
- Nav "How it Works" link is activated (was disabled/greyed in index.html)
- Fonts updated to load Inter weights 300–900
- **Status: Awaiting David's review and approval before replacing index.html**

---

## Decisions Made

1. **Portrait layout (LOCKED):** Arena is 860×1140px portrait. CX=430, CY=700, ORBIT_R=300. Do not change orbit geometry without explicit approval.

2. **Tooltip system (LOCKED):** `position: fixed`, driven by JS `getBoundingClientRect()`. Width 580px. No CSS `overflow: hidden` on arena container (must stay `overflow: visible`).

3. **Click panel disabled:** The click-to-open agent detail panel is removed. Hover tooltip only. The `cmc-agent-panel` div is `display:none`. This decision is locked unless David explicitly reverses it.

4. **Cyan lines are always visible at low opacity (0.45), brighten on Canopy Agent hover.** They are NOT visible/glow on orbit agent hover — only Canopy Agent hover triggers cyan.

5. **CMC glow colors:** Orbit agent hover → orange (`rgba(239,159,39,...)`). Canopy Agent hover → cyan (`rgba(0,204,238,...)`). These are distinct and must not be mixed.

6. **Constraint Agent description:** Starts with "The Constraint Agent vets possible constraints:" — the first thing it does is identify and classify. Source: Turner PCS Constraint Management Instructor Guide v2.2 (2026-03-27) + CanopyOS S2004 architecture session. Reference file was uploaded: `constraint-agent-concept.md`.

7. **index-merged.html is a draft.** Before replacing index.html and pushing, David must review and approve. People are currently viewing the live index.html link.

8. **Constraint Agent** added as a fully defined agent this session (alongside RFI, Change Management, Document agents from the same session). Source material: uploaded PDF (CM Instructor Guide) + constraint-agent-concept.md.

---

## Technical Issues & Resolutions

| Issue | Resolution |
|-------|-----------|
| Tooltips clipped by arena container | Switched to `position: fixed` tooltip; arena CSS changed to `overflow: visible` |
| HTML tags rendering as literal text in modal body | Paragraphs starting with `<` use `p.innerHTML` instead of `p.textContent` |
| `TIP_W` JS constant out of sync with CSS width | Both updated to 580px |
| Orbit ring z-index below PCB board | arena-svg z-index raised to 6 (was 2); cmc-wrapper at 5 |
| CY accidentally pushed to 680 during layout changes | David caught it. Reverted to 620 (now 700 after intentional layout shift). Rule: never change CY without explicit approval |
| Cyan lines not visible (lineGlow filter) | Removed filter entirely; set `opacity="1"` → lines now crisp. Default opacity 0.45 at rest |
| Canopy Agent overlapping orbit top agents | Moved Canopy Agent from 190px to 100px (now 180px after layout shift) |

---

## Document Status at Close

| Section / File | Status | Notes |
|---|---|---|
| `index.html` (live) | ✅ Live / Unchanged | Phases 1 & 2. Do not touch without explicit approval. |
| `canopyos-architecture.html` | ✅ Built / Local | Phase 3 diagram. Not yet in repo. |
| `index-merged.html` | 🟡 Draft / Awaiting Review | Phases 1+2+3 combined. Not pushed. |
| SESSION_S2004_RECAP.md | ✅ Written | This file |
| MEMORY.md | ✅ Updated | S2004 decisions appended |
| `canopyos.html` (duplicate) | ⚠️ Stale | Still in repo. Delete pending. |

---

## S2005 Starting Point

**First-action items for S2005:**

1. **Review `index-merged.html`** — David reviews the merged file in browser. If approved, replace `index.html` with it and push. If redlines needed, address them first.

2. **Complete remaining agent tooltips** — Five agents still need hover content written and confirmed: CIL Agent, Production Agent, Code Agent, Safety Agent, Scheduling Agent. Use the same format as the four completed agents.

3. **Canopy Agent tooltip** — The Canopy Agent box has hover styling but no tooltip content has been written yet.

4. **Push `canopyos-architecture.html` to repo** — This file is built and local but not yet in the GitHub repo.

5. **Compliance Agent decision** — David was leaning toward 7th main agent. Deferred from S2003. Still unresolved.

6. **Delete `canopyos.html` from repo** — Stale duplicate from S2000. Still pending.

**Carry-forward from prior sessions (still open):**
- Fix 2 CLAUDE.md errors: (a) boot sequence path should reference `/RECAPS/` subfolder; (b) `gh_token.txt` convention note
- Create COS-2 sub-issues (~7) and COS-1 sub-issues (2) in Linear
- Optional: custom domain, reconnect GitHub MCP with write token, install Karpathy plugin globally

---

## Live URL Confirmation

**Live:** https://solosevn.github.io/CanopyOS/
**Status:** Phases 1 & 2 only. Phase 3 (`index-merged.html`) is local draft awaiting approval and push.
**Repo:** https://github.com/solosevn/CanopyOS (branch: `main`)

---

*Session S2004 closed May 26, 2026. Next: S2005.*
