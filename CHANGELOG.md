# Changelog

## 2026-05-06

### ✅ Dashboard / Backtest Engine
- ปรับ logic การคำนวณเมื่อเปิด `DCA`:
  - ยังทำ `rebalance` ตามรอบปกติ
  - นำเงิน `DCA inflow` ไปรวมในฐานคำนวณ `rebalance` ด้วย
- ปรับ Transaction log ให้รองรับ event แบบรวมวันเดียว (`DCA_REBAL`) และลดปัญหา log ซ้ำ
- แก้การคำนวณ `avgTO` ให้ไม่ถูกนับผิดจาก DCA-only event
- ปรับ RSI เป็น **Wilder's Smoothed RSI**
- ปรับ performance บางจุด:
  - ใช้ map lookup แทน `indexOf/find` ซ้ำในกราฟและ surface

### ✅ Growth Analysis
- เพิ่มเส้น `Real Cost (Initial + DCA)`
- แก้ `Real Cost` ให้เพิ่มตาม `inflow` ได้ถูกต้อง (รวมกรณี `DCA_REBAL`)
- ปรับ benchmark ให้รองรับการคำนวณแบบมี DCA ตาม result ที่เลือก

### ✅ Main Chart UX
- เพิ่มปุ่มสลับมุมมองกราฟ:
  - `Growth`
  - `Asset Price`
- เพิ่มกราฟราคา asset ในตำแหน่งเดียวกับ Growth Analysis
- แก้ปัญหาสลับไป `Asset Price` แล้วกราฟ Growth ไม่ซ่อน
  - เปลี่ยนการ toggle จากระดับ canvas เป็น wrapper
- ปรับกราฟ `Asset Price` ให้ auto ใช้ **log scale** เมื่อช่วงราคาห่างกันมาก

### ✅ Transaction Logs
- รวมการแสดงผล `Injection + Rebalance` ในวันเดียวกันเป็นรายการเดียว
- รวมผล Smart Allocation เข้ากับ `% of Portfolio` ที่ต้องปรับ
- เพิ่มแสดงมูลค่า `$` ในส่วน Holdings

### ✅ Portfolio Tracker
- ย้ายบล็อก `สรุปตาม Portfolio` ไปไว้เหนือ `รายการลงทุน`
- ย้าย/จัดตำแหน่งตัวกรองให้อยู่เหนือรายการลงทุน
- ตั้งหัวตารางรายการลงทุนเป็น **sticky** ขณะ scroll
- จำกัดความสูงรายการลงทุนให้แสดงประมาณ 10 แถว และเกินให้ scroll
- ปรับราคาในตารางรายการลงทุนให้แสดงตามข้อมูลจริง (ไม่บังคับปัด 2 ตำแหน่ง)
- แก้สรุปตาม Portfolio ให้คำนวณครบทุกพอร์ต ไม่ผูกกับพอร์ตที่ load config เท่านั้น
- แก้การประเมินมูลค่า/กำไรสำหรับพอร์ตผสม ETF+Crypto:
  - ใช้ข้อมูลราคาแบบราย asset (Engine data + cache fallback)
  - รองรับการคำนวณข้ามโหมดได้ดีขึ้น

### ✅ Fetch Market Data
- ปรับปุ่ม `Fetch Market Data` ให้ fetch ครอบคลุมทุก asset ที่เกี่ยวข้อง:
  - ETF universe
  - Crypto universe
  - Universe ปัจจุบัน
  - Benchmark
  - Assets ที่อยู่ใน Portfolio Tracker
- ปรับ fetch เป็นแบบ **per-asset source routing**:
  - ETF → Yahoo
  - Crypto → Binance
- ปรับระบบ cache ให้รองรับ mode ต่อ asset (`ETF` / `CRYPTO`) ได้ชัดเจนขึ้น

---

> หมายเหตุ: รายการนี้สรุปการแก้ไขรอบล่าสุดในไฟล์หลัก `index.html`
