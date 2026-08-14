# gsc-generative-ai-performance-content-optimization

A Codex skill for analyzing Google Search Console Generative AI Performance and Web Search data to identify page-level AI-search visibility gaps and defensible content optimization opportunities.

The skill combines deterministic data processing with evidence-based model judgment:

> **Code calculates. The model judges.**

It is designed to help SEO, GEO, content, growth, and digital strategy teams determine which pages should be protected, investigated, deepened, rebuilt, created, merged, fixed, monitored, or deprioritized.

## What This Skill Does

This skill helps Codex:

- Validate whether Generative AI and Web Search exports are comparable
- Detect export row-limit and missing-row risks
- Normalize and match page URLs conservatively
- Preserve `MATCHED`, `AI_ONLY`, and `WEB_ONLY` observation states
- Compare similar page cohorts instead of mixing unrelated page types
- Assign eligible pages to Search × AI visibility quadrants
- Identify high-value Search and AI visibility mismatches
- Diagnose Decision Task, Answer, Evidence, and Delivery gaps
- Convert findings into an owner-ready optimization backlog
- Produce content briefs only when a new or substantially revised asset is justified
- Define matched pre-change and post-change validation windows

The skill does not assume that every page needs GEO optimization.

## Important Data Constraints

The dedicated Google Search Console Generative AI report should be treated as a visibility report—not as a prompt, citation, or conversion analytics system.

Depending on the current GSC report, the available fields may include:

- AI impressions
- Pages
- Countries
- Devices
- Dates

The dedicated report does not, by itself, prove:

- Which prompt produced an AI impression
- Which query triggered a particular AI answer
- Where a citation appeared
- Which passage was quoted
- Whether an AI impression generated a conversion
- Whether low AI exposure means that a page has poor content

The skill therefore follows several non-negotiable rules:

- Missing export row does not automatically mean zero
- Generative AI report data is not an AI prompt log
- Low AI exposure does not automatically mean poor GEO
- Quadrants prioritize investigation; they do not provide the final diagnosis
- Content recommendations require evidence
- Traditional SEO foundations remain relevant
- No special “AI schema” is assumed to be required for Google AI features

When the report or its fields may have changed, verify the latest official Google Search Console documentation before analysis.

## Required Inputs

At minimum, provide two page-level exports covering the same scope.

### 1. Generative AI Search — Pages

Preferred fields:

- Page or URL
- AI Impressions
- Date range
- Country filter
- Device filter

### 2. Web Search Results — Pages

Preferred fields:

- Page or URL
- Clicks
- Impressions
- CTR
- Average position
- The same date range, country, and device filters

## Optional Inputs

The analysis becomes stronger when you also provide:

- Web Search query export
- Page type
- Language or locale
- Business value
- Content owner
- Previous-period exports
- Analytics or conversion data
- Representative AI answers and visible citations
- Competitor pages
- First-party evidence assets

Page-type and language metadata allow the skill to compare similar pages instead of applying one global benchmark to an entire mixed website.

## Analysis Modes

The skill uses the highest analysis mode supported by the available evidence.

### Mode A — Visibility Triage

Inputs:

- Generative AI Pages export
- Web Search Pages export

Supports:

- Data QA
- URL matching
- Observation status
- Cohort thresholds
- Percentiles
- Four-quadrant triage
- Candidate shortlisting

### Mode B — Query-Informed Diagnosis

Mode A plus Web Search query data.

Adds:

- Decision-task inference
- Query clustering
- Demand and intent evidence

Web Search queries are treated as task and demand evidence—not as direct AI prompts.

### Mode C — Page Diagnosis

Mode B plus accessible page content or HTML.

Adds:

- Task-mismatch diagnosis
- Answer-gap diagnosis
- Evidence-gap diagnosis
- Delivery-risk review

### Mode D — AI-Answer Landscape Diagnosis

Mode C plus representative AI answers and visible supporting links.

Adds:

- Specific analysis of current answer patterns
- Citation-source comparisons
- Competitor citation analysis

## Four-Quadrant Framework

Eligible matched URLs are classified using Web Search impressions and Generative AI impressions.

| Quadrant | Meaning | Default Approach |
|---|---|---|
| Q1 — Search High / AI High | Core asset | Protect, refresh, or selectively deepen |
| Q2 — Search High / AI Low | Potential AI gap | Validate AI-answer demand before changing the page |
| Q3 — Search Low / AI High | AI discovery opportunity | Investigate complex, comparative, or fan-out tasks |
| Q4 — Search Low / AI Low | Low signal | Reassess demand, uniqueness, and business value |

The quadrant is a triage mechanism, not a content-quality score.

## Diagnosis Framework

Shortlisted pages are diagnosed in this order:

```text
Decision Task
    ↓
Answer Gap
    ↓
Evidence Gap
    ↓
Delivery Gap
    ↓
Primary Action
```

### Decision Task

What is the user trying to accomplish?

Examples:

- Learn
- Compare
- Evaluate
- Choose
- Buy
- Implement
- Troubleshoot
- Calculate
- Verify

### Answer Gap

Does the page answer the relevant task clearly and completely?

Possible findings include:

- `FORMAT_FINDABILITY_GAP`
- `MISSING_ANSWER_ASSET`
- `TASK_MISMATCH`

### Evidence Gap

Does the page contain evidence that makes it more useful and trustworthy than a generic summary?

Examples:

- Original data
- First-party testing
- Screenshots or demonstrations
- Customer cases
- Benchmarks
- Transparent methodology
- Expert judgment
- Primary sources
- Traceable citations
- Clear scope and limitations

### Delivery Gap

Can search and AI systems reliably access and interpret the page?

Examples:

- Indexability
- Robots or CDN restrictions
- Canonicals and redirects
- Snippet controls
- Important information unavailable as text
- JavaScript rendering issues
- Weak internal discoverability
- Structured data that does not match visible content

## Supported Actions

The skill assigns one primary action and, when justified, one secondary action:

- `PROTECT`
- `REFRESH`
- `DEEPEN`
- `REBUILD`
- `CREATE`
- `EXPAND_CLUSTER`
- `MERGE`
- `FIX_DELIVERY`
- `DEPRIORITIZE`
- `MONITOR`

It does not automatically recommend longer articles, FAQ sections, prompt repetition, arbitrary schema, or one page for every query variation.

## Deterministic Analysis Script

The repository includes:

```text
scripts/prepare_gsc_analysis.py
```

The script performs:

- Conservative URL normalization
- Outer joins and observation-state preservation
- Export row-limit QA
- Optional metadata enrichment
- Cohort-aware median thresholds
- Cohort and global percentiles
- Four-quadrant assignment
- Joined CSV and summary JSON generation

It intentionally does not diagnose content quality. Diagnosis occurs after deterministic triage.

## Requirements

- Python 3.10 or newer recommended
- CSV and TSV files use the Python standard library
- XLSX support requires `openpyxl`

Install optional XLSX support:

```bash
python3 -m pip install openpyxl
```

## Script Usage

Basic CSV or TSV analysis:

```bash
python3 scripts/prepare_gsc_analysis.py \
  --ai generative-ai-pages.csv \
  --web web-search-pages.csv \
  --out-prefix output/gsc-ai-analysis
```

With optional metadata:

```bash
python3 scripts/prepare_gsc_analysis.py \
  --ai generative-ai-pages.xlsx \
  --web web-search-pages.xlsx \
  --metadata page-metadata.xlsx \
  --out-prefix output/gsc-ai-analysis \
  --min-cohort-size 30
```

Optional metadata may include:

```text
URL
Page Type
Language
Business Value
Owner
```

Generated outputs:

```text
output/gsc-ai-analysis_joined.csv
output/gsc-ai-analysis_summary.json
```

## Installing the Skill

### Install through Codex

Ask Codex:

```text
Install the skill from:
https://github.com/dageno-agents/gsc-generative-ai-performance-content-optimization
```

### Manual Installation

Clone the repository into your Codex skills directory:

```bash
git clone \
  https://github.com/dageno-agents/gsc-generative-ai-performance-content-optimization.git \
  ~/.codex/skills/ai-search-content-opportunity
```

Restart Codex after installation if the skill is not detected immediately.

## Example Requests

After installing the skill, you can ask Codex:

```text
Use the AI Search Content Opportunity skill to analyze these GSC Generative AI and Web Search page exports.
```

```text
Match these GSC exports, run data QA, build cohort-aware quadrants, and identify the highest-priority Q2 and Q3 pages.
```

```text
Analyze these GSC exports and page-type metadata. Produce an owner-ready content optimization backlog.
```

```text
Review this shortlisted Q2 page and determine whether it has a Decision Task, Answer, Evidence, or Delivery gap.
```

```text
Compare the 28 days before and after these content changes and prepare a validation report.
```

## Output Structure

The default analysis produces:

1. Analysis Summary and Data QA
2. Cohort and Quadrant Overview
3. Unobserved and Low-Sample Queues
4. Prioritized URL Queue
5. Deep Diagnosis Cards
6. Owner-Ready Action Backlog
7. Optimization or Content Briefs when justified
8. Validation Plan

See [`references/output-schema.md`](references/output-schema.md) for the recommended fields.

## Repository Structure

```text
.
├── SKILL.md
├── CHANGELOG.md
├── scripts/
│   └── prepare_gsc_analysis.py
└── references/
    ├── official-gsc-data-model.md
    ├── validation-protocol.md
    ├── diagnosis-playbook.md
    └── output-schema.md
```

## Validation Approach

The default evaluation protocol uses:

- At least 28 days of baseline data
- 28 days after the change
- Matching property, country, device, and page scope
- Recorded change dates and change types
- Same-cohort untreated pages as an internal comparison group when feasible

The skill does not claim causality from correlated movement alone.

See [`references/validation-protocol.md`](references/validation-protocol.md) for details.

## Version

Current version: **2.1.0**

See [`CHANGELOG.md`](CHANGELOG.md) for release history.

## Disclaimer

This is an independent Codex skill and is not an official Google product.

Google Search Console fields and reporting behavior may change. Always verify current official documentation before relying on report-specific assumptions.
