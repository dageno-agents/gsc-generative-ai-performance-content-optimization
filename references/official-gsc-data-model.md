# Official GSC Generative AI report notes

Last reviewed: 2026-08-14

Use this file as a maintenance reference. If web access is available, prefer the latest official Google documentation because this report is still evolving.

## Current Search report facts

Google's dedicated Generative AI Performance report for Search currently reports visibility in generative AI features on Google Search.

Official sources:

- Search Console Help — Generative AI performance report (Search):
  https://support.google.com/webmasters/answer/16984139?hl=en
- Google Search Central Blog — Introducing Search Generative AI performance reports in Search Console:
  https://developers.google.com/search/blog/2026/06/gen-ai-performance-reports
- Google Search Central — AI features and your website:
  https://developers.google.com/search/docs/appearance/ai-features
- Google Search Central — Optimizing for generative AI features:
  https://developers.google.com/search/docs/fundamentals/ai-optimization-guide
- Search Console Help — Export data directly from a Search Console report:
  https://support.google.com/webmasters/answer/12919797?hl=en

## What is currently included

Search Generative AI report:

- AI Overviews
- AI Mode
- Impressions
- Pages
- Countries
- Devices
- Dates

Google notes that Search Labs experiments are not included.

## Important reporting behavior

- Generative AI Search data is included in the Web search type of the overall Search Performance report.
- Do not add Generative AI impressions to Web impressions as if they were two independent traffic universes.
- Page data is grouped by final linked URL after redirects and generally assigned according to canonical reporting rules.
- Chart totals can differ from page-table totals because chart data may be property-aggregated while page tables are page-aggregated.
- Search Console report table exports are truncated to 1,000 representative rows; report totals can include data not present in the exported table.
- Values displayed as `~` or `-` may be exported as zero.

## What the dedicated report does not currently expose

Do not claim the report gives:

- query/prompt text
- clicks
- CTR
- average position
- citation placement
- exact quoted passages
- conversion/revenue attribution

If Web Search query data is used alongside the Generative AI report, describe it as query/task context, not as a direct prompt-to-AI-impression attribution.

## Optimization guidance that should constrain recommendations

Google's current guidance says:

- core SEO best practices remain relevant for AI Overviews and AI Mode
- there are no extra technical requirements solely for these AI features
- pages must be indexed and eligible to appear in Search with a snippet
- there is no special schema.org markup required specifically for AI features
- no new machine-readable AI files are required
- query fan-out can issue multiple related searches to support complex answers
- valuable, unique, non-commodity, first-hand content is emphasized over generic repetition

The skill should therefore avoid default recommendations such as "add AI schema," "create ai.txt," "make one page per fan-out query," or "split everything into tiny chunks."
