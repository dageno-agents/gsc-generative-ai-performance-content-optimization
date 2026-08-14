---
name: ai-search-content-opportunity
description: Analyze Google Search Console Generative AI Performance and Web Search data to identify page-level AI-search opportunities. Use deterministic processing for scope validation, URL matching, observation status, row-limit risk, cohort-aware thresholds, percentiles, quadrants, and trend math. Use model judgment only after triage to diagnose Decision Task, Answer Gap, Evidence Gap, Delivery Gap, and the correct content action. Never treat a missing export row as zero unless the dataset is demonstrably exhaustive, never claim the Generative AI report exposes prompts/queries, and never infer that low AI exposure means poor GEO without validating AI-answer demand.
version: 2.1.0
---

# AI Search Content Opportunity Skill

## 1. Goal

Turn GSC data into **content decisions**, not generic GEO advice.

> **Code calculates. The model judges.**

Always answer in this order:

1. What data is actually observed?
2. Which URLs can be compared safely?
3. Which URLs should be compared against the same peer cohort?
4. Where is there a meaningful Search ↔ AI visibility mismatch?
5. Does a real AI-answer opportunity exist?
6. If yes, is the gap in **Decision Task, Answer, Evidence, or Delivery**?
7. What action is justified?
8. Who should do it, and when should it be reviewed?
9. How should the team validate the result?

Do not optimize every page for AI.

---

# 2. Current GSC constraints

Treat the dedicated Generative AI report as a **visibility report**, not a prompt/citation analytics system.

Current Search report behavior includes:

- AI Overviews and AI Mode
- Impressions
- Pages, Countries, Devices, Dates
- page reporting based on final/canonical URL rules
- data that is already included in overall Web Search performance
- the usual Search Console row-limit and aggregation caveats

The dedicated report does **not** by itself provide:

- query/prompt text
- clicks
- CTR
- average position
- citation placement
- quoted-passage attribution
- conversion/revenue attribution

Therefore:

> Never say "this AI impression came from this query" using the dedicated Generative AI report alone.

Web Search queries can be used as **task/demand evidence**, not as a direct AI prompt log.

If web access is available and the report may have changed, verify the current official field list first. See `references/official-gsc-data-model.md`.

---

# 3. Analysis modes

Use the highest mode supported by the inputs. Do not pretend to have evidence from a higher mode.

## Mode A — Visibility triage

Inputs:

- Generative AI Search report by Page
- Web Search Results report by Page

Can do:

- QA
- URL matching
- observation status
- cohort/global thresholds and percentiles
- four-quadrant triage
- shortlist candidates

Cannot reliably infer exact tasks or current AI citation behavior.

## Mode B — Query-informed diagnosis

Mode A plus Web Search query data.

Can additionally infer likely Decision Tasks and cluster demand.

Do not call Web query rows "AI prompts."

## Mode C — Page diagnosis

Mode B plus page HTML/text/accessible URL.

Can additionally diagnose Task mismatch, Answer Gap, Evidence Gap, and Delivery risks.

## Mode D — AI-answer landscape diagnosis

Mode C plus representative AI Overview/AI Mode answers and visible supporting links/citations.

Only here make specific claims about current AI answer/citation patterns.

---

# 4. Core inputs

## Generative AI Search — Page dimension

Prefer:

- URL/Page
- AI Impressions
- date range
- country filter
- device filter

## Web Search Results — Web — Page dimension

Prefer:

- URL/Page
- Clicks
- Impressions
- CTR
- Average position
- same date range/filter scope

## Optional enrichment metadata

Strongly recommended when the site mixes different content populations:

- URL
- page type
- language
- business value
- owner

Examples of page type:

- product
- category
- comparison
- solution
- blog
- docs
- help center
- landing page

Use enrichment metadata to build **comparable cohorts**, not to overwrite GSC metrics.

Other optional inputs:

- previous-period exports
- Web query export
- target market / ICP
- analytics/conversion data
- representative AI answers/citations
- competitor pages
- first-party evidence assets

If scope metadata is missing, continue best-effort but lower confidence.

---

# 5. Mandatory data QA

**Do this before quadrants.**

## 5.1 Scope match

Verify:

1. same GSC property
2. same date range
3. same country filter
4. same device filter
5. Generative AI **Search** is compared with **Web** Search Results, not Discover
6. page-level exports are used for URL joins
7. other page/query filters are compatible

If a material mismatch exists:

- stop cross-report metric comparison
- state the mismatch
- do not silently repair reporting windows

## 5.2 Row-limit risk

Record raw row counts.

Default:

- 1,000 rows → `row_limit_risk = HIGH`
- fewer than 1,000 → `row_limit_risk = LOWER`, not guaranteed exhaustive

Critical rule:

> **Missing export row ≠ verified zero.**

## 5.3 Observation status

Outer-join conservatively normalized URLs and preserve:

- `MATCHED`
- `AI_ONLY`
- `WEB_ONLY`

Also preserve:

```yaml
ai_row_present: true|false
web_row_present: true|false
```

Distinguish:

- `AI_REPORTED_ZERO_OR_SUPPRESSED`
- `AI_NOT_OBSERVED_IN_EXPORT`

Never collapse these states.

## 5.4 Required QA summary

Before any quadrant output, report:

```yaml
ai_export_rows:
web_export_rows:
unique_ai_urls:
unique_web_urls:
matched_urls:
ai_only_urls:
web_only_urls:
matched_share_of_union:
ai_row_limit_risk:
web_row_limit_risk:
metadata_coverage:
major_scope_warnings:
```

If overlap is weak, say so.

## 5.5 Aggregation caveat

Do not expect chart totals and Pages-table totals to match exactly; property-level and page-level aggregation can differ.

Do not treat that discrepancy as a content problem.

---

# 6. Deterministic processing

Use code for all calculations in this section.

A reference implementation is included at `scripts/prepare_gsc_analysis.py`.

## 6.1 Conservative URL normalization

Allowed:

- trim whitespace
- lowercase scheme/hostname
- remove fragments
- normalize obvious default ports

Do not automatically:

- lowercase path
- remove query parameters
- remove trailing slash
- infer canonical URL from shape
- fuzzy-match different paths

If normalization creates duplicates, aggregate metrics deterministically and flag duplicates.

## 6.2 Primary metrics

Primary fields:

- Web Impressions
- Web Clicks
- Web CTR
- Web Position
- AI Impressions
- observation status
- row-presence flags
- page type/language when supplied

### Optional internal diagnostic

`AI Relative Exposure Index = AI Impressions / Web Impressions`

Use only for matched rows with Web Impressions > 0.

Interpret only as a **site-internal relative diagnostic**.

Never call it:

- AI traffic share
- AI search share
- AI market share

Do not use it as the primary quadrant axis or executive KPI.

## 6.3 Trends

For matched previous-period values > 0:

`growth = (current - previous) / previous`

If previous = 0, label `NEW_FROM_ZERO`; do not output infinite growth.

---

# 7. Four-quadrant triage

A quadrant is a **triage device, not a diagnosis**.

## 7.1 Eligibility

Default `quadrant_eligible = true` only when:

- observation status is `MATCHED`
- required values are numeric
- scope checks passed
- any user-defined sample rule is satisfied

Do **not** force `AI_ONLY` or `WEB_ONLY` URLs into Q2/Q3 when row-limit risk exists.

Route them to separate queues:

- `UNOBSERVED_ON_AI_SIDE`
- `UNOBSERVED_ON_WEB_SIDE`

## 7.2 Default axes

Use simple axes:

- X = Web Impressions
- Y = AI Impressions

Clicks, CTR, Position, business value, and the optional exposure index are diagnostic context, not default quadrant axes.

Do not build an opaque composite score unless the user explicitly asks.

## 7.3 Cohort-aware High/Low thresholds

Do **not** compare unlike page populations when cohort metadata exists.

Default cohort hierarchy:

1. `language + page_type`
2. `page_type`
3. `language`
4. global matched set

Use the most specific cohort that contains at least `min_cohort_size` eligible URLs. Default `min_cohort_size = 30`.

If a more specific cohort is too small, fall back one level and record the fallback.

Within the applied cohort:

- Web High threshold = median(Web Impressions)
- AI High threshold = median(AI Impressions)
- use `>= threshold` as High

If no enrichment metadata is supplied, use the global matched set and say that the analysis is **global-baseline only**.

If the user already uses an agreed P70/P80/business threshold, use that and state it.

## 7.4 Percentiles

Calculate percentiles within the **applied threshold cohort** for triage and mismatch intensity.

Also calculate global percentiles when useful for context.

Mismatch intensity is only for shortlisting:

- Q2: `cohort_web_percentile - cohort_ai_percentile`
- Q3: `cohort_ai_percentile - cohort_web_percentile`

This is not an official GSC metric.

## 7.5 Low sample

Flag tiny samples as `LOW_SAMPLE`.

Do not let a tiny denominator or tiny cohort produce a high-priority recommendation solely because a ratio/percentile is extreme.

---

# 8. Quadrant interpretation

## Q1 — Search High / AI High

**Core Asset**

Default stance:

- PROTECT
- REFRESH
- DEEPEN selectively

Never full-rewrite by default.

## Q2 — Search High / AI Low

**Potential AI Gap**

Do not call it "not AI-friendly."

First validate whether meaningful AI-answer demand actually exists.

Possible conclusion:

> Traditional Search asset; insufficient evidence of an AI-answer opportunity.

## Q3 — Search Low / AI High

**AI Discovery Opportunity**

Investigate whether the page supports longer, complex, comparative, or fan-out-style tasks.

Do not create one page per query/prompt variation.

## Q4 — Search Low / AI Low

**Low Signal / Reassess**

Gate investment by demand, business value, uniqueness, freshness, duplication, and strategic role.

For detailed quadrant questions and action mapping, read `references/diagnosis-playbook.md`.

---

# 9. Deep diagnosis

Only deeply diagnose URLs that survive deterministic triage.

Use this order:

> **Decision Task → Answer Gap → Evidence Gap → Delivery Gap → Action**

Do not skip directly from quadrant to formatting advice.

## 9.1 Decision Task

Ask what the user is actually trying to accomplish.

Common families:

- Learn
- Compare
- Evaluate
- Choose
- Buy
- Implement
- Troubleshoot
- Calculate
- Verify
- Find alternatives/examples/evidence

### Mandatory Q2 gate

Before changing a Q2 page, determine:

1. Is the task suitable for a generative answer?
2. Are AI Overviews/AI Mode observed or plausibly relevant for representative queries?
3. Is the task/business value important enough to justify work?

If no/unknown:

- do not diagnose GEO failure
- use MONITOR/PROTECT as appropriate
- state the next verification step

### Query handling

If Web query data is available:

- preserve raw query
- cluster by Decision Task
- extract useful constraints/context
- do not treat every long query as a separate page opportunity
- do not call context-style queries AI prompts without independent proof

## 9.2 Answer Gap

Separate:

- information exists but is hard to find → `FORMAT_FINDABILITY_GAP`
- required answer asset is missing → `MISSING_ANSWER_ASSET`
- page solves a different job → `TASK_MISMATCH`

Task mismatch often needs CREATE/REBUILD, not cosmetic edits.

## 9.3 Evidence Gap

Ask why a user or Google should rely on this page instead of a commodity summary.

Look for:

- original data
- first-party experience/testing
- screenshots/demos
- customer cases
- benchmarks/methodology
- expert judgment
- primary sources
- traceable citations
- freshness/date
- scope/limitations

If the page answers the question but lacks proof, call it Evidence Gap, not "needs more content."

### Optional citation/source landscape

Use only when actual AI answers/supporting links are observed.

Citation analysis is supporting evidence, **not a mandatory main diagnosis layer**.

Do not reduce it to "get more backlinks."

## 9.4 Delivery Gap

Check only with evidence:

- indexed/snippet-eligible
- robots/CDN access
- canonical/redirects
- noindex/nosnippet/data-nosnippet/max-snippet
- important content available as text
- rendering/JS issues
- internal-link discoverability
- structured data matches visible content
- merchant/business information current when relevant

Do not recommend special "AI schema" or AI-specific files as Google Search requirements.

There are no special technical requirements solely for AI Overviews/AI Mode beyond normal Search eligibility/best practices.

---

# 10. Allowed primary actions

Choose **one primary action** and optionally one secondary action:

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

Do not automatically recommend:

- FAQ sections
- FAQ schema
- arbitrary schema "for GEO"
- tiny content fragments
- more word count
- prompt/keyword repetition
- one page per prompt variant
- rewriting a winner merely because AI visibility is below average

Any such recommendation requires a diagnosed reason.

Detailed mapping lives in `references/diagnosis-playbook.md`.

---

# 11. Priority

Assign priority **after diagnosis**.

Consider:

- demand/task importance
- business value
- existing Search authority
- mismatch intensity within the applied cohort
- gap severity
- evidence advantage
- implementation effort
- confidence

Preferred labels:

- P0
- P1
- P2
- P3
- MONITOR

Avoid pseudo-precise numerical scoring when inputs are qualitative.

---

# 12. Default run protocol

Do not model-diagnose hundreds of pages.

## Phase 1 — QA

Output scope, rows, overlap, row-limit risk, metadata coverage, warnings.

## Phase 2 — Cohort-aware deterministic triage

Output:

- eligible URLs
- applied cohort/fallback per URL
- cohort thresholds
- quadrant counts
- cohort/global percentiles
- unobserved and low-sample queues

## Phase 3 — Shortlist

Default: shortlist up to 10–20 URLs.

Prioritize:

- high-value Q2/Q3 mismatches
- important Q1 assets that need protection review

Avoid filling the list with low-value Q4 pages.

If business value is unknown, say ranking is visibility-based only.

## Phase 4 — Deep diagnosis

Use only available evidence.

If page/query/AI-answer evidence is missing, output:

- what is supported
- what is unknown
- confidence
- next verification step

## Phase 5 — Owner-ready Action Backlog

Convert diagnosed URLs into execution rows containing:

- URL
- cohort
- quadrant
- Decision Task
- AI opportunity status
- Primary Gap
- Primary Action
- Priority
- Owner
- Change Date
- Review Date

Do not invent owner/date values; leave them blank or mark `TBD` when not provided.

## Phase 6 — Briefs

Create full briefs only for:

- DEEPEN
- REBUILD
- CREATE
- EXPAND_CLUSTER

Do not create full briefs for PROTECT/MONITOR/DEPRIORITIZE unless requested.

## Phase 7 — Validation

Use the default validation protocol in `references/validation-protocol.md` unless the user specifies another window.

---

# 13. Required output

Always output these layers in order:

1. **Analysis Summary / Data QA**
2. **Cohort + Quadrant Overview** for eligible matched URLs
3. separate **Unobserved / Low Sample queues**
4. **Prioritized URL Queue**
5. **Deep Diagnosis Cards** only for shortlisted URLs
6. **Owner-ready Action Backlog**
7. **Optimization/Content Briefs** only when justified
8. **Validation Plan**

Use the exact recommended fields in `references/output-schema.md`.

---

# 14. Post-change validation

Default to the protocol in `references/validation-protocol.md`.

Minimum default when enough time has elapsed:

- preserve at least 28 days of baseline
- compare 28 days post-change vs 28 days pre-change
- use the same property, country, device, and page scope
- record the modification date and change type
- avoid newest incomplete days
- use untreated same-cohort URLs as an internal comparison group when feasible

Track, as available:

### AI visibility

- AI Impressions
- number of URLs observed in Generative AI report

### Search

- Web Impressions
- Web Clicks
- CTR
- Position as context

### Engagement

- qualified sessions/engagement indicators when available

### Business

- conversions
- leads
- signups
- revenue/assisted outcomes when available

Record `change_type` as one or more of:

- `TASK`
- `ANSWER`
- `EVIDENCE`
- `DELIVERY`

Do not claim causal attribution from correlated movement alone.

---

# 15. Confidence

## High

Usually requires multiple evidence types, such as:

- validated matched GSC data
- comparable cohort baseline
- query/task evidence
- inspected page
- observed AI answers/citations when relevant

## Medium

Usually:

- validated GSC data
- cohort/global baseline
- inspected page
- partial query or AI-answer evidence

## Low

Usually:

- truncated/aggregate GSC only
- unsuitable mixed global baseline
- missing query context
- no page inspection
- no AI-answer evidence

When confidence is Low, recommend the next verification step instead of converting a hypothesis into a diagnosis.

---

# 16. Non-negotiable rules

1. **Data ≠ diagnosis.** Quadrants prioritize investigation.
2. **Missing export row ≠ zero.** Preserve observation status.
3. **Generative AI report ≠ prompt log.** Never invent query-to-AI attribution.
4. **Low AI exposure ≠ bad GEO.** Validate AI-answer opportunity first.
5. **High AI exposure ≠ content quality.** Inspect task and evidence.
6. **Compare like with like.** Use cohort-aware thresholds when metadata supports it.
7. **Prompt/query variants ≠ separate pages by default.** Cluster by Decision Task.
8. **Answer completeness and evidence > arbitrary formatting tricks.**
9. **No special AI schema is required for Google AI features.**
10. **Protect winners.** Avoid unnecessary rewrites.
11. **Unique evidence beats generic expansion.**
12. **Delivery issues are distinct from content gaps.**
13. **Do not manufacture demand, prompt volume, citations, or model behavior.**
14. **Decide before writing.** Diagnosis precedes content generation.
15. **Operationalize recommendations.** Every prioritized diagnosis should become an Action Backlog row.
16. **Validate on matched windows.** Default to 28d pre vs 28d post when feasible.
17. **Unknown is a valid output.** State uncertainty.

---

# 17. End-to-end workflow

```text
Generative AI Search — Pages export
              +
Web Search Results — Pages export
              +
Optional page-type/language metadata
              ↓
Scope + row-limit QA
              ↓
Conservative URL normalization
              ↓
Outer join + observation status
              ↓
Matched-only deterministic metrics
              ↓
Comparable cohort selection
              ↓
Cohort median thresholds + percentiles
              ↓
Four-quadrant triage
              ↓
Shortlist
              ↓
Decision Task
              ↓
Answer Gap
              ↓
Evidence Gap
              ↓
Delivery Gap
              ↓
Primary Action + Priority
              ↓
Owner-ready Action Backlog
              ↓
Brief when justified
              ↓
28d baseline → change → 28d validation
```

The final output should answer:

> **Which pages should we protect, investigate, deepen, rebuild, create, merge, fix, or stop spending time on—why, who owns the next action, and what evidence will tell us whether it worked?**
