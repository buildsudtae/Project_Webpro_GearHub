# ⚙️ Gear Hub — ระบบจัดการคลังสินค้าอุปกรณ์ IT

ระบบจัดการคลังสินค้าสำหรับอุปกรณ์เทคโนโลยีสารสนเทศ รองรับ 3 บทบาทผู้ใช้งาน พร้อมระบบ Workflow การขอเบิกสินค้า Transaction Log และ Dashboard แบบ Real-time

---

## 🎯 Pain Point

| ปัญหาเดิม | สิ่งที่ Gear Hub แก้ไข |
|-----------|----------------------|
| นับสต็อกด้วยมือ จดกระดาษ เกิด Human Error | ระบบอัปเดตสต็อกอัตโนมัติทุกครั้งที่บันทึก |
| ฝ่ายขายต้องโทรถามคลังทุกครั้ง | Workflow คำขอเบิก — ส่งผ่านระบบ อนุมัติได้เลย |
| ไม่มีประวัติย้อนหลัง ไม่รู้ว่าใครทำอะไร | Transaction Log บันทึกทุกการเคลื่อนไหว ตรวจสอบได้ 100% |

---

## ✨ ฟีเจอร์หลักตามบทบาท

### 👑 Administrator
- จัดการผู้ใช้งานทั้งหมด
- เพิ่ม / แก้ไข / ลบสินค้า
- รับสินค้าเข้าคลัง, เบิกสินค้าออก, ปรับสต็อก
- อนุมัติหรือปฏิเสธคำขอเบิกสินค้า
- ดู Dashboard, รายงาน และ Transaction Log ทั้งหมด

### 🏭 Warehouse Staff
- รับสินค้าเข้าคลัง และปรับสต็อก
- **อนุมัติหรือปฏิเสธ** คำขอเบิกสินค้าจากฝ่ายขาย
- ดูประวัติการทำรายการ

### 🛒 Sales Staff
- ค้นหาสินค้าและตรวจสอบจำนวนในคลัง
- **ส่งคำขอเบิกสินค้า** พร้อมระบุเหตุผล
- ดู Dashboard และสถานะคำขอของตนเอง

---

## 🛠️ Technology Stack

**Frontend**

| เทคโนโลยี | บทบาท |
|-----------|-------|
| HTML + EJS | โครงสร้างหน้าเว็บและ Template Engine (Dynamic rendering) |
| CSS + Bootstrap 5 | UI ที่สวยงาม, Responsive ทุกขนาดหน้าจอ |
| Bootstrap Icons | ไอคอนในเมนูและปุ่มต่าง ๆ |
| JavaScript | Autocomplete ค้นหาสินค้า, Form Validation |
| Chart.js | กราฟและแผนภูมิวงกลมในหน้ารายงาน |

**Backend**

| เทคโนโลยี | บทบาท |
|-----------|-------|
| Node.js | Runtime Environment — รัน JavaScript บนฝั่ง Server |
| Express.js | Web Framework — จัดการ Routing และ Request/Response |
| SQLite3 | ฐานข้อมูลแบบ File-based ไม่ต้องติดตั้ง Server แยก |
| Express-session | จัดการ Session ผู้ใช้ รองรับ Role-based Access Control |

---

## 🗄️ Database Design (6 ตาราง)

```
users              → ข้อมูลผู้ใช้งาน (user_id, username, password, full_name, role, email)
products           → ข้อมูลสินค้า (product_code, product_name, category_id, brand, model, price, status)
categories         → หมวดหมู่สินค้า (CPU, Video Card, Mother Board, Storage)
stock              → ยอดสต็อกจริง (warehouse_qty, storefront_qty, reorder_point)
request_from_sales → คำขอเบิกสินค้า (status: pending / approved / rejected)
stock_transactions → Log ทุกการเปลี่ยนแปลง (add, adjust, receive, dispatch, delete)
```

> `stock_transactions` คือหัวใจของระบบ — บันทึก **ผู้กระทำ, วันเวลา และหมายเหตุ** ทุกครั้งที่มีการเปลี่ยนแปลงสต็อก

---

## 🚀 วิธีติดตั้งและรันโปรเจกต์

### สิ่งที่ต้องมี
- [Node.js](https://nodejs.org/) (v16+)

### ขั้นตอน

```bash
# 1. Clone repository
git clone https://github.com/Jaturapat-Buak/GearHub.git
cd GearHub

# 2. ติดตั้ง dependencies
npm i express ejs sqlite3 express-session

# 3. รันโปรแกรม
node index.js
```

4. เปิด browser แล้วไปที่ **http://localhost:3000**

---

## 🔑 บัญชีผู้ใช้สำหรับทดสอบ

| Role | Username | Password |
|------|----------|----------|
| Administrator | `admin` | `1234` |
| Warehouse Staff | `warehouse_staff` | `1234` |
| Sales Staff | `sales_staff` | `1234` |

---

## 📖 การใช้งานระบบ

### Dashboard
หลังล็อกอิน ระบบแสดงภาพรวมคลังทันที ได้แก่ จำนวนสินค้าทั้งหมด, รับเข้าวันนี้, เบิกออกวันนี้, สินค้าใกล้หมด และประวัติรายการล่าสุด พร้อมแผนภูมิสัดส่วนประเภทสินค้า

### การแจ้งเตือน
🔔 กระดิ่งด้านบนแจ้งเตือนสินค้าที่จำนวนต่ำกว่า `reorder_point` — กดดูรายละเอียดได้ทันที

### Autocomplete
ทุกหน้าที่ต้องกรอกชื่อสินค้า ระบบจะแนะนำรายการอัตโนมัติขณะพิมพ์ พร้อมแสดงจำนวนสต็อกปัจจุบันเมื่อเลือกสินค้า

### Workflow เบิกสินค้า
```
Sales Staff กรอกคำขอ → ระบบสร้าง Request (pending) → Warehouse Staff อนุมัติ/ปฏิเสธ → สต็อกอัปเดตอัตโนมัติ
```

---

## 👥 ผู้จัดทำ

| ชื่อ - นามสกุล | รหัสนักศึกษา |
|----------------|-------------|
| นายจตุรภัทร กิติมาโภคิน | 67070017 |
| นายณัฐวุฒิ ทิพย์รัตน์ | 67070053 |
| นายอาวิษกรณ์ ตั้งประดิษฐ์ชัย | 67070200 |
| นายณัฏฐ์พงศ์ พรหมแก้ว | 67070227 |
