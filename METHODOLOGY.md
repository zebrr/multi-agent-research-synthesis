# Multi-Agent Research Synthesis Methodology

Use these phases as project or agent instructions for your model or CLI agent, adapting them to your specific needs.

---

## Phase 1: Research Task Preparation

### Introduction

Input: an unstructured request from a customer (team, stakeholder, or your own hypothesis). Through dialogue with a SOTA model (ChatGPT, Claude, Gemini, ...), the request is transformed into a structured Deep Research task.

The task is designed iteratively: refining scope, formulating criteria, agreeing on tags and templates. The completed task must be self-contained — the research agent works from it without additional explanations.

The structure below can be used as part of project instructions for systematic research design. Each specific task is built according to these criteria, adapting to the domain and objective.

### 1.1. Context and Objective

**Agent Role** — 1-2 sentences, focus and expertise.

**Research Objective** — what we're researching and why. This is the key section that determines everything else: inclusion/exclusion criteria, tag selection, card focus. The objective formulation should give the agent a clear understanding of which cases are relevant and which are noise.

**Constraints:**
- Geography (focus + what to include with caution)
- Time period (launch/update year)
- Language of sources and final report

### 1.2. Scope

**What We Include** — types of research objects with examples. Specificity helps the agent calibrate boundaries.

**What We Do NOT Include** — explicit exclusions with examples. Without this, the agent will bring in adjacent noise.

**Exceptions to Exclusions** — when an object from "do not include" is still included (e.g., "ChatGPT — only if there's institutional deployment with measured results").

### 1.3. Eligibility (Case Selection Criteria)

Minimum requirements — all must be met simultaneously:
- Matches scope
- Time period
- Scale (not a one-off experiment)
- Sources: ≥2, of which ≥1 P1

**Negative cases (paused/discontinued)** — include if:
- Other criteria are met
- Reason for stopping is publicly documented

### 1.4. Tags and Scales

Format as tables — better readability, agent follows more precisely.

**Solution Type** — domain-specific, 5-8 tags maximum. More = agent gets confused.

**Other Classifiers** — as needed for the domain (audience, deployment model, payment model, technologies, etc.).

**R0-R3 (Results Maturity)** — universal scale:

| Tag | Description |
|-----|-------------|
| R0 | Announcement only, no data on progress/results |
| R1 | Qualitative data: reviews, effects described in words |
| R2 | Quantitative data without rigorous methodology (no control group, self-reported) |
| R3 | Rigorous data: control group, longitudinal, peer-reviewed, or very strong public evidence base |

**Status** — optional, but proves universally useful:

| Tag | Description |
|-----|-------------|
| ACT | Active — operational |
| PAU | Paused — suspended |
| DIS | Discontinued — closed |

Tags and scales must be documented in a Legend section in the research task.

### 1.5. Source Policy

**Categories:**

| Tag | What It Includes |
|-----|------------------|
| P1 | Official pages, press releases, case studies, peer-reviewed |
| P2 | Media, interviews, reviews, expert blogs |
| P3 | Unverified posts, forums, reviews |

**Requirements:**
- Minimum per case: ≥2 sources, of which ≥1 P1
- URL required (full, not shortened to domain)
- URL verification: try fetch → if blocked, search by URL → if unsuccessful, add `[unverified]` label
- `[unverified]` — acceptable, source is not discarded, will be verified separately during normalization

### 1.6. Output Format

**Document Format: Markdown (.md)**

**Report Structure:**
1. **Legend** — all tags and scales with explanations (copy from task)
2. **Methodology** — how searched, which queries, limitations
3. **Casebook** — cases C01...Cxx by template
4. **Observed Patterns** — patterns with references to [Cxx] and [Sxx], no conclusions or recommendations
5. **Appendix — Sources** — source list in [Sxx] format

### 1.7. Card Template (Casebook)

Each case gets a unique ID: **C01, C02, ... Cxx**

```markdown
### [Cxx] — [Name]: [Brief Description]

**Basic Information** // Required per research criteria
- Name: ...
- Region: ...
- Launch Year: ...
- Status: ACT/PAU/DIS — [explanation]

**Classification** // Required per research criteria
- Solution Type: [TAG] — [explanation in words]
- [Other tags]: [TAG] — [explanation]
- Results Maturity: [R0-R3] — [explanation]

**What It Is / How It Works** // Fields as needed
[2-3 paragraphs]

**Results and Metrics** // Fields as needed
[1-2 paragraphs]

**Sources** // Required!
[S001], [S002], ...
```

All tags are accompanied by verbal explanations directly in the card.

### 1.8. Observed Patterns

Patterns the agent noticed "while fresh" after processing all cases.

**Requirements:**
- Each pattern is supported by references to cases [Cxx] and/or sources [Sxxx]
- Observations only, no conclusions or recommendations
- `Inference` (agent's own conclusions) marked explicitly

### 1.9. Appendix — Sources

List of all sources:

```
[S001]: P1, https://full.url/path/to/page, "Official Case Study Title"
[S002]: P2, https://another.url/article
...
[S103]: P1 [unverified], https://blocked.site/report, "Annual Report 2024"
```

Title is optional, but if present — in quotes after URL.

### 1.10. Quality Control

Checklist before delivery:
- [ ] All tags match Legend
- [ ] All cases have C-ID in [Cxx] format
- [ ] All facts/metrics have [Sxxx]
- [ ] All [Sxxx] from text exist in Appendix in specified format
- [ ] `Inference` marked explicitly
- [ ] URLs are complete (not shortened to domain)

---

## Phase 2: Multi-Agent Collection

- One task → loaded into several (3+) different agents via available method (app, API)
- Primary: OpenAI (Agent and DeepResearch — two different ones!), Claude Research, Gemini DeepResearch
- Optional: Perplexity Labs, Qwen DeepResearch, Kimi Researcher, Manus Wide Research, ...
- Each works independently
- Results: ~50-200KB per agent, multiple documents
- Quality and structure vary: agents may ignore parts of the task, use their own tag/reference formats, add unrequested sections — this is normal and accounted for in the methodology

---

## Phase 3: Preparation and Structural Normalization

**Goal:** bring raw agent reports to a unified structure for subsequent reference work and synthesis.

- Files are large (50-200KB) → work via bash/grep, don't read entirely
- Strictly one file at a time, context reset between files
- Task (TASK.md) — the reference: compare each file's structure and tags against task requirements

### Agent Coding

Use standard abbreviations for WRK file naming and mapping:

| Code | WRK File |
|------|----------|
| Claude | WRK_Claude_Research.md |
| Gemini | WRK_Gemini_DeepResearch.md |
| OAI_DR | WRK_OpenAI_DeepResearch.md |
| OAI_Ag | WRK_OpenAI_Agent.md |
| Perplx | WRK_Perplexity_Labs.md |
| Qwen | WRK_Qwen_Research.md |
| Kimi | WRK_Kimi_Researcher.md |
| Manus | WRK_Manus_Wide_Research.md |

If the research uses an agent not on the list — add to report with analogous format.

### 3.1. Creating Working Copies

- Create `_work` folder in the research directory
- Copy all agent reports (except the task) as `WRK_<original_name>.md`
- Primary files remain untouched (READONLY at all subsequent stages)
- Create `NORMALIZATION_REPORT.md` with template (see 3.5)

### 3.2. Initial Audit

For each file — status assessment:

| Criterion | What to Check |
|-----------|---------------|
| Structure | Are sections from the task present? In what order? |
| Cards | Is there a Casebook? How many cases? |
| Tags | Codes or descriptions? What format? |
| References | Appendix, inline, footnotes? How many unique? |
| Language | Matches task? |

**Result:** completed summary table in NORMALIZATION_REPORT.md with qualitative assessment of each file.

**Qualitative Assessment** — conclusion per file:
- ✅ Ready for normalization — structure close to task
- ⚠️ Requires significant work — structure far from task, but data is valuable
- ❌ Recommended to use only as case source — format incompatible

Final decision on files with ⚠️ and ❌ ratings is made by the user.

### 3.3. Structural Normalization

For each file (one at a time, sequentially):

**Checklist:**

- [ ] Sections match "Output Format" from task
- [ ] Misplaced sections → rearrange
- [ ] Missing sections → create with label `<!-- MISSING: Data not provided by agent -->`
- [ ] Unique sections (not in task) → move to end with label `<!-- UNIQUE: use at Cross-synthesis stage -->`
- [ ] No Appendix with references (inline format) → mark `<!-- MISSING: Appendix Sources — references inline, restore in Phase 4 -->`
- [ ] Tags in cards: where descriptions instead of codes → add codes, where impossible to determine → `[TAG:UNKNOWN]`
- [ ] Tag format is uniform within file (for correct processing during synthesis)
- [ ] Legend removed from file (if present)

**After processing file:**
- Update details in NORMALIZATION_REPORT.md
- Reset context
- Move to next file

### 3.4. Quality Check

After processing all files — final verification:

- [ ] All files from summary table processed
- [ ] Section structure matches task (or marked MISSING/UNIQUE)
- [ ] Tags in code format (or marked TAG:UNKNOWN)
- [ ] Legends removed from all files
- [ ] NORMALIZATION_REPORT.md is current

### 3.5. Output Artifacts

**`_work` folder:**
- `WRK_*.md` — structurally normalized files
- `NORMALIZATION_REPORT.md` — report (created when processing first WRK file, updated for others)

**NORMALIZATION_REPORT.md Template:**

```markdown
# Normalization Report: [Research Title]

## Structure Statistics (Phase 3)

| File | Rating | Cards | References | Issues |
|------|--------|-------|------------|--------|
| WRK_*.md | ✅/⚠️/❌ | N | N | MISSING: ..., UNIQUE: ... |

## File Details

### WRK_[Agent].md
- **Rating:** ✅/⚠️/❌ — rationale
- **Structure:** matches / corrected (what exactly)
- **Tags:** format, normalization status
- **References:** format (Appendix / inline / footnotes), count
- **Unique sections:** list or "none"
- **Language:** RU / EN
- **Notes:** file specifics
```

---

## Phase 4: Reference Normalization

**Goal:** bring references in each WRK file to a unified format, verify all references are used and all statements are attributed.

**Principle:** one WRK file at a time, isolated, context reset between files (as in Phase 3).

### 4.1. Inventory

Extract two sets:

**T (text)** — all unique URLs from file text:
- inline markdown `[name](url)`
- footnotes `[^1]: url`
- bare URLs
- references to existing Appendix `[Sxxx]` → get URL by ID

**A (Appendix)** — all URLs from Appendix (if exists):
- If MISSING (marked in Phase 3) → A = ∅

**Result:** two URL lists for comparison.

### 4.2. Appendix Normalization

Compare T and A:

| Case | Condition | Action |
|------|-----------|--------|
| **MISSING** | A = ∅ (Appendix was not created by agent) | Create Appendix from T |
| **Incomplete** | T ⊄ A (in text, not in Appendix) | Add missing entries |
| **Excessive** | A ⊄ T | Handle in step 4.3 |
| **Complete** | T = A | Do nothing |

**Entry format:** `[Sxxx]: Px, URL, "name"` (name optional)

**S-ID assignment:** sequential numbering S001, S002, ... for new entries. If entries exist — continue numbering.

### 4.3. Conversion and Unused Check

**Conversion:**
- Replace all inline/footnotes/bare URLs in text with `[Sxxx]`
- Clean URLs of garbage anchors (`#:~:text=...`, `#section`), but DO NOT shorten to domain

**Duplicate handling:**
- If after anchor cleanup two URLs become identical → keep one S-ID
- Delete second S-ID from Appendix
- Both places in text get same S-ID
- DO NOT rebuild numbering (gaps like S001, S003, S004 — ok, will fix at mapping stage)

**Unused check:**
- Extract set of S-IDs from text (used)
- Compare with set of S-IDs in Appendix
- Difference = `[unused]` → mark in Appendix

**Quality control:**
- [ ] No bare URLs in text (all replaced with `[Sxxx]`)
- [ ] All `[Sxxx]` from text exist in Appendix (no dangling)
- [ ] No URL duplicates in Appendix (one URL = one S-ID)

### 4.4. Source Verification

**Resolving [unused]:**
- Delete from Appendix (source not used in text) and update NORMALIZATION_REPORT.md

**Resolving [unverified] (if such marks exist):**
1. `web_fetch` URL
2. If blocked → `web_search` by URL + keywords from context (possibly in English)
3. Successfully verified → remove label
4. Failed → leave `[unverified]`, update NORMALIZATION_REPORT.md

### 4.5. Attribution Check

Check: are there statements with numbers/metrics in text without `[Sxxx]`.

Semantic search: by keywords (effect size, %, year, name, ...), reading range, analysis.

If gaps found → record in NORMALIZATION_REPORT.md (don't fix, just document).

### 4.6. Output Artifacts

**Checklist for each WRK file:**
- [ ] Appendix in `[Sxxx]: Px, URL, "name"` format
- [ ] No inline URLs in text
- [ ] No dangling references
- [ ] No URL duplicates in Appendix
- [ ] No `[unused]` (deleted)

**Files:**
- `WRK_*.md` — with normalized references and Appendix
- `NORMALIZATION_REPORT.md` — updated

**Add to NORMALIZATION_REPORT.md table:**

```markdown
## Reference Statistics (Phase 4)

| File | Format | Sources | Deleted (unused) | Unverified |
|------|--------|---------|------------------|------------|
| WRK_*.md | ... | N | X | Y |

### Deleted References [unused]

| File | S-ID | URL |
|------|------|-----|
| WRK_Agent1.md | S045 | https://... |
| WRK_Agent2.md | S012 | https://... |

### Sources [unverified]

| File | S-ID | URL | Used In |
|------|------|-----|---------|
| WRK_Agent1.md | S023 | https://... | C05, Observed Patterns |

### Statements Without Attribution

| File | Statement | Context |
|------|-----------|---------|
| WRK_Agent2.md | "g = 0.68" | Observed Patterns, line ~120 |
```

---

## Phase 5: Source Re-Attribution (Optional)

> ⚠️ **Optional phase.** Does not formalize well, requires design for specific case.

**When needed:**

If Phase 4 results for a WRK file show:
- Many deleted `[unused]` references (>30% of original Appendix)
- Significant portion of text without attribution (many entries in "Statements Without Attribution")

This questions the source's value. User decides:
- **Re-attribute** — restore links between text and sources (before synthesis!)
- **Decline** — exclude WRK file from synthesis

### 5.1. Generate JSON with Claims

Semantically (reading in parts) extract all unattributed statements:
- Exact quote (can take long chunk for context)
- Exact line number in WRK file
```json
{
  "claims": [
    {
      "text": "effect size g = 0.68 for adaptive learning interventions",
      "line": 234,
      "source": null
    }
  ]
}
```

### 5.2. Verify Claims and Enrich JSON

For each claim:
1. `web_search` by statement content
2. If source found → record in JSON: URL, type (P1/P2/P3), name
3. If not found → leave `source: null`

Note: claims may be in RU, sources 95% in EN.
```json
{
  "claims": [
    {
      "text": "effect size g = 0.68 for adaptive learning interventions",
      "line": 234,
      "source": {
        "url": "https://...",
        "type": "P1",
        "name": "SRI Study 2020"
      }
    }
  ]
}
```

### 5.3. Update WRK File (per Phase 4 Rules)

- Use enriched JSON with claims
- Add found sources to Appendix
- Attribute claims in text `[Sxxx]` by line numbers from JSON

### 5.4. Update NORMALIZATION_REPORT.md

Add section (don't touch previous):

```markdown
## Re-Attribution Statistics (Phase 5, Optional)

| File | Total Claims | Attributed | Remaining Without Source |
|------|--------------|------------|--------------------------|
| WRK_Agent1.md | 15 | 12 | 3 |
```

**Output artifacts:**
- `_work/_claims_[AgentCode].json` — working file with claims and found sources (agent codes see Phase 3)
- `WRK_*.md` — updated with attribution
- `NORMALIZATION_REPORT.md` — supplemented

---

## Phase 6: Multi-Pass Synthesis

### 6.1. Core Synthesis Rules

**Working directory:** all work is done only in `_work` folder. Original agent files — READONLY at all synthesis stages.

**Target state:** by end of synthesis all WRK files should be "empty" (only usage labels) — this indicates processing completeness.

### Large File Tactics

Combined WRK file volume usually doesn't fit in context entirely. After several synthesis stages, SYNTHESIS.md itself also becomes too large.

**Reading strategy:**
- DO NOT read WRK files entirely — use `grep`/`search` to extract needed data
- Read files in parts: `head -N`, `tail -N`, `sed -n 'X,Yp'`
- Use `grep -A N` / `grep -B N` for context around found items
- When working with cards — extract one at a time via grep by case ID

**Writing strategy:**
- Update SYNTHESIS.md incrementally — replace TODO labels with content
- After each stage update execution report
- Delete processed data from WRK files (details — at specific stages)

**Typical commands:**
```bash
# Find all case cards
grep -n "^### C" _work/WRK_*.md

# Extract card by ID (with N lines context)
grep -A 50 "^### C01" _work/WRK_*.md

# Find all URLs
grep -oE 'https?://[^)>\s]+' _work/WRK_*.md

# Find UNIQUE sections (marked during normalization)
grep -n "<!-- UNIQUE" _work/WRK_*.md
```

### 6.2. Scaffold Document and Synthesis Artifacts

#### SYNTHESIS.MD Scaffold Document

```markdown
# [Research Title]

Synthesis of `XX` studies. <-- specify how many studies -->

Audience: <-- TODO: add after SUMMARY.MD generation -->
Report Purpose: <-- TODO: add after SUMMARY.MD generation -->
Date: <-- TODO: add after completing work on SYNTHESIS.MD -->
Sources: <-- list agents by full names, not codes -->

## Research Objective
<!-- TODO: transfer from TASK.md when creating scaffold -->

## Legend
<!-- TODO: transfer from TASK.md when creating scaffold -->

## Methodology

### Approach
<!-- TODO: describe methodology, criteria, final numbers (Phase 7) -->

### Reference Files
- [NORMALIZATION_REPORT.md](./_work/NORMALIZATION_REPORT.md)
- [_case_references.json](./_work/_case_references.json) <!-- after case mapping 6.3.1  -->
- [_sources_references.json](./_work/_sources_references.json) <!-- after source mapping 6.3.2 -->


### Limitations
<!-- TODO: identified limitations on languages, coverage, validations (Phase 7) -->

## Agent Comparison

### Quantitative Analysis
<!-- TODO: agent comparison tables after mappings see 6.3.3 (cases, sources, P1%, regions) -->

### Qualitative Analysis
<!-- TODO: agent evaluation based on cross-synthesis, strengths/weaknesses, recommendations (Phase 7) -->

## Casebook
<!-- 
Synthesized case cards.
May be divided by regions or other criterion from TASK.md.
-->

## Observed Patterns
<!-- 
Section structure is determined by research domain.
Possible grouping axes:
- By tags/classifiers from TASK.md (solution type, audience, model)
- By regions (if geographic specificity exists)
- By stages (adoption, scaling, barriers)
-->

## Negative Cases
<!-- 
Cases with PAU/DIS status and failure patterns.
What doesn't work, why, failure timeline, insights at agent's discretion.
-->

## Key Research Questions
<!-- 
Sections per questions from TASK.md.
Each question — separate subsection with:
- Direct answer with attribution [Cxx] [Sxxx]
- Details / nuances
- Evidence level (R0-R3)
- Important/additional insights at agent's discretion
-->

## Metrics Framework
<!-- 
Optional — if cases contain R2/R3 metrics.
Grouping by metric types (outcomes, engagement, business).
-->

## Contradictions and Verification
<!-- 
Structure as needed:
- Resolved contradictions (indicating how resolved)
- Unresolved contradictions (require additional verification)
- Research methodological limitations
- Important/additional insights at agent's discretion
-->

## Appendix — Index of Sources
<!-- TODO: final index, generated from _sources_references.json in Phase 7 -->
```

#### Case Reference File

JSON reference file is created during case mapping and used at all subsequent stages:

**`_case_references.json`** — master case list:
```json
{
  "C01": {
    "short": "DreamBox",
    "full": "DreamBox Learning: Adaptive Math K-8",
    "cite": "DreamBox [C01]",
    "status": "ACT",
    "R": "R2",
    "agents": {
      "Claude": "C03",
      "Gemini": "C01",
      "OAI_DR": "C05"
    },
    "sources": []  // filled in 6.5.1
  }
}
```

#### Source Reference File

JSON reference file is created during source mapping and used at all subsequent stages:

**`_sources_references.json`** — master source list:
```json
{
  "S001": {
    "url": "https://...",
    "type": "P1",
    "name": "SRI Study 2020", // optional, simply absent if no name
    "agents": {
      "OAI_DR": "S025",
      "Perplx": "S027"
    }
  },
  "S002": {
    "url": "https://...",
    "type": "P2",
    "agents": {
      "Claude": "S103"
    }
  }
}
```

### 6.3 Reference File Preparation

#### 6.3.1. Case Mapping

**Goal:** build unified case reference with deduplication and mapping to original agent IDs.

**Input data:** case cards from all WRK files (Casebook sections).

**Actions:**
1. Extract all cards from WRK files (`grep -n "^### C" _work/WRK_*.md`)
2. Deduplicate by entity (one product/organization = one case)
3. Assign final IDs: C01, C02, ..., Cxx
4. For each case record:
   - `short`, `full`, `cite` — names for use in text
   - `status` — from card (ACT/PAU/DIS)
   - `R` — results maturity level (R0-R3)
   - `agents` — original ID mapping by agents
5. Save to `_work/_case_references.json` (format see 6.2)
   - `short` — 1–2 key words (sometimes 3 if preposition/article)
   - `sources` — filled later, at card synthesis stage (6.5.1)

**Verification:**
- [ ] All cases from all WRK files accounted for
- [ ] No duplicates by entity
- [ ] Each case has status and R

**After completing mapping:** add case reference link to SYNTHESIS.md (Methodology → Reference Files section)

#### 6.3.2. Source Mapping

**Goal:** build unified source reference with mapping to original agent IDs.

**Input data:** Appendix from all WRK files (normalized in Phase 4).

**Actions:**
1. Extract all entries from Appendix of all WRK files
2. Merge by URL — one URL = one entry, different original IDs in `agents`
3. Assign final IDs: S001, S002, ...
4. For each source record:
   - `url`, `type` (P1/P2/P3), `name`
   - `agents` — original ID mapping by agents
5. Save to `_work/_sources_references.json` (format see 6.2)
6. Verify mapping selectively (while Appendix still exists, before step 7)
7. Delete Appendix from WRK files, adding label `<!-- REMOVED: Appendix → _sources_references.json -->`

**Verification:**
- [ ] All sources from all Appendixes accounted for
- [ ] No URL duplicates
- [ ] Appendix in WRK files replaced with REMOVED labels

**After completing mapping:**
- Add source reference link to SYNTHESIS.md (Methodology → Reference Files section)

#### 6.3.3. Quantitative Agent Analysis

**Goal:** build agent comparison tables based on reference files.

**Input data:** `_case_references.json`, `_sources_references.json`

**Actions:**

1. **Cases and Sources Table:**

| Agent | Cases | Unique | Sources | P1 | P2 | P3 | P1% |
|-------|-------|--------|---------|----|----|----|----|

- Cases — how many times agent is mentioned in `_case_references.json`
- Unique — cases where only this agent is in `agents`
- Sources / P1 / P2 / P3 — from `_sources_references.json`

2. **Regional Distribution** (if applicable):

| Agent | USA | Europe | Asia | Global |
|-------|-----|--------|------|--------|

3. **Notes** — textual conclusions:
- Leader by cases
- Leader by unique finds
- Leader by source volume
- Leader by source quality (P1%)
- Regional focus (if any)

4. **Key Patterns** — brief characterization of each agent (1-2 lines)

**Result:** completed "Agent Comparison → Quantitative Analysis" section in SYNTHESIS.md

**Verification:**
- [ ] Table numbers match reference file totals
- [ ] All used agents are represented

### 6.4. Attribution Rules

All references in SYNTHESIS.md use final IDs from reference files:
- Cases: `_case_references.json` → C01, C02, ...
- Sources: `_sources_references.json` → S001, S002, ...

#### Reference Formats

| Type | Format | Example |
|------|--------|---------|
| Case | `Short [Cxx]` | `DreamBox [C01]` |
| Source | `[Sxxx]` | `[S001]` |
| Multiple | `Short [Cxx], [Sxxx]` | `DreamBox [C01], [S042]` |

#### Hierarchy: Case vs Source

- Case — generalized entity with sources inside
- Reference to `[Cxx]` covers all `sources` of that case
- Separate source `[Sxxx]` — only if it's NOT in any case

**Don't duplicate:** if statement is confirmed by case `[C01]`, don't add `[S001]` if S001 ∈ C01.sources.

#### What to Attribute by Section

| Section | Attribution |
|---------|-------------|
| Objective, Legend, Methodology (except Qualitative Analysis), Appendix | Not required |
| Agent Comparison → Qualitative Analysis | Cases and/or sources |
| Casebook (cards) | Only sources `[Sxxx]` |
| Observed Patterns | Cases and sources (if not in case) |
| Negative Cases | Cases and sources (if not in case) |
| Key Research Questions | Cases and sources (if not in case) |
| Metrics Framework | Cases and sources (if not in case) |
| Contradictions and Verification | Cases and sources (if not in case) |

#### What to Always Attribute

- Numbers, metrics, statistics
- Quotes
- Specific facts (dates, names, events)
- Causal statements

#### Own Conclusions

Inference (agent's conclusions not backed by sources) — mark explicitly: `[Inference]`

#### Recording Contradictions

All contradictions recorded with attribution of both positions:
```
Per DreamBox [C01]: effect was 15%. Per [S042]: effect did not exceed 8%.
```

Mark `<!-- VERIFY: brief description -->` for verification at verification stage.

#### What Should NOT Be in Final Text

- Original agent IDs — in reference files
- Bare URLs
- References `[Cxx]` without Short name
- Duplication of sources included in case

### 6.5. Synthesis Stages

#### 6.5.1. Card Synthesis

**Goal:** merge data for each case from all WRK files into one synthesized card.

**Input data:**
- `_case_references.json` — case reference with agent mapping
- `_sources_references.json` — source reference
- WRK files with normalized cards

**Algorithm (one case at a time):**

1. Take case from reference (C01, C02, ...)
2. By `agents` mapping find original cards in WRK files
3. Extract cards: `grep -A 50 "^### C03" WRK_Claude.md` etc.
4. Merge data:
   - Basic information — take most complete version
   - Classification — compare tags, record discrepancies
   - Description — synthesize, remove duplicates
   - Results/metrics — collect all unique
5. Re-attribute sources:
   - Take original S-ID from agent card (e.g., S025 from WRK_OpenAI_DeepResearch.md)
   - Find in _sources_references.json entry where `agents.[AgentCode]` equals this ID (e.g., `agents.OAI_DR: "S025"`)
   - Replace original ID with found entry's key — this is the final S-ID (e.g., S001)
6. Update `_case_references.json`:
   - Fill `sources` field for this case `"sources": ["S001", "S003", ...]` etc.
7. Write card to SYNTHESIS.md (Casebook section)
8. Delete used cards from WRK files, adding label `<!-- REMOVED: [Cxx] → SYNTHESIS.md/Casebook -->`

**Card format:** see TASK.md, section 1.7 "Card Template"

**Verification after each card:**
- [ ] All sources re-attributed (no original agent IDs)
- [ ] `sources` in reference updated
- [ ] Cards in WRK files replaced with REMOVED labels

**Verification after all cards:**
- [ ] All cases from reference processed
- [ ] Casebook sections in WRK files contain only REMOVED labels

#### 6.5.2. Cross-Synthesis

**Goal:** synthesize patterns, answer key research questions, identify and record contradictions.

**Input data:**
- SYNTHESIS.md with completed Casebook
- `_case_references.json` with filled `sources`
- `_sources_references.json`
- WRK files: Observed Patterns sections, unique sections (`<!-- UNIQUE -->`)

**Sections to complete:**
- Observed Patterns
- Negative Cases
- Key Research Questions
- Metrics Framework (if there are R2/R3 cases)
- Contradictions and Verification

**Algorithm:**

1. **Collect material from WRK files:**
   - Extract Observed Patterns from all agents
   - Extract unique sections (marked `<!-- UNIQUE -->`)
   - Key questions list from TASK.md

2. **Pattern validation:**
   - Pattern from 2+ agents → accept
   - Pattern from one agent → verify against synthesized cards in Casebook
   - Discrepancy between agents → record in "Contradictions" section

3. **Synthesize by section (one at a time):**

   **Observed Patterns:**
   - Group by axes from TASK.md (solution type, region, stage...)
   - Attribute: cases `Short [Cxx]`, sources `[Sxxx]` if not in case
   - Add own insights if identified during synthesis — mark `[Inference]`
   - Delete processed patterns from WRK, adding `<!-- REMOVED: Observed Patterns → SYNTHESIS.md/Observed Patterns -->`

   **Negative Cases:**
   - Collect cases with PAU/DIS status from Casebook
   - Add failure patterns from agents' Observed Patterns
   - Highlight common failure causes if traceable — mark `[Inference]`
   - Attribute per rules 6.4

   **Key Research Questions:**
   - Each question from TASK.md → separate subsection
   - Direct answer with attribution
   - Evidence level (R0-R3)
   - Additional insights on question if any — mark `[Inference]`

   **Metrics Framework:**
   - Only if there are R2/R3 cases
   - Group by metric types
   - Attribute metric sources

   **Contradictions and Verification:**
   - Collect all recorded contradictions
   - Add non-obvious discrepancies identified during synthesis
   - Research methodological limitations

4. **Record contradictions:**
   - Format: "Per Short [Cxx]: A. Per [Sxxx]: B."
   - All contradictions → `<!-- VERIFY: description -->`

5. **Delete from WRK:**
   - After processing each section delete used material
   - Label: `<!-- REMOVED: [name] → SYNTHESIS.md/[section] -->`

**Verification after cross-synthesis:**
- [ ] All sections from list completed (or marked N/A)
- [ ] All patterns attributed per rules 6.4
- [ ] Own insights marked `[Inference]`
- [ ] Contradictions recorded with `<!-- VERIFY -->`
- [ ] Observed Patterns sections in WRK files contain only REMOVED labels
- [ ] Unique sections in WRK files contain only REMOVED labels

### 6.6. Phase Output Artifacts

**Ready artifacts:**

| Artifact | Status |
|----------|--------|
| `SYNTHESIS.md` | Completed: Objective, Legend, Methodology, Casebook, Observed Patterns, Negative Cases, Key Questions, Metrics Framework, Contradictions |
| `_case_references.json` | Complete, with filled `sources` |
| `_sources_references.json` | Complete, with agent mapping |
| `NORMALIZATION_REPORT.md` | Final |
| WRK files | Contain only REMOVED labels + Methodology section **NOT DELETED** (processed in Phase 7) |

**What may require refinement:**

- `<!-- VERIFY: ... -->` labels — contradictions for verification in Phase 7
- "Agent Comparison → Qualitative Analysis" section — completed in Phase 7
- "Methodology → Limitations" section — supplemented in Phase 7
- Appendix — Index of Sources — generated from reference in Phase 7

**Attribution:**
- All statements attributed per rules 6.4
- Own conclusions marked `[Inference]`

---

## Phase 7: Validation and Finalization

**Goal:** resolve contradictions, complete remaining SYNTHESIS.md sections, prepare final document without service labels.

### 7.1. Contradiction Verification

**Goal:** resolve all `<!-- VERIFY: ... -->` labels in SYNTHESIS.md.

**Algorithm (one contradiction at a time):**

1. Extract VERIFY label and context
2. Formulate search query (note: synthesis may be in RU/other language, primary sources mostly in EN)
3. Execute `web_search` on contradiction substance
4. Determine outcome:

| Outcome | What Happened | Action |
|---------|---------------|--------|
| **Resolved in favor of one** | Confirmed A, refuted B | Fix text, update sources (add/remove) |
| **Both correct** | Interpretation, incompleteness, about different things | Clarify context in text, sources valid |
| **Both wrong** | Found third version, both sources erroneous | Fix text with new data, add source, mark old as unreliable in report |
| **Unresolvable** | Real domain contradiction, different studies give different results | Record in text as "data diverges" with attribution of both |
| **Unverifiable** | Paywall, site down, not searchable | Escalate to user |

5. Update SYNTHESIS.md: remove VERIFY label, fix/clarify text
6. Update reference files if sources changed:
   - `_sources_references.json` — add new source / mark deleted one
   - `_case_references.json` → `sources` — update case source list
7. Record resolution in NORMALIZATION_REPORT.md

**User escalation** (separate from main flow):

If verification results in case losing sources and no longer meeting criteria (< 2 sources or < 1 P1):
- Pause processing this contradiction
- Record in escalation table
- Wait for user decision: downgrade R / keep with caveat / delete case

**Add to NORMALIZATION_REPORT.md:**

```markdown
## Contradiction Verification (Phase 7)

| Contradiction | Outcome | Resolution | Source Changes |
|---------------|---------|------------|----------------|
| [brief description] | Resolved / Both correct / Both wrong / Unresolvable / Unverifiable | [what was done] | [added Sxxx / removed Sxxx / —] |

### Require User Decision

| Contradiction | Escalation Reason | Options |
|---------------|-------------------|---------|
| ... | Case C0x loses P1 source | Downgrade to R1 / Keep with caveat / Delete case |
| ... | Could not verify | Accept version A / B / Keep both |
```

**Verification:**
- [ ] No `<!-- VERIFY -->` labels in SYNTHESIS.md
- [ ] All resolutions recorded in NORMALIZATION_REPORT.md
- [ ] Escalations passed to user and resolved

### 7.2. Methodology → Approach

**Goal:** transfer agent search methodology from WRK files to SYNTHESIS.md.

**Actions:**

1. Extract Methodology sections from all WRK files
2. Compare agent approaches:
   - If similar → generalize in 2-3 sentences
   - If significantly different → bullet list by agent with key differences
3. Write to SYNTHESIS.md → Methodology → Approach
4. Delete Methodology from WRK files, add `<!-- REMOVED: Methodology → SYNTHESIS.md -->`

**What to record:**
- Types of search queries
- Search languages
- Regional/source focus (if explicit)
- Approach specifics (if agent indicated)
- Other facts at agent's discretion (domain-specific)

**Verification:**
- [ ] "Approach" section completed
- [ ] Methodology in WRK files replaced with REMOVED

### 7.3. Methodology → Limitations

**Goal:** document research methodological limitations.

**What to include:**
- Language bias (source distribution by language, underrepresented regions)
- Coverage gaps (solution types / regions with few cases)
- Data quality (share of self-reported, lack of control groups, R0-R3 distribution)
- Other limitations at agent's discretion (domain-specific)

**Actions:**

1. Analyze distribution in `_case_references.json` and `_sources_references.json`
2. Identify gaps and bias
3. Write to SYNTHESIS.md → Methodology → Limitations

**Verification:**
- [ ] "Limitations" section completed
- [ ] Specific numbers where possible (X% sources in EN, Y cases from region Z)

### 7.4. Qualitative Agent Analysis

**Goal:** summarize experience working with agents, provide usage recommendations.

**Input data:**
- Quantitative analysis (tables from 6.3.3)
- Verification experience (7.1) — who gave accurate data, who had more errors
- Synthesis experience (6.5) — unique finds, card quality

**What to record per agent (1-3 sentences):**
- Strengths (regions, source types, depth)
- Weaknesses (omissions, errors, bias)
- Usage recommendation

**Actions:**

1. Summarize observations on agents
2. Write to SYNTHESIS.md → Agent Comparison → Qualitative Analysis

**Verification:**
- [ ] All agents from research characterized
- [ ] Recommendations specific, not generic phrases

### 7.5. Appendix — Index of Sources

**Goal:** generate final bibliography from source reference.

**Actions:**

1. Load `_sources_references.json`
2. Generate in format:
```markdown
## Appendix — Index of Sources

[S001]: P1, https://full.url/path, "Title if exists"
[S002]: P2, https://another.url
...
```
3. Write to SYNTHESIS.md → Appendix — Index of Sources

**Verification:**
- [ ] All S-IDs from reference included
- [ ] Format uniform

### 7.6. Quality Control

**Goal:** ensure SYNTHESIS.md is final and WRK files are empty.

**SYNTHESIS.md checklist:**
- [ ] Date filled in
- [ ] No `<!-- TODO -->` labels
- [ ] No `<!-- VERIFY -->` labels
- [ ] No service comments (except possible `[Inference]`)
- [ ] All `[Cxx]` exist in `_case_references.json`
- [ ] All `[Sxxx]` exist in `_sources_references.json`
- [ ] No dangling references (ID mentioned but not in reference)
- [ ] Formatting consistent (headings, tables, lists)

**WRK file checklist:**
- [ ] All sections replaced with `<!-- REMOVED: ... -->`
- [ ] No content except labels

**Reference file checklist:**
- [ ] `_case_references.json` — all `sources` filled
- [ ] `_sources_references.json` — no URL duplicates

**Actions on errors:**
- Dangling reference → find in reference or remove from text
- Duplicate in reference → merge, update references in text
- Non-REMOVED content in WRK → escalate to user

### 7.7. Phase Output Artifacts

| Artifact | Status |
|----------|--------|
| `SYNTHESIS.md` | Final, all sections completed, no service labels |
| `WRK_*.md` | Only REMOVED labels |
| `_case_references.json` | Final |
| `_sources_references.json` | Final |
| `NORMALIZATION_REPORT.md` | Supplemented with verification section |

**Readiness for Phase 8:**
- [ ] All 7.6 checklists passed
- [ ] User escalations resolved
- [ ] SYNTHESIS.md ready for SUMMARY.md generation

---

## Phase 8: Executive Summary and Finalization

**Goal:** transform SYNTHESIS.md into SUMMARY.md — a condensed document for target audience decision-making.

### 8.1. Defining Focus

Focus is determined either from research design TASK.md or through dialogue with user.

Before starting, record:
- **Audience** — who reads (role, level)
- **Purpose** — what decision they make based on report

Focus determines what goes to top, what gets shortened, which details to preserve.

**Focus examples:**

| Audience | Purpose | Emphasis |
|----------|---------|----------|
| PMO, product | Insights, hypothesis validation | Patterns, mechanics, what works/doesn't work |
| CTO, architect | Solution evaluation | Technologies, integrations, scaling |
| Investor | Due diligence | Metrics, unit economics, risks |
| Researcher | Domain overview | Methodology, gaps, directions |

### 8.2. SUMMARY.md Scaffold

```markdown
# [Research Title]

Synthesis of N studies.
Audience: [audience]
Purpose: [decision reader makes]
Date: YYYY-MM-DD
Sources: <-- list agents by full names, not codes -->

## Research Objective
[from SYNTHESIS, unchanged]

## Legend
[from SYNTHESIS, unchanged]

## Methodology
[condensed: approach + key limitations]

### Agent Comparison
[table: cases, sources, P1%, qualitative assessment]

## Top 10 Cases
[selection criterion determined by focus]

## Top 5 Negative Cases
[what went wrong, why it matters]

## Observed Patterns
[key ones, with attribution]

## Key Research Questions
[answers condensed]

## Metrics Framework
[if R2/R3 data exists]

## Contradictions
[unresolved or critical for decision]
```

### 8.3. Transformation Principles

**What to transfer from SYNTHESIS:**
- Conclusions and patterns — condensed, with attribution
- Methodology — key points, no file details
- Research question answers — briefly

**What to remove:**
- Full Casebook — replaced with top tables
- Appendix Sources — attribution stays inline
- Service sections (references, mappings)
- Normalization details

**Top cases and negative cases:**

Not cards, but tables. One case = one row with key result.

Top cases format:
```markdown
| # | Case | Region | Type | R | Key Result |
|---|------|--------|------|---|------------|
| 1 | Short [Cxx] | ... | TAG | Rx | What makes it top + [Sxxx] |
```

Negative cases format:
```markdown
| # | Case | Region | Failure Reason | Lesson |
|---|------|--------|----------------|--------|
| 1 | Short [Cxx] | ... | What went wrong | Takeaway for reader |
```

After negative cases table — failure patterns (3-5 points, summary).

**Top case selection criterion:**
- Determined by research focus and audience
- State explicitly above table: *"Criterion: R3 first, then R2 with largest reach"*

**Agent freedom:**
- Agent determines which patterns are key for this audience
- Agent may add sections relevant to focus
- Agent may change section order for reading logic
- Agent adds insights considered important for reader

### 8.4. Formatting

**Attribution:**
- Cases: `Short [Cxx]` — always with name
- Sources: `[Sxxx]` — inline, no separate Appendix
- Inference: mark `(Inference: N=X)` if N<3

**Language:**
- Terms — explanation on first use
- If report in RU/other language — minimize anglicisms, use local equivalents

### 8.5. Checklist

- [ ] Audience and purpose recorded in header
- [ ] Objective and Legend transferred from SYNTHESIS
- [ ] Top 10 and Top 5 selected by audience-relevant criterion
- [ ] Selection criterion stated explicitly
- [ ] Failure patterns summarized after negative cases
- [ ] Attribution preserved
- [ ] Inference marked
- [ ] Language clean, terms explained
