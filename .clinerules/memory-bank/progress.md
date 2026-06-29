# 📋 Global Portfolio Engine - Progress Tracker

**Project:** global-portfolio-engine-etf-crypto
**Last Updated:** 2026-06-29
**Status:** v55.1 - Transaction Logs Fix (Stable)

## 📅 Recent Updates (2026-06-29)

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