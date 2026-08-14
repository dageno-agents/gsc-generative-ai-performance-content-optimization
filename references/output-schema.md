# Output schema

Use this after the core SKILL.md run protocol.

## A. Analysis Summary

```yaml
property:
period:
filters:
analysis_mode:
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
metadata_rows:
metadata_matched_urls:
metadata_page_type_coverage:
metadata_language_coverage:
quadrant_eligible_urls:
cohort_method:
min_cohort_size:
major_warnings:
```

## B. Cohort + Quadrant Overview

Provide counts only for eligible matched URLs.

For each applied cohort/fallback, report:

- cohort label
- cohort level (`language+page_type`, `page_type`, `language`, `global`)
- eligible URL count
- Web High threshold
- AI High threshold
- Q1/Q2/Q3/Q4 counts

List separately:

- UNOBSERVED_ON_AI_SIDE
- UNOBSERVED_ON_WEB_SIDE
- LOW_SAMPLE

Do not hide these states inside Q2/Q3.

## C. Prioritized URL Queue

Recommended columns:

- URL
- page type
- language
- applied cohort
- cohort level
- observation status
- quadrant
- Web Impressions
- AI Impressions
- cohort Web percentile
- cohort AI percentile
- global Web percentile
- global AI percentile
- business value
- primary hypothesis
- confidence
- next check

## D. Deep Diagnosis Card

### URL

`https://example.com/page`

### Status

`Q2 — Search High / AI Low`

### Cohort

`en | comparison` (or fallback used)

### Confidence

`High / Medium / Low`

### What the data actually tells us

State only what GSC supports.

### What the data does NOT tell us

Call out missing prompt/citation/task evidence.

### Decision Task

One sentence.

### AI-answer opportunity

`Verified / Plausible / Unverified / Low`

### Diagnosis

Choose one or more:

- No verified AI-answer opportunity
- Decision-task mismatch
- Answer gap
- Evidence gap
- Delivery gap
- Freshness gap
- Insufficient sample
- Unobserved due to export limits

### Why

2–4 evidence-based reasons.

### Primary Action

One allowed action label.

### Recommended Changes

Concrete section/module-level changes.

### Evidence Required

What must be created, measured, tested, or sourced.

### Do Not Do

Unsupported changes to avoid.

### Priority

`P0 / P1 / P2 / P3 / MONITOR`

### Validation Plan

Use the default 28d matched-window protocol unless another window is justified.

## E. Owner-ready Action Backlog

Create one row per prioritized URL after diagnosis.

Recommended columns:

- URL
- page type
- language
- applied cohort
- quadrant
- Decision Task
- AI opportunity (`Verified / Plausible / Unverified / Low`)
- Primary Gap (`TASK / ANSWER / EVIDENCE / DELIVERY / NONE`)
- Primary Action
- Priority
- business value
- confidence
- Owner
- Change Date
- Review Date
- baseline window
- evaluation window
- notes

Rules:

- do not invent Owner, Change Date, or Review Date
- use `TBD` or blank if not provided
- Primary Gap must describe the main reason for action, not every possible issue
- Review Date should normally be scheduled after enough post-change data exists for the validation window

## F. Optimization / Content Brief

Only for DEEPEN, REBUILD, CREATE, or EXPAND_CLUSTER.

```yaml
working_title:
url_or_new_asset:
primary_topic:
decision_task:
primary_intent:
target_audience:
business_stage:
why_this_content_should_exist:
current_answer_gap:
current_evidence_gap:
current_delivery_gap:
evidence_required:
must_answer:
  - ...
recommended_sections:
  - ...
do_not_include:
  - ...
internal_links:
external_primary_sources:
validation_plan:
confidence:
```

The brief must explain **why the page should exist/change** before explaining how to write it.

## G. Validation Report

When post-change data exists, report:

```yaml
url:
cohort:
primary_action:
change_type:
change_date:
baseline_window:
evaluation_window:
control_urls_or_cohort:
ai_visibility_change:
search_change:
engagement_change:
business_change:
major_confounders:
interpretation:
confidence:
next_action:
```

Do not infer causality from correlation alone.
