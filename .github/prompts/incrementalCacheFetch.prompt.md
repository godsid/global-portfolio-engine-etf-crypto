---
name: incrementalCacheFetch
description: Update cache-aware market fetching with timezone cutoff and incremental merge.
argument-hint: Data source types, daily close cutoffs, timezone, and freshness criteria.
---
Refactor the current market-data loading flow to be cache-first and incrementally updatable.

Requirements:
- Inspect existing cache read/write and fetch functions in the current code.
- Determine whether cached data is already up to date by comparing the latest cached date against the expected latest completed daily candle.
- Compute expected completion using the specified timezone and per-source daily close cutoffs:
  - Source type A closes at `<cutoffA>`
  - Source type B closes at `<cutoffB>`
- If cache is up to date, return cached data without fetching.
- If cache is stale, fetch only missing data starting after the latest cached date.
- Merge fetched rows into cache without duplicates (key by date), preserve sorted chronological order.
- Do not clear the full cache when stale; extend it incrementally.
- Keep graceful fallback behavior: if incremental fetch fails, keep using existing cache when available.
- Preserve existing logging/status updates and response contracts.

Implementation expectations:
1. Add helper(s) for timezone-aware "expected latest date" calculation.
2. Add helper(s) for cache freshness check by latest date.
3. Add helper(s) to merge historical rows deterministically.
4. Extend provider fetch methods to support optional `fromDate` incremental fetching.
5. Update the main per-asset fetch flow to:
   - use cache when fresh,
   - fetch incrementally when stale,
   - write merged data back to cache.

Constraints:
- Keep changes minimal and localized.
- Preserve existing API shapes unless explicitly required.
- Avoid unrelated refactors.
- Ensure behavior is deterministic across timezones.
