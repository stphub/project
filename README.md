# code
#include <Wire.h>
#include <MPU6050.h>

MPU6050 mpu;

// กำหนดขามอเตอร์
int motor1 = 3;  // MOSFET ตัวที่ 1 (PWM)
int motor2 = 4;  // MOSFET ตัวที่ 2 (PWM)

void setup() {
  Serial.begin(9600);
  Wire.begin();
  mpu.initialize();

  pinMode(motor1, OUTPUT);
  pinMode(motor2, OUTPUT);

  // ตรวจสอบการเชื่อมต่อ MPU6050
  if (mpu.testConnection()) {
    Serial.println("✅ MPU6050 Connected!");
  } else {
    Serial.println("❌ MPU6050 Connection Failed!");
    while (1); // หยุดโปรแกรมถ้า MPU ไม่เชื่อมต่อ
  }
}

void loop() {
  int16_t ax, ay, az;
  mpu.getAcceleration(&ax, &ay, &az);

  // คำนวณค่าความเร่งรวม (absolute value)
  long totalVibration = abs(ax) + abs(ay) + abs(az);

  // แสดงค่าที่อ่านได้ใน Serial Monitor
  Serial.print("Vibration: ");
  Serial.println(totalVibration);

  // แปลงค่าความเร่งเป็นสัญญาณ PWM (0–255)
  int speed = map(totalVibration, 10000, 40000, 0, 255);
  speed = constrain(speed, 0, 255);

  // ✅ ควบคุมมอเตอร์ 2 ตัว
  analogWrite(motor1, speed);        // มอเตอร์ตัวที่ 1 ทำงานตามแรงสั่น
  analogWrite(motor2, 255 - speed);  // มอเตอร์ตัวที่ 2 สลับแรงกัน

  delay(500);
}

