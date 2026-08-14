# Validation protocol

Use this after a prioritized URL has been changed.

## Default window

When enough time has elapsed and no other window is specified:

- baseline: at least 28 complete days before the change
- evaluation: 28 complete days after the change
- use the same GSC property and the same country/device/page scope
- avoid the newest incomplete days

If seasonality or low volume makes 28 days unstable, extend the window and explain why.

## Record the intervention

For each URL, preserve:

- change date
- primary action
- change type
- owner
- what modules/sections changed

Allowed change types:

- `TASK` — page promise, intent, or Decision Task changed
- `ANSWER` — missing answer assets/decision information added or reorganized
- `EVIDENCE` — original proof, first-party data, tests, examples, cases, methodology added
- `DELIVERY` — crawl/index/render/canonical/snippet/internal-link delivery fixed

A change may have multiple types, but identify the primary type.

## Comparison hierarchy

Preferred comparison:

1. same URL, 28d post vs 28d pre
2. same cohort trend over the same periods
3. untreated same-cohort URLs as an internal comparison group when feasible

Use same-cohort controls to reduce false conclusions caused by sitewide demand shifts or Google-wide changes.

Do not call the control group a randomized experiment unless it actually is one.

## KPI layers

### 1. AI visibility

Use as available:

- AI Impressions
- whether the URL is observed in the Generative AI report
- cohort-relative AI percentile/quadrant movement

### 2. Search

Use as available:

- Web Impressions
- Clicks
- CTR
- Average Position as context

### 3. Engagement

Use analytics if available:

- qualified visits
- engagement / depth indicators
- assisted journeys

### 4. Business

Use as available:

- conversions
- leads
- signups
- revenue / assisted revenue

## Interpretation

A successful change does not require every metric to improve.

Examples:

- Q2 page: AI visibility rises while Search remains stable → positive signal
- Q1 page: Search and AI remain stable after a refresh → protection may be successful
- Q3 page: AI visibility remains strong and business engagement improves → expansion may be justified
- Delivery fix: impressions recover without content expansion → confirms delivery issue mattered

Always report confounders such as:

- seasonality
- sitewide technical changes
- major ranking updates
- campaigns
- product launches
- reporting-window mismatch

Do not claim causal attribution from correlated movement alone.
