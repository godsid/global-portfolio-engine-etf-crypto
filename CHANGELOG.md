# Changelog

## v53.0 — 2026-05-29

### Fixed
- **Portfolio value not decreasing on sell (critical bug fix)**: เมื่อขาย asset แล้วมูลค่า portfolio ไม่ควรลดลง เพราะเงินจากการขายกลายเป็น USD cash
  - `updateSummary()`: แยกคำนวณ `totalInvested` (buy only) และ `totalCashFromSales` (sell only) แทนการใช้ net cost
  - `updateSummary()`: คำนวณ `totalValue = holdings market value + cash from sales` แทนการใช้ holdings เฉยๆ
  - `renderPortfolioSummary()`: เพิ่ม `cashFromSales` field และคำนวณ cost แบบ proportional (ลดต้นทุนตามสัดส่วนเมื่อขาย)
  - Holdings cost tracking: ใช้ average cost method — เมื่อขายจะลด cost ตามสัดส่วนของจำนวนที่ขาย

### Changed
- **Version bump**: v52.0 → v53.0

---

## v52.0 — 2026-05-28

### Changed
- **Version bump**: v51.0 → v52.0
- **P&L Chart Modal layout**: Changed pie chart section from 50% to 30% width (7:3 ratio) for better chart visibility

---

## v51.0 — 2026-05-27

### Added
- **Thai Market Support**: New market type option for SET (Thailand) with 250 trading days/year
- **Thai Universe**: Pre-configured Thai stocks (PTT.BK, CPF.BK, BDMS.BK, KBANK.BK, etc.)
- **Thai Market Quick Add**: Quick add dropdown with SET Index, Energy, Banking, Services categories
- **THAI_UNIVERSE constant**: 14 popular Thai stocks with .BK suffix

### Changed
- **Version bump**: v50.0 → v51.0
- **normalizeMarketMode()**: Now handles "THAI" mode
- **setMarketMode()**: Updated to handle Thai market with 250 trading days

---

## v50.0 — 2026-05-26

### Added
- เพิ่มระบบ **Encryption Key Manager** — Dialog สำหรับตั้งค่า/จัดการ Encryption Key
- เพิ่ม Dialog สำหรับ **Export Configs พร้อม Encryption** — รองรับ .gpe (encrypted) format
- เพิ่มระบบ **Import Configs พร้อม Decryption** — รองรับทั้ง .json (legacy) และ .gpe (encrypted)
- เพิ่ม `CryptoUtils` — XOR cipher สำหรับเข้า/ถอดรหัสไฟล์ export
- เพิ่มปุ่ม **Key Settings** ใน sidebar สำหรับจัดการ Encryption Key
- เพิ่ม **Portfolio Rebalance Calculator** — Modal สำหรับคำนวณและแสดงรายการซื้อ/ขายเพื่อปรับสัดส่วน

### Changed
- **Version bump:** v49.0 → v50.0 (Liquid Alpha Edition)
- ปรับ `exportAllConfigs()` ให้เข้ารหัสไฟล์ด้วย Encryption Key ก่อน export
- ปรับ `importConfigs()` ให้ถอดรหัสไฟล์ .gpe อัตโนมัติหากมี Encryption Key
- ปรับ title และ branding เป็น "Liquid Alpha Edition"

---

## v49.0 — 2026-05-21

### Added
- เพิ่มระบบ IndexedDB Manager สำหรับเก็บข้อมูล cache (แทนที่ localStorage เพื่อรองรับข้อมูลขนาดใหญ่)
- รองรับ fallback ไปใช้ localStorage หาก IndexedDB ไม่พร้อมใช้งาน
- เพิ่ม P&L Chart Modal — กราฟแสดงกำไร/ขาดทุนตามช่วงเวลาของแต่ละ Portfolio (ปุ่ม chart icon ข้าง "สรุปตาม Portfolio")

### Changed
- **DCA Monthly Budget:** เปลี่ยน DCA จาก "ต่อรอบ rebalance" เป็น "งบประมาณรายเดือน" — ระบบคำนวณจำนวน rebalance cycles ในเดือนนั้น และแบ่งงบเท่าๆ กันต่อ cycle (DCA Monthly ($) label ใน UI)
- ปรับปรุงระบบ cache ของ Engine ให้ใช้ IndexedDB เป็นหลัก
- ปรับ clearCache ให้ล้างทั้ง IndexedDB และ localStorage cache

---

## v48.5 — 2026-05-14

### Changed
- เพิ่มแสดงชื่อ Config ที่ load อยู่ใน sidebar header (Config: [ชื่อ config])
- ย้ายปุ่ม "เพิ่มรายการ" จาก header ของ Portfolio Tracker tab ไปไว้ใน card "รายการลงทุน"
- ปรับขนาดปุ่มให้เล็กลง (text-xs) เพื่อให้สอดคล้องกับ layout ของ card

## v48.4 — 2026-05-06

### Added
- เพิ่มเส้น `Real Cost (Initial + DCA)` ใน Growth Analysis
- เพิ่มปุ่มสลับมุมมองกราฟ `Growth` / `Asset Price`
- เพิ่มกราฟราคา asset ในตำแหน่งเดียวกับ Growth Analysis

### Changed
- ปรับ logic เมื่อเปิด `DCA` ให้ยังคง rebalance และนำ `DCA inflow` ไปคำนวณรวมใน rebalance
- ปรับ Benchmark ให้คำนวณแบบมี DCA ตาม result ที่เลือก
- ปรับโครงสร้าง Transaction Logs ให้รองรับ event รวมวันเดียว (`DCA_REBAL`)
- รวม Smart Allocation เข้ากับ `% of Portfolio` ใน Transaction Logs
- เพิ่มมูลค่า `$` ในส่วน Holdings ของ Transaction Logs
- ย้าย `สรุปตาม Portfolio` ไปไว้เหนือ `รายการลงทุน`
- ย้าย/จัดตำแหน่งกรอบ `กรองตาม Portfolio` ให้อยู่เหนือรายการลงทุน
- ตั้งหัวตารางรายการลงทุนเป็น sticky ระหว่าง scroll
- จำกัดความสูงรายการลงทุนให้แสดงประมาณ 10 แถว และเกินให้ scroll
- ปรับรูปแบบแสดงราคาในรายการลงทุนให้แสดงตามข้อมูลจริง (ไม่บังคับปัด 2 ตำแหน่ง)
- ปรับ `Fetch Market Data` ให้ครอบคลุมทุก asset ที่เกี่ยวข้อง (ETF/Crypto universe, universe ปัจจุบัน, benchmark, assets ใน Portfolio Tracker)
- ปรับ fetch เป็น per-asset source routing (ETF → Yahoo, Crypto → Binance)
- ปรับระบบ cache ให้รองรับ mode ต่อ asset (`ETF` / `CRYPTO`)
- ปรับกราฟ `Asset Price` ให้ auto ใช้ log scale เมื่อช่วงราคาห่างกันมาก

### Fixed
- แก้ `avgTO` ที่เคยนับผิดจาก DCA-only event
- แก้ `Real Cost` ไม่เพิ่มในบางกรณี (รวมกรณี `DCA_REBAL`)
- แก้ปัญหาสลับไป `Asset Price` แล้วกราฟ Growth ไม่ซ่อน (เปลี่ยน toggle จาก canvas เป็น wrapper)
- แก้การคำนวณสรุปตาม Portfolio ที่ไม่ครบเมื่อโหลด config คนละโหมด (ETF/Crypto)
- แก้ปัญหาค่าคำนวณใน `สรุปตาม Portfolio` ถูก reset เมื่อเลือก filter
- ปรับ RSI เป็น **Wilder's Smoothed RSI**
- ปรับ performance บางจุดด้วย map lookup แทน `indexOf/find` ซ้ำในกราฟและ surface

---

> หมายเหตุ: รายการนี้สรุปการแก้ไขรอบล่าสุดในไฟล์หลัก `index.html`