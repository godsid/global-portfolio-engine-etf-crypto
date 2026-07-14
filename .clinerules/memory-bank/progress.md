# 📋 Global Portfolio Engine - Progress Tracker

**Project:** global-portfolio-engine-etf-crypto
**Last Updated:** 2026-07-14
**Status:** v55.3 - Recalculate & Send Daily Report Button (Stable)

## 📅 Recent Updates (2026-07-14)

### Recalculate & Send Daily Report Button

**Status:** ✅ COMPLETED
**Priority:** 🟡 Medium
**Files Modified:** `index.html`
**Version:** v55.2 → v55.3

**Tasks:**
- [x] Add "Recalculate & Send" button to Daily Report tab
- [x] Add `force` parameter to `runDailyReport()` for manual sends
- [x] Add `recalculateAndSend()` method to `DailyReport` object
- [x] Version bump v55.2 → v55.3

**Changes:**
1. **New Button**: Added a green "Recalculate & Send" button next to "Refresh" and "Test Telegram" in the Daily Report tab header.
2. **`force` parameter**: Modified `TelegramScheduler.runDailyReport()` to accept `force = false` parameter. When `true`, bypasses the `config.enabled` check so manual sends work even when the scheduler is disabled.
3. **`recalculateAndSend()` method**: New method on `DailyReport` that:
   - Checks Telegram config (token + chatId) and alerts if missing
   - Calls `refreshReport()` to recalculate P&L data
   - Calls `runDailyReport(true)` to generate and send the report
   - Shows loading state → success/error state on the button
   - Updates Telegram status area with success/error message
   - Auto-resets button after 2.5 seconds

## 📅 Previous Updates (2026-07-02)

### Daily Report Tab Content Not Hidden When Switching Tabs

**Status:** ✅ COMPLETED
**Priority:** 🟡 Medium
**Files Modified:** `index.html`
**Version:** v55.1 → v55.2

**Tasks:**
- [x] Daily Report content remains visible when switching to other tabs

**Root Cause:**
The `UI.switchTab()` function hides other tab content elements by their IDs, but `'report'` was missing from the array of IDs to hide. The Daily Report content lives in `#view-report`, so it was never hidden when navigating away.

**Fix Applied:**
Added `'report'` to the hide list in `UI.switchTab()` so that `#view-report` gets the `hidden` class applied when switching to any other tab.

## 📅 Previous Updates (2026-06-29)

### Transaction Logs Not Displaying Fix

**Status:** ✅ COMPLETED
**Priority:** 🔴 High
**Files Modified:** `index.html`
**Version:** v55.0 → v55.1

**Tasks:**
- [x] Transaction Logs not displaying when DCA = 0

**Root Cause:**
1. The `logs.push()` was inside an `if (cfg.dca > 0)` block. Since DCA defaults to `0`, no log entries were ever created for pure rebalance strategies, leaving the Transaction Logs panel empty.
2. The `sharesChange`/`diff` update for the last log entry was also guarded by `cfg.dca > 0`, so even if a log existed it would have no Buy/Sell adjustments or drift data.

**Fix Applied:**
1. Added an `else` branch that creates a `REBAL` type log entry when `DCA = 0` so rebalance events are visible in the Transaction Logs panel.
2. Removed the `cfg.dca > 0` guard on the `sharesChange`/`diff` update so pure rebalance events also show Buy/Sell adjustments and % drift.

## 🔒 Security History

| Date | Status | Implementation |
|------|--------|----------------|
| 2026-06-15 | Proposed AES-GCM | PBKDF2 + AES-GCM (not deployed) |
| 2026-06-24 | Current | XOR + Base64 (stable) |