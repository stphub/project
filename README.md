# project
#การทำงานของอุปกรณ์เเต่ละตัว
1) Arduino Nano

หน้าที่หลัก:

ควบคุมการทำงานทั้งหมดของ GyroGlove

อ่านค่าความเร่งและแรงสั่นจาก MPU6050

ประมวลผลแรงสั่น → แปลงเป็นสัญญาณ PWM เพื่อควบคุมมอเตอร์

ตรวจ Push Button เพื่อเปิด/ปิดวงจร

2) MPU6050 (GY-521)

หน้าที่หลัก:

เซนเซอร์ 6 แกน (3 แกน accelerometer + 3 แกน gyroscope)

ตรวจจับแรงสั่น / การเคลื่อนไหวของมือผู้สวมถุงมือ

ส่งข้อมูลผ่าน I2C (SDA/A4, SCL/A5) ไปยัง Arduino

ข้อมูลนี้จะถูก Arduino ใช้คำนวณ PWM ควบคุมมอเตอร์

3) แบตเตอรี่ Li-ion 18650 ×2 (อนุกรม)

หน้าที่หลัก:

เป็นแหล่งจ่ายไฟหลักของวงจร

ให้แรงดันรวม ≈7.4V

ต่อเข้ากับ Vin Arduino → Arduino ลดแรงดันเหลือ 5V

จ่ายไฟให้ MOSFET + Motor + Capacitor + MPU6050

4) MOSFET N-channel (AO3400 / IRLZ44N / IRLZ34N)

หน้าที่หลัก:

ทำหน้าที่เป็น สวิตช์อิเล็กทรอนิกส์

Gate รับสัญญาณ PWM จาก Arduino → เปิด/ปิด Motor

Source → GND, Drain → Motor −

Pull-down 10kΩ → ทำให้ MOSFET ปิดแน่นอนเมื่อ Arduino ยังไม่ส่งสัญญาณ

5) Vibration Motor 1034

หน้าที่หลัก:

สร้างแรงสั่นเพื่อ counteract tremor ของผู้ป่วยพาร์กินสัน

รับไฟ 3–5V จาก MOSFET → PWM จาก Arduino ควบคุมแรงสั่น

6) Flyback Diode (1N5819)

หน้าที่หลัก:

ป้องกันแรงดันย้อน (back EMF) จาก Motor → ป้องกันวงจร Arduino / MOSFET เสียหาย

Cathode → Motor +, Anode → Motor −

7) Capacitor

Electrolytic 1000µF / 16V → กรองไฟความถี่ต่ำ / ลด voltage drop เวลามอเตอร์ทำงาน

Ceramic 0.1µF → กรองสัญญาณรบกวนความถี่สูง / ripple

ต่อ ขนานกับ Motor + / GND → ทำให้วงจรไฟนิ่งและ Arduino อ่านค่า MPU6050 ได้แม่นยำ

8) Push Button

หน้าที่หลัก:

เปิด / ปิดวงจร GyroGlove

ต่อ D2 → GND ใช้ INPUT_PULLUP ใน Arduino

กดปุ่ม → Arduino รู้ว่าต้องเริ่มหรือหยุดการทำงาน


----------------------------------------------------------------------------------------------------------------------------


อุปกรณ์หลักและหน้าที่

Arduino Nano

ควบคุมวงจรทั้งหมด

อ่านค่าจาก MPU6050

แปลงค่าการสั่นเป็น PWM ควบคุมมอเตอร์

ตรวจ Push Button เพื่อเปิด/ปิดระบบ

MPU6050 (GY-521)

เซนเซอร์ 6 แกน (Accelerometer + Gyroscope)

ตรวจจับแรงสั่น / การเคลื่อนไหวของมือ

ส่งข้อมูลผ่าน I2C (SDA/A4, SCL/A5) ไป Arduino

แบตเตอรี่ Li-ion 18650 ×2 (อนุกรม)

แหล่งจ่ายไฟหลัก 7.4V

ต่อเข้ากับ Vin Arduino → Arduino ใช้ 5V ภายใน

จ่ายไฟให้ MOSFET, Motor, Capacitor, MPU6050

MOSFET N-channel (AO3400 / IRLZ44N / IRLZ34N)

ทำหน้าที่เป็นสวิตช์อิเล็กทรอนิกส์

Gate รับสัญญาณ PWM จาก Arduino → เปิด/ปิด Motor

Gate → Arduino D3/D5 ผ่าน 200Ω

Gate → GND ผ่าน 10kΩ Pull-down

Source → GND, Drain → Motor −

Vibration Motor 1034

สร้างแรงสั่นเพื่อ counteract tremor

Motor + → แบต 7.4V, Motor − → Drain MOSFET

Flyback Diode (1N5819 / SS14)

ป้องกันแรงดันย้อนจากมอเตอร์

Cathode → Motor +, Anode → Motor −

Capacitor

Electrolytic 1000µF / 16V → ขนาน Motor + / GND → ลด voltage drop

Ceramic 0.1µF → ขนานใกล้ Electrolytic → กรองสัญญาณรบกวนความถี่สูง

Push Button

เปิด/ปิดระบบ

ต่อ D2 → GND ใช้ INPUT_PULLUP ใน Arduino

กดปุ่ม → Arduino รู้ว่าจะเริ่มหรือหยุดวงจร

Resistors

200Ω → Gate resistor MOSFET

10kΩ → Pull-down Gate MOSFET

4.7kΩ → Pull-up SDA/SCL (ถ้า MPU6050 ไม่มี)

สายและอุปกรณ์ต่อพ่วง

สาย Power 20–22 AWG → แดง/ดำ + / GND

สาย Signal 24 AWG / Jumper → SDA/SCL, Gate, Push Button

Header / Female Header → Arduino Nano

JST / Molex → ต่อแบตถอดได้ (ถ้าต้องการ)

อุปกรณ์เสริม

Fuse / Polyfuse 1–2A → ป้องกันลัดวงจร

Power Switch (Toggle) → ต่อขั้ว + จาก holder ไป Arduino (ถ้าไม่ใช้ Push Button)

Heat-shrink tubing / เทปฉนวน → ป้องกันลัดวงจร
