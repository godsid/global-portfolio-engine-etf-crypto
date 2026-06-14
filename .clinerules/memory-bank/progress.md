# 📋 Global Portfolio Engine - Progress Tracker

**Project:** global-portfolio-engine-etf-crypto
**Last Updated:** 2026-06-14
**Status:** Daily Telegram Scheduler (v55.0) - Almost Done

---

## 🎯 Task Overview

### 8️⃣ Daily Telegram Scheduler + Background Auto-Run

**Status:** ✅ COMPLETED
**Priority:** 🔴 High
**Completed:** 2026-06-14
**Files Modified:** `index.html`

**Tasks:**
- [x] สร้าง SnapshotManager (เก็บ daily NAV ใน IndexedDB)
- [x] สร้าง TelegramScheduler (5 นาที interval, multi-browser lock)
- [x] สร้าง DailyReportGenerator (today/7d/30d P&L, strategy performance)
- [x] เพิ่ม Telegram Settings UI ใน Sidebar
- [x] เพิ่ม Daily Report Tab
- [x] เพิ่ม Rebalance Target Editor (UI + Save/Load)
- [x] Encrypt Bot Token ด้วย key เดียวกับ .gpe file
- [x] อัปเดต version → v55.0 (title tag)
- [x] Git commit และ push

**Implementation Details:**

1. **SnapshotManager** ✅
   - Key: `gpe_snapshots` ใน IndexedDB
   - Schema: `{portfolio, date, totalInvested, marketValue, cashBalance, holdings, strategyResults}`
   - Methods: `saveDailySnapshot()`, `getHistoricalRange()`, `calculatePnLDelta()`

2. **TelegramScheduler** ✅
   - Interval: 5 นาที (300,000 ms)
   - Lock: localStorage `{instanceId, timestamp}` expire 10 นาที
   - Token storage: Encrypt ด้วย CryptoUtils
   - Methods: `init()`, `tick()`, `acquireLock()`, `releaseLock()`, `runDailyReport()`

3. **DailyReportGenerator** ✅
   - P&L Summary: Today, 7d, 30d delta
   - Strategy Performance: ตาม profile.strategy (ALL = top 3)
   - Rebalance Check: target % vs current %

4. **Rebalance Target Editor** ✅
   - UI: Modal สำหรับกำหนด target % แต่ละ asset
   - Storage: localStorage via RebalanceTargetManager
   - Auto-load: โหลด target ที่บันทึกไว้เมื่อเปิด portfolio
   - Save/Load buttons: บันทึกและโหลด target %

---

## ✅ Completed

- [x] วิเคราะห์สถานะปัจจุบันของระบบ
- [x] วางแผนสำหรับ Daily Telegram Scheduler
- [x] บันทึก task ไว้ใน progress.md
- [x] สร้าง SnapshotManager
- [x] สร้าง TelegramScheduler
- [x] สร้าง DailyReportGenerator
- [x] เพิ่ม Telegram Settings UI
- [x] เพิ่ม Daily Report Tab
- [x] เพิ่ม Rebalance Target Editor
- [x] Encrypt Bot Token
- [x] Version update to v55.0
- [x] Git commit & push

---

## 📝 Notes

- ใช้ `setInterval(5 * 60 * 1000)` แทน cron (เพราะ browser-based)
- Bot Token encrypt ด้วย CryptoUtils แล้ว
- Multi-browser lock ใช้ localStorage + timestamp (expire 10 นาที)
- Telegram message ใช้ Markdown formatting
- Snapshot เก็บใน IndexedDB เพื่อไม่กิน localStorage quota
- Rebalance Target บันทึกใน localStorage ผ่าน RebalanceTargetManager

## 🔲 Next Tasks

1. **Telegram Bot Testing** - ทดสอบการส่งข้อความจริง
2. **Push Notification** - เพิ่ม push notification สำหรับ portfolio alerts
