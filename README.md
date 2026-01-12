# Multi-Agent Research Synthesis

[![License: CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/)
![Type: AI Skill](https://img.shields.io/badge/Type-AI%20Skill-purple)
![Docs: EN | RU](https://img.shields.io/badge/Docs-EN%20%7C%20RU-blue)

A methodology for conducting comprehensive research using multiple AI research agents, with a CLI AI agent orchestrating phased synthesis of results (AI-skill).

---

## Table of Contents

- [The Problem](#the-problem)
- [The Solution](#the-solution)
- [What This Methodology Solves](#what-this-methodology-solves)
- [Division of Responsibilities](#division-of-responsibilities)
  - [Actor 1: Designer](#actor-1-designer-conversational-model)
  - [Actor 2: Research Agent](#actor-2-research-agent-deep-research-agents)
  - [Actor 3: Synthesizer](#actor-3-synthesizer-cli-agent-with-bash-access)
- [Tasks for the CLI Agent](#tasks-for-the-cli-agent-actor-3)
- [Methodology Principles](#methodology-principles)
- [Methodology](#methodology)
- [How to Use](#how-to-use)
- [License](#license)

---

## The Problem

Different research agents produce results of varying volume and quality due to:
- different domain configurations
- hidden system instructions
- internal search, processing, and filtering mechanics
- access to different websites

**Using a single SOTA agent doesn't guarantee quality results** — it may miss important sources, have bias toward certain regions/languages, or simply "dig in the wrong direction."

The obvious solution: use multiple (3+) research agents and combine the results.

**How to do this effectively?**
- Combined output volume (500KB+) doesn't fit in context
- LLMs trying to "digest everything at once" get confused and lose details
- Manual synthesis takes days and still loses information
- No consistent, verifiable, or repeatable results

## The Solution

Parallel launch of multiple research agents → strict task preparation methodology → normalization of raw data and references → phased synthesis through a "living scaffold" with embedded instructions → fully validatable final document → executive summary.

## What This Methodology Solves

**Deduplication**: A significant portion of information overlaps between agents — the methodology extracts all value without duplicates.

**Enrichment**: Each agent finds something unique — the methodology helps identify, verify, and use these findings.

**Contradiction Resolution**: Some data conflicts between sources — the methodology requires either verification through primary sources or explicit notation of discrepancies.

**Validatability**: Any fact in the final document can be verified — there's source attribution and bibliography.

**Time Savings**: Research can be completed in 1-2 working days instead of weeks (if done by a human analyst).

## Division of Responsibilities

The user manages three AI actors, each with their own specialization:

### Actor 1: Designer (conversational model)

**What it does:**
- Transforms the customer's request into a structured task
- Defines scope, eligibility, tags, and scales
- Prepares card templates and output formats
- Establishes source policies and quality checklists

**Result:** Self-contained Deep Research task (TASK.md)

### Actor 2: Research Agent (deep research agents)

**What it does:**
- Collection and validation of cases by specified criteria
- Cards by template with C-ID
- Sources with URL and S-ID in Appendix
- Observed Patterns (observations with attribution)

**What it does NOT do:**
- Executive Summary
- Conclusions and recommendations
- Cross-synthesis
- Data normalization

**Result:** Raw reports (50-200KB per agent)

### Actor 3: Synthesizer (CLI agent with bash access)

**What it does:**
- Structure, tag, and reference normalization
- Case and source deduplication
- ID mapping between agents
- Cross-synthesis and contradiction resolution
- Additional checks and validations
- Summary and recommendations

**Results:**
- `NORMALIZATION_REPORT.md` (created in Phase 3, updated in Phases 4-7)
- `_case_references.json`
- `_sources_references.json`
- `SYNTHESIS.md`
- `SUMMARY.md`

## Tasks for the CLI Agent (Actor 3)

The methodology is implemented through a sequence of tasks. Each task represents one type of cognitive work for the agent; details are in the corresponding phases.

| # | Task | Phase | Human Involvement |
|---|------|-------|-------------------|
| 1 | Structural normalization | 3 | |
| 2 | Reference normalization | 4 | ✓ Decision on Phase 5 |
| 2a | Re-attribution (if needed) | 5 | (Optional) |
| 3 | Case mapping | 6.3.1 | |
| 4 | Source mapping | 6.3.2 | ✓ Reference validation |
| 5 | SYNTHESIS.md scaffold + quantitative analysis | 6.2, 6.3.3 | |
| 6 | Card synthesis | 6.5.1 | |
| 7 | Cross-synthesis | 6.5.2 | ✓ Escalations, contradictions |
| 8 | Validation and finalization | 7 | |
| 9 | Executive Summary | 8 | |

### Human Involvement Points

**After task 2** — Based on reference normalization results, decide if Phase 5 (re-attribution) is needed. Triggers: >30% unused references, many unattributed statements.

**After task 4** — Fresh look at references before starting synthesis. Verify: deduplication is correct, mappings haven't lost data.

**After task 7** — Resolution of escalations from cross-synthesis: unresolvable contradictions, cases losing sources, decisions on `[Inference]`.

## Methodology Principles

The methodology is not dogma, but a set of project instructions that have proven effective. They can and should be adapted to specific research needs.

1. **Task quality determines result quality** — tag legend, scope, eligibility, output format in TASK.md. Without a clear task, agents will produce garbage.

2. **One type of cognitive work = one task** — the agent doesn't get confused, results are predictable, easy to debug.

3. **Normalization** — structure, tags, references are brought to a unified format before synthesis begins ("cleaning up after agents"). Without this, synthesis is impossible.

4. **Synthesis state management** — scaffold with TODOs guides the agent between sessions, case and source references are built before card synthesis, "empty" WRK files indicate processing completeness.

5. **Large file handling strategy** — grep/search by keywords instead of "swallowing 500KB." Read in parts, write incrementally.

6. **Agent comparison** — document which agent is better for what, reuse knowledge in future research.

7. **Three levels of verification** — fetch → search by URL + context → human-in-the-loop. Escalate if automatic resolution fails.

8. **Contradictions are explicitly documented** — with attribution of both positions, critical ones go to verification or escalation.

9. **Summary with focus** — audience and purpose determine structure, what goes to the top, which details to preserve.

10. **Separation of technical and semantic work** — scripts for consistency checking (ID, format, counters). Everything else — read and verify semantically, regex doesn't work on analytics.

## Methodology

Detailed description of all phases (1-8) with templates, checklists, and examples:

- 🇬🇧 [METHODOLOGY.md](./METHODOLOGY.md) — English version
- 🇷🇺 [METHODOLOGY_RU.md](./METHODOLOGY_RU.md) — Russian version

## How to Use

1. **Read the README** — understand the approach and actors
2. **Study Phase 1** — task preparation determines everything else
3. **Choose research agents** — minimum 3 (OpenAI, Claude, Gemini recommended)
4. **Follow the phases** — use as project instructions for Actor 3
5. **Adapt** — the methodology is not dogma, adjust to your domain

**Minimum set:** `TASK.md` (task) → raw agent reports → `SYNTHESIS.md` → `SUMMARY.md`

## License

Creative Commons Attribution 4.0 International (CC BY 4.0)

Copyright (c) 2026 Askold Romanov

You are free to:
- **Share** — copy and redistribute the material in any medium or format
- **Adapt** — remix, transform, and build upon the material for any purpose, even commercially

Under the following terms:
- **Attribution** — You must give appropriate credit, provide a link to the license, and indicate if changes were made.

Full license text: https://creativecommons.org/licenses/by/4.0/
