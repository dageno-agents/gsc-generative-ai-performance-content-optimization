# Changelog

## 2.1.0

Alignment update with the article methodology and team execution workflow.

### Cohort-aware quadrants

- Added optional URL enrichment metadata: page type, language, business value, owner.
- Added deterministic cohort hierarchy: `language + page_type` → `page_type` → `language` → global.
- Added default `min_cohort_size = 30` and explicit fallback recording.
- Thresholds and triage percentiles now use the applied peer cohort when available.
- Added global percentiles as context while keeping cohort-relative mismatch as the main shortlist signal.
- Added stronger warning that mixed sitewide baselines can misclassify product/docs/blog populations.

### Validation

- Added a default 28-day pre vs 28-day post validation protocol.
- Added intervention tracking with `TASK / ANSWER / EVIDENCE / DELIVERY` change types.
- Added untreated same-cohort URLs as an optional internal comparison group.
- Split validation into AI visibility, Search, Engagement, and Business layers.
- Added explicit anti-causality language and confounder reporting.

### Action backlog

- Added a required owner-ready Action Backlog after deep diagnosis.
- Added Owner, Change Date, Review Date, cohort, AI opportunity, Primary Gap, Primary Action, and Priority fields.
- Brief generation remains restricted to DEEPEN / REBUILD / CREATE / EXPAND_CLUSTER.

### Script

- Added optional metadata input to `prepare_gsc_analysis.py`.
- Added metadata coverage reporting.
- Added deterministic cohort assignment, cohort thresholds, cohort percentiles, and fallback level per URL.

## 2.0.0

Major reliability and method-alignment update.

### Data layer

- Added explicit `MATCHED / AI_ONLY / WEB_ONLY` observation states.
- Missing export rows are no longer treated as zeros by default.
- Added mandatory 1,000-row truncation QA.
- Added chart/table aggregation caveat.
- Added `ai_row_present` / `web_row_present` and zero-vs-unobserved distinction.
- Quadrants now default to matched, eligible URLs only.
- Default axes are simple Web Impressions × AI Impressions.
- Default High/Low threshold is deterministic site-relative median.
- Added percentile mismatch intensity for shortlisting, not as a KPI.
- Downgraded AI Exposure Ratio to an optional internal diagnostic.

### Diagnosis

- Main diagnosis chain changed from Citation-first to:
  `Decision Task → Answer Gap → Evidence Gap → Delivery Gap → Action`.
- Citation/source landscape is now optional and only used when actual AI-answer evidence exists.
- Added explicit Q2 AI-answer-opportunity gate.
- Added Delivery Gap checks and `FIX_DELIVERY` action.

### Output/run behavior

- Added mandatory Analysis Summary / QA output before quadrants.
- Added separate unobserved and low-sample queues.
- Added default shortlist-before-deep-diagnosis protocol.
- Added "What the data does NOT tell us" to URL diagnosis cards.
- Added stronger confidence and next-verification behavior.

### Package

- Added official GSC data-model maintenance reference.
- Added quadrant/diagnosis playbook.
- Added deterministic preprocessing reference script.
