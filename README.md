1) Arduino Nano

ควบคุมทั้งหมด: อ่าน MPU6050, สั่งมอเตอร์, ตรวจ Push Button

ขาเชื่อมต่อ:

D2 → Push Button (ใช้ INPUT_PULLUP)

D3 → Gate MOSFET Motor A ผ่าน 200Ω

D5 → Gate MOSFET Motor B ผ่าน 200Ω

A4 / A5 → SDA / SCL ของ MPU6050

Vin → +7.4V แบตเตอรี่, GND → GND รวมทุกส่วน

2) MPU6050 (GY-521)

VCC → 5V Arduino

GND → GND Arduino

SDA → A4, SCL → A5

ถ้าไม่มี pull-up → ต่อ 4.7kΩ จาก SDA→5V และ SCL→5V

3) แบตเตอรี่ Li-ion 18650 ×2 (อนุกรม)

7.4V nominal → ต่อ ตรงไป Vin Arduino

GND ต่อ GND Arduino + MOSFET + Motor + Capacitor

4) MOSFET + มอเตอร์

Gate → D4 / D5 ผ่าน 200Ω

Gate → GND ผ่าน 10kΩ pull-down ทั้ง2ตัว

Source → GND

Drain → Motor −

Motor + → แบต +7.4V

5) Flyback Diode (1N5819)

Cathode (ขีด) → Motor +

Anode → Motor − (Drain)

ต่อขนานกับมอเตอร์

6) Capacitor

Electrolytic 1000µF / 16V → ต่อ ขนาน Motor + / GND

Ceramic 0.1µF → ต่อขนานใกล้ Electrolytic

7) Push Button

ขาหนึ่ง → D2

ขาอีกขา → GND

ใช้ INPUT_PULLUP ในโค้ด

กดปุ่ม → Arduino เริ่ม/หยุดวงจร


------------------------------------------------------------------------------------------------------------------------------------- 


1) แหล่งจ่ายไฟ

 แบตเตอรี่ / Power Bank ให้แรงดัน 5V–7.4V (ตาม Arduino)

 + ของแหล่งจ่ายไฟต่อไปยัง Vin (ถ้า >7V) หรือ 5V Pin (ถ้า 5V)

 GND ต่อร่วมกับ Arduino, MOSFET, มอเตอร์, MPU6050

2) สวิตช์เปิด/ปิด

 ต่อ สวิตช์ 2 ขา บนสาย + ของแหล่งจ่ายไฟ

 สวิตช์ ON → Arduino เริ่มทำงาน

 สวิตช์ OFF → วงจรทั้งหมดหยุด

3) MOSFET + มอเตอร์

 Gate ต่อ Arduino PWM pin (ตัวอย่าง D3 / D4) ผ่าน Resistor 220Ω

 Gate → GND ผ่าน Pull-down 10kΩ

 Source → GND ของวงจร

 Drain → ขั้ว - ของมอเตอร์

 ขั้ว + ของมอเตอร์ → +5V ของวงจร

4) MPU6050

 VCC → 5V Arduino

 GND → GND Arduino

 SDA → A4 (Arduino Uno)

 SCL → A5 (Arduino Uno)

 ตรวจสอบ Serial Monitor → ค่า Ax, Ay, Az แสดงปกติ

5) Capacitor

 Electrolytic 1000µF / 16V

ต่อ +5V / Vin

ต่อ GND

 Ceramic 100µF (หรือมากกว่า)

ต่อขนานกับ +5V / GND ใกล้ Arduino / MPU6050

6) สายและการต่อ

 GND ต่อร่วมทุกจุด (Arduino, MOSFET, มอเตอร์, MPU6050, Capacitor)

 สาย + และ - ของมอเตอร์ต่อถูกต้อง (Drain / Source / +5V)

 ตรวจสอบว่าขั้ว Capacitor Electrolytic ถูกต้อง

7) ทดสอบ

 เปิดสวิตช์ → Arduino รันโค้ดอัตโนมัติ

 MPU6050 อ่านค่าความเร่ง / แรงสั่น

 มอเตอร์ตอบสนองตามแรงสั่น PWM (สลับ 2 ตัว)

 ตรวจสอบ Serial Monitor → ค่าความสั่นไม่เป็น 0
