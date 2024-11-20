# MenuManager Library for Arduino (พร้อมจอ LCD แบบ I2C)

ไลบรารีนี้ช่วยให้คุณสามารถจัดการเมนูบนจอ LCD แบบ I2C ได้อย่างง่ายดาย โดยใช้ปุ่มควบคุมสำหรับการนำทางและการเลือก ออกแบบมาให้ใช้งานร่วมกับไลบรารี `LiquidCrystal_I2C` สำหรับจอ LCD ที่ควบคุมผ่าน I2C โดยเฉพาะ

---

## คุณสมบัติ

- รองรับการนำทางผ่านรายการเมนู
- API ที่เข้าใจง่ายสำหรับการเพิ่มเมนู
- ใช้งานกับจอ LCD แบบ I2C ได้ (เช่น 16x2, 20x4)
- ใช้ปุ่มควบคุมสำหรับการนำทางและการเลือกเมนู
- สามารถปรับแต่งรายการเมนูและการควบคุมได้

---

## การติดตั้ง  

### 1. ติดตั้งไลบรารี `LiquidCrystal_I2C`
1. ไปที่เมนู **Sketch** → **Include Library** → **Manage Libraries**
2. ค้นหา `LiquidCrystal_I2C` และติดตั้ง

### 2. เพิ่มไลบรารี `MenuManager` ลงใน Arduino
1. ดาวน์โหลดไฟล์ `MenuManager.h` และ `MenuManager.cpp`
2. วางไฟล์ในโฟลเดอร์ `libraries` ของ Arduino IDE

---

## การทำงานของไลบรารี

ไลบรารี `MenuManager` ใช้สำหรับสร้างและแสดงผลเมนูบนจอ LCD แบบ I2C คุณสามารถเพิ่มเมนูใหม่ นำทางผ่านเมนูด้วยปุ่ม และเลือกเพื่อแสดงข้อความยืนยันได้  

### ฟังก์ชันหลัก:
- `add(String menuName)`  
  เพิ่มรายการเมนูใหม่
- `displayMenu()`  
  แสดงผลเมนูปัจจุบันบนจอ LCD
- `handleInput(int btnNext, int btnSelect)`  
  จัดการการกดปุ่มสำหรับนำทางและการเลือก
- `getSelectedMenu()`  
  รับชื่อของเมนูที่ถูกเลือก

---

## ตัวอย่างการใช้งาน

ตัวอย่างนี้แสดงวิธีการใช้งานไลบรารี `MenuManager` เพื่อสร้างระบบเมนูบนจอ LCD แบบ I2C ขนาด 16x2:

```cpp
#include <Arduino.h>
#include <Wire.h>
#include <LiquidCrystal_I2C.h>
#include "MenuManager.h"

// เริ่มต้นจอ LCD ด้วยที่อยู่ I2C (0x27) และขนาด 16x2
LiquidCrystal_I2C lcd(0x27, 16, 2);

// สร้างออบเจ็กต์ MenuManager
MenuManager menu(&lcd);

// กำหนดพินของปุ่ม
#define BTN_NEXT 7    // ปุ่มสำหรับเปลี่ยนไปเมนูถัดไป
#define BTN_SELECT 8  // ปุ่มสำหรับเลือกเมนู

void setup() {
    // เริ่มต้นจอ LCD
    lcd.begin();
    lcd.backlight();

    // ตั้งค่าพินของปุ่มเป็น INPUT_PULLUP
    pinMode(BTN_NEXT, INPUT_PULLUP);
    pinMode(BTN_SELECT, INPUT_PULLUP);

    // เพิ่มรายการเมนู
    menu.add("เมนู 1: ตัวเลือก A");
    menu.add("เมนู 2: ตัวเลือก B");
    menu.add("เมนู 3: ตัวเลือก C");
    menu.add("เมนู 4: ตัวเลือก D");
    menu.add("เมนู 5: ตัวเลือก E");

    // แสดงเมนูแรก
    menu.displayMenu();
}

void loop() {
    // จัดการการกดปุ่มสำหรับการนำทางและการเลือกเมนู
    menu.handleInput(BTN_NEXT, BTN_SELECT);

    // ตัวเลือกเสริม: รับเมนูที่ถูกเลือกและแสดงผลบน Serial Monitor
    if (digitalRead(BTN_SELECT) == LOW) {
        String selectedMenu = menu.getSelectedMenu();
        Serial.println("คุณเลือก: " + selectedMenu);
    }
}
