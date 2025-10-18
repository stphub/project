# code
ใส่แบต 18650 2 ก้อนในรางต่อเป็น series (ตรวจ +/− ถูกต้อง) → จะได้ ~7.4V nominal

ก่อนต่อโหลด อย่ายังไม่เสียบ Arduino ให้ต่อสายจากราง (สาย + และ −) เข้ากับ IN+ / IN− ของ MT3608

ต่อมัลติมิเตอร์ไปวัดที่ MT3608 OUT+ / OUT− แล้วปรับสกรู VR จนได้ 5.00 V (no load) — ทำขั้นตอนนี้ให้เสร็จ ก่อน ต่อ Arduino หรืออุปกรณ์ใดๆ

B — สร้าง rail power และ capacitor
4. ตัดสาย Power (20–22 AWG) สั้นๆ สองเส้น (แดง/ดำ) สำหรับ OUT+ / OUT− ออกจาก MT3608 ไปเป็นจุดกระจาย (node)
5. ต่อ Electrolytic 1000 µF ขนานระหว่าง OUT+ (ขั้ว+) และ OUT− (ขั้ว−) ของ MT3608
6. ขนานอีกตัวด้วย Ceramic 0.1 µF ใกล้ๆ ตัว 1000 µF (ขา+ -> OUT+, ขา− -> OUT−) — ถ้าไม่มี 0.1 µF ก็ไปก่อนได้แต่แนะนำใส่

C — ต่อ Arduino กับ MPU6050
7. MT3608 OUT+ → 5V pin ของ Arduino Nano (หรือ VIN ถ้าบอร์ดต้องการ แต่ถ้า OUT=5V ให้ต่อเข้า 5V)
8. MT3608 OUT− → GND ของ Arduino Nano (common ground)
9. MPU6050: VCC → 5V ; GND → GND ; SDA → A4 ; SCL → A5

ถ้าโมดูล MPU6050 ไม่มี pull-ups ให้ต่อ 4.7 kΩ ระหว่าง SDA→5V และ SCL→5V

D — ต่อมอเตอร์ + MOSFET + diode (ทำซ้ำ 2 ครั้ง สำหรับ A และ B)
10. Motor A: ขั้ว + ของ MotorA → MT3608 OUT+ (ใช้สาย 20–22 AWG สั้นๆ)
11. Motor A: ขั้ว − ของ MotorA → Drain ของ MOSFET_A
12. MOSFET_A Source → GND (MT3608 OUT−)
13. Gate MOSFET_A ← ต่อจาก Arduino D3 ผ่าน 100 Ω
14. Gate MOSFET_A → ต่อ 100 kΩ ลง GND (pull-down)
15. Flyback Diode (1N5819): Cathode (แถบ) → MotorA +, Anode → MotorA − (Drain)
16. ซ้ำข้อ 10–15 สำหรับ Motor B โดยใช้ MOSFET_B และ Arduino D5

E — ปุ่มเปิด/ปิด
17. ต่อ Push button แบบใช้ INPUT_PULLUP:
- หนึ่งขา → Arduino D2
- อีกขา → GND
(โค้ดจะตั้ง INPUT_PULLUP ดังนั้นไม่ต้องใช้ resistor เพิ่ม ถ้าใช้แบบอื่น ให้ต่อ pull-down 10k ตามต้องการ)

F — ตรวจสอบก่อนจ่ายไฟ
18. ตรวจสอบความเชื่อมต่อขั้ว + / − ทุกจุดอีกครั้ง (มัลติมิเตอร์ช่วยเช็ก)
19. วัดที่ MT3608 OUT+ อีกครั้ง ให้แน่ใจเป็น 5.00 V ก่อนเสียบ Arduino
20. ตรวจสอบว่า MOSFET ไม่ต่อกลับขา (Drain/Source/Gate ถูกต้อง) และไดโอดต่อถูกทิศ

-------------------------------------------------------------------------------------------------------------------------------------------

1) Arduino Nano
ควบคุมการทำงานทั้งหมด อ่านค่าเซนเซอร์ MPU6050 และสั่งมอเตอร์
ขาเชื่อมต่อหลัก:
D2 → Push Button
D3 → Gate MOSFET Motor A
D5 → Gate MOSFET Motor B
A4 (SDA) / A5 (SCL) → MPU6050
5V / GND → Power จาก MT3608

2) เซนเซอร์ MPU6050 (GY-521)
ตรวจจับการสั่น / การเคลื่อนไหว 6 แกน
ขาเชื่อมต่อ:
VCC → 5V Arduino
GND → GND Arduino
SDA → A4
SCL → A5
*** ถ้าโมดูลไม่มี pull-up ให้ต่อ 4.7 kΩ จาก SDA→5V และ SCL→5V

3) Boost Converter MT3608
แปลงไฟจากแบต 7.4V (18650 ×2) ลงเหลือ 5V สำหรับ Arduino + มอเตอร์
ขาเชื่อมต่อ:
IN+ → + แบต
IN− → − แบต
OUT+ → 5V Arduino / Motor +
OUT− → GND Arduino / Motor −
การปรับค่า:
ก่อนต่อ Arduino → วัด OUT+ / OUT− ด้วยมัลติมิเตอร์
ปรับสกรู VR จนได้ 5.00 V (no load)

4) แบตเตอรี่ Li-ion 18650 แหล่งพลังงานสำหรับ Boost + Arduino + Motor
จำนวน: 2 ก้อนต่ออนุกรม → 7.4V nominal
ตัวเลือก: Protected 18650 + Holder 2-slot
ขั้วต่อ:
สาย AWG 20–22 ต่อจากแบต + / − ไปยัง Boost Converter

5) Vibration Motor 1034
สร้างแรงสั่น counteract tremor
ขนาด: 10×3.4 mm, 3–5V, 100–300 mA
ขาเชื่อมต่อ:
→ MT3608 OUT+
− → Drain ของ MOSFET

6) MOSFET N-channel (AO3400 / IRLZ44N / IRLZ34N) เปิด/ปิด Motor ตาม PWM จาก Arduino
ขาเชื่อมต่อ:
Gate → Arduino D3 (Motor A) / D5 (Motor B) ผ่าน 100 Ω
Source → GND
Drain → Motor −
Pull-down Gate: 100 kΩ → GND

7) Flyback Diode 1N5819 ป้องกันแรงดันย้อนจากมอเตอร์
ขาเชื่อมต่อ:
Cathode (ขีด) → Motor +
Anode → Motor − (Drain)

8) Capacitor กรองไฟ
Electrolytic 1000 µF / 16V → ขนาน OUT+ / OUT− ของ MT3608
Ceramic 0.1 µF → ขนานใกล้ตัว 1000 µF (กรอง high-frequency)

9) Push Button
บทบาท: เปิด/ปิดระบบ
ขาเชื่อมต่อ:
หนึ่งขา → Arduino D2
อีกขา → GND
หมายเหตุ: ใช้ INPUT_PULLUP ในโค้ด
