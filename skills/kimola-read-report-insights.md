---
name: Read research insights from a Kimola feed and report
description: Walk from a feedback feed to its research reports and pull the individual analyses (summaries, themes, personas, insights) out of a report.
api: https://api.kimola.com/v1
operations:
  - "GET /v1/feeds"
  - "GET /v1/feeds/{code}/reports"
  - "GET /v1/reports/{code}"
  - "GET /v1/reports/{code}/analyses"
  - "GET /v1/reports/{code}/analyses/{slug}"
---

# Read research insights from a Kimola feed and report

## Auth
Send `Authorization: Bearer <apiKey>` on every request (key from the Kimola
dashboard Account menu).

## Steps
1. **List feeds.** `GET /v1/feeds?pageIndex=0&pageSize=10` to find the feed
   (stream of feedback around a topic/brand) you care about; note its `code`.
2. **Find the feed's reports.** `GET /v1/feeds/{code}/reports` (or
   `GET /v1/feeds/{code}/reports/recent`) to list the research reports derived
   from that feed; note a report `code`.
3. **Open the report.** `GET /v1/reports/{code}` for report metadata, then
   `GET /v1/reports/{code}/analyses` to list the analyses it contains.
4. **Pull an analysis.** `GET /v1/reports/{code}/analyses/{slug}` for a single
   analysis, and `GET /v1/reports/{code}/analyses/{slug}/data` for its
   structured data (themes, sentiment, personas, journey maps, etc.).

## Rules
- Pagination is zero-based via `pageIndex`/`pageSize` (default pageSize 10).
- 404 means the report/feed/analysis does not exist; 406 signals a query
  processing failure — retry the list call. See errors/kimola-error-codes.yml.
