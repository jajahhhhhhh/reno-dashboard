# CLAUDE.md — Dashboard J (Lipa & Chaweng Renovation)

## โปรเจกต์นี้คืออะไร
ระบบติดตามการรีโนเวท 2 ที่บนเกาะสมุยของ J:
- 🏠 **ลิปะน้อย (Lipa)** — รีโนเวทบ้าน (ห้องน้ำ, ครัว)
- 🏢 **เฉวง (Chaweng)** — ตึก 3 ชั้น (ไฟฟ้าใหม่, กั้นห้อง, ป๊อปอัพสโตร์ชั้นล่าง + Airbnb ชั้น 2-3)

ผู้รับเหมา: **MR.HOME KOH SAMUI (พี่ปอ / MR THANAPAT NGAMSONG)**
Deploy: **GitHub Pages** — push แล้วเว็บอัปเดตเองใน 1-2 นาที

## ⚠️ กฎเหล็ก (ห้ามละเมิด)
1. **แยก Lipa / Chaweng เสมอ** — ทุก task, payment, inventory ต้องระบุ project ชัดเจน ห้ามรวมกันโดยไม่ได้ขอ
2. **เก็บ SKU และราคาจริงจากบิล** — HomePro, Global House ทุกใบต้องบันทึกเลขที่ออเดอร์/ใบกำกับภาษี
3. **ภาษาไทยเป็นหลัก** ใน UI และข้อมูล
4. ห้ามลบข้อมูลเก่าโดยไม่ได้รับยืนยัน

## โครงสร้างไฟล์
```
index.html              → หน้า landing (สถิติรวม อ่านจาก localStorage)
dashboard-final.html    → Kanban + การเงิน + AI อ่าน LINE
inventory-system.html   → สต็อกวัสดุ + สแกน QR (jsQR + กล้อง)
```

## ข้อมูลอยู่ตรงไหน (สำคัญที่สุด)
ข้อมูลทั้งหมด hardcode เป็น JS array ในไฟล์ HTML + persist ใน localStorage:

### dashboard-final.html
- `const T0=[...]` — งานทั้งหมด 36 รายการ
  - format: `{id, t:ชื่องาน, n:โน้ต, s:'Todo|In Progress|Review|Done', p:'Lipa|Chaweng', type:'General|Electrical|Plumbing|Demo|Structure|Finishing', cost, date:'YYYY-MM-DD', pri:'High|Medium|Low', by:'MR|W'}`
- `const P0=[...]` — payment log (จ่ายแล้ว + รอจ่าย)
  - format: `{id, date:'DD/MM/69', desc, proj:'Lipa|Chaweng', who, amount, status:'paid|pending'}`
- localStorage keys: `dj_tasks`, `dj_pays`, `dj_logs`

### inventory-system.html
- `const SAMPLE_ITEMS=[...]` — สต็อก 16+ SKU จากบิลจริง
  - format: `{id, sku, name, cat:'โคมไฟ|ไฟฟ้า|ชุดเครื่องนอน|ห้องน้ำ|เครื่องมือ|เฟอร์นิเจอร์|อื่นๆ', icon, proj:'Lipa|Chaweng|Both', qty, minQty, unit, price, loc, note}`
- localStorage keys: `stock_items`, `stock_txs`

## งานที่ทำบ่อย

### 1. เพิ่ม payment จากบิลใหม่
แก้ array `P0` ใน dashboard-final.html — เพิ่ม object ใหม่ พร้อมเลขที่ออเดอร์ในฟิลด์ desc
**หมายเหตุ:** ถ้า user เคยเปิดเว็บแล้ว localStorage จะ override ข้อมูลใหม่ — บอก user ให้กด clear localStorage หรือเพิ่มผ่าน UI แทน

### 2. เพิ่มงานจาก LINE export
user อัปโหลด .txt จาก LINE → อ่านและแยกงาน/เงินตาม format ข้างบน → อัปเดต T0/P0
- ##ลิปะน้อย หรือพูดถึง "บ้าน" = Lipa
- ##เฉวง, "ตึก", "3 ชั้น" = Chaweng
- "เบิก" = MR ขอเบิกเงิน → payment

### 3. เพิ่มสินค้าเข้าสต็อกจากบิล
แก้ `SAMPLE_ITEMS` ใน inventory-system.html — ใช้ SKU จริงจากใบเสร็จ

### 4. Deploy
```bash
git add -A && git commit -m "update" && git push
```
GitHub Pages rebuild เอง ~1-2 นาที URL: https://jajahhhhhhh.github.io/reno-dashboard/

## บริบทธุรกิจ (เฉวง)
- งบรวม ~750,000 บาท | เซ้งระยะยาว จ่ายล่วงหน้าแล้ว
- ชั้น 1: self-service (ซักผ้าหยอดเหรียญ, ตู้กดน้ำ/ขนม, ตู้เกม) — IoT อัตโนมัติ คนน้อย
- ชั้น 2-3: Airbnb สไตล์ Japandi (ดีไซน์โดย Waw / e8ightdesign)
- ⚠️ มีปัญหาของหายที่ไซต์เฉวง (เหล็ก, แม่เหล็กประตู, สายไฟ) — ยังไม่ปิดเคส

## คนที่เกี่ยวข้อง
| ชื่อ | บทบาท |
|---|---|
| พี่ปอ / MR.HOME | ผู้รับเหมาหลักทั้ง 2 ไซต์ |
| Joy | จัดซื้อวัสดุ (บางบิลออกในนาม Joy) |
| W.ch | ผู้ร่วมตัดสินใจ |
| Waw (e8ightdesign) | ดีไซเนอร์ Japandi + บาร์ |
| Farid | ดูแลบาร์ |

## บริษัทสำหรับใบกำกับภาษี
บริษัท นิกเชน จำกัด — Tax ID 0845568023436 (เกาะสมุย)
