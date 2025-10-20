# code
#include <Wire.h>
#include <MPU6050.h>

MPU6050 mpu;

#define MOTOR_PIN 3
#define BUTTON_PIN 2

bool gloveActive = false;  // สถานะถุงมือ ON/OFF

void setup() {
  Serial.begin(9600);
  Wire.begin();

  // Initialize MPU6050
  mpu.initialize();
  if (!mpu.testConnection()) {
    Serial.println("MPU6050 connection failed!");
    while (1); // หยุดโปรแกรมถ้าเชื่อมต่อไม่สำเร็จ
  }

  pinMode(MOTOR_PIN, OUTPUT);
  pinMode(BUTTON_PIN, INPUT_PULLUP);  // ปุ่มต่อกับ GND

  Serial.println("GyroGlove ready!");
}

void loop() {
  // ตรวจสอบปุ่มเปิด–ปิด
  if (digitalRead(BUTTON_PIN) == LOW) { // กดปุ่ม
    delay(50); // debounce
    while(digitalRead(BUTTON_PIN) == LOW); // รอปล่อย
    gloveActive = !gloveActive; // toggle สถานะ
    Serial.print("Glove Active: "); Serial.println(gloveActive);
  }

  if (gloveActive) {
    // อ่านค่ามุม/การเคลื่อนไหวจาก MPU6050
    int16_t ax, ay, az;
    int16_t gx, gy, gz;
    mpu.getMotion6(&ax, &ay, &az, &gx, &gy, &gz);

    // ตัวอย่างเงื่อนไขสั่น: ถ้ามีการเคลื่อนไหวเกิน threshold
    int threshold = 10000; // ปรับตามต้องการ
    if (abs(ax) > threshold || abs(ay) > threshold || abs(az) > threshold) {
      digitalWrite(MOTOR_PIN, HIGH); // มอเตอร์สั่น
    } else {
      digitalWrite(MOTOR_PIN, LOW);  // มอเตอร์หยุด
    }

    // Debug
    Serial.print("ax: "); Serial.print(ax);
    Serial.print(" ay: "); Serial.print(ay);
    Serial.print(" az: "); Serial.println(az);

    delay(50); // loop 20Hz
  } else {
    digitalWrite(MOTOR_PIN, LOW); // ปิดมอเตอร์ถ้าปุ่ม OFF
  }
}

------------------------------------------------------------------

#include <Wire.h>
#include <MPU6050.h>

MPU6050 mpu;

const int motorA = 3;     // Motor A control pin
const int motorB = 5;     // Motor B control pin
const int buttonPin = 2;  // Push button pin

bool systemOn = false;

void setup() {
  Serial.begin(9600);
  Wire.begin();

  // Initialize MPU6050
  mpu.initialize();
  if(!mpu.testConnection()){
    Serial.println("MPU6050 connection failed!");
  } else {
    Serial.println("MPU6050 ready");
  }

  // Motor pins
  pinMode(motorA, OUTPUT);
  pinMode(motorB, OUTPUT);

  // Button pin
  pinMode(buttonPin, INPUT_PULLUP); // Using internal pull-up

  // Ensure motors are off initially
  digitalWrite(motorA, LOW);
  digitalWrite(motorB, LOW);
}

void loop() {
  // Check button press (toggle system on/off)
  static bool lastButtonState = HIGH;
  bool buttonState = digitalRead(buttonPin);
  if (lastButtonState == HIGH && buttonState == LOW) {
    systemOn = !systemOn;
    delay(50); // debounce
  }
  lastButtonState = buttonState;

  if(systemOn){
    int16_t ax, ay, az;
    int16_t gx, gy, gz;

    // Read accelerometer and gyro values
    mpu.getMotion6(&ax, &ay, &az, &gx, &gy, &gz);

    // For demo: simple threshold on acceleration to trigger motors
    if(abs(ax) > 2000 || abs(ay) > 2000 || abs(az) > 2000){
      digitalWrite(motorA, HIGH);
      digitalWrite(motorB, HIGH);
    } else {
      digitalWrite(motorA, LOW);
      digitalWrite(motorB, LOW);
    }

    // Optional: print values to Serial Monitor
    Serial.print("A: "); Serial.print(ax); Serial.print(","); Serial.print(ay); Serial.print(","); Serial.println(az);
    Serial.print("G: "); Serial.print(gx); Serial.print(","); Serial.print(gy); Serial.print(","); Serial.println(gz);
  } else {
    digitalWrite(motorA, LOW);
    digitalWrite(motorB, LOW);
  }

  delay(20); // small delay for stability
}

