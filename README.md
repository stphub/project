# project
อิเล็กทรอนิกส์หลัก

Arduino Nano — 1

MPU6050 module — 1

MT3608 DC-DC Boost (หรือ boost module รองรับ ≥1A–2A) — 1
หมายเหตุ: ถ้าคาดจะสั่นต่อเนื่องหนัก แนะนำหาโมดูล boost ที่ระบุรองรับกระแส ≥2A (เช่น MP2307/XL6009 แบบแรงสม่ำเสมอ)

Li-ion 18650 cell (protected) — 2 (แนะนำ) — พร้อม battery holder (2-slot หรือ 2×1 แบบที่มีสาย)

Vibration Motor 1034 (10×3.4 mm) — 2

N-channel MOSFET logic-level (เช่น AO3400, IRLZ44N, IRLZ34N) — 2

Flyback diode (Schottky, เช่น 1N5819 หรือ SS14) — 2 (1 ตัว/มอเตอร์)

Capacitor Electrolytic 1000 µF / 16V (หรือ 25V) — 1 (ขนานที่ OUT ของ boost)

Ceramic 0.1 µF (100 nF) — 1 (ขนานใกล้ IC เพื่อกรอง high-freq)

Resistor 100 Ω — 2 (gate resistor สำหรับ MOSFET)

Resistor 100 kΩ — 2 (gate pull-down สำหรับ MOSFET)

Resistor 4.7 kΩ — (ถ้าบอร์ด MPU6050 ไม่มี pull-up) 2 ชิ้น (pull-up SDA/SCL)

Resistor 10 kΩ — 1 (ถ้าต้องการ pull-down/pull-up ปุ่ม)

Push button (toggle หรือ momentary + debouncing ในโค้ด) — 1

หมุด header / female header (สำหรับ Arduino Nano) — ตามต้องการ

สายและการเชื่อมต่อ

สาย Power (stranded) 20 AWG หรือ 22 AWG — ประมาณ 1 m (แดง/ดำ)

สาย Signal 24 AWG หรือ jumper dupont — ชุด (สำหรับ SDA/SCL, gate, ปุ่ม)

JST connector / Molex / XT30 (ถ้าต้องการ) — ตามต้องการ (สำหรับแบตถอดได้)

Heat-shrink tubing, เทปฉนวน

อุปกรณ์งานบัดกรี/เครื่องมือ

บอร์ดทดลอง (breadboard) — 1 (ทดสอบ)

PCB / Perfboard ถ้าจะบัดกรีถาวร — 1

หัวแร้ง + ตะกั่ว/เฟลักซ์, หัวต่อ, คีมปอกสาย, มัลติมิเตอร์

ตัวเลือกเสริม (แนะนำ)

Fuse หรือ Polyfuse 1–2 A — 1 (ป้องกันลัดวงจรจากแบต)

Power switch (Toggle) — 1 (ต่อขั้ว + จาก holder ไป boost)

Heat sink เล็กๆ หรือการระบายความร้อนให้ boost ถ้าทำงานหนัก

DRV2605 / Haptic driver — (optional) ถ้าต้องการ haptic คุณภาพสูงกว่า
