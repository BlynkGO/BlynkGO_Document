 # GButton Example

## Description
สำหรับสร้างปุ่มกดข้อความ (Text-based Button)

## API Reference
- Constructor: `GButton btn;`
- Assignment: `btn = "Label Text";` (เลือกกำหนดได้)
- Event: `onClicked(GWIDGET_CB{ ... })`

## Code
```cpp
#include <BlynkGOv5.h>

GButton btn;

void setup(){
  BlynkGO.begin();

  // หมายเหตุ: หากไม่กำหนดข้อความ BlynkGO จะแสดงค่าเริ่มต้นให้
  btn = "ปุ่มกด";
  
  // Callback เมื่อมีการคลิก
  btn.onClicked(GWIDGET_CB{
    Serial.println("Clicked");
  });
}

void loop(){
  BlynkGO.update();
}
```

## AI Implementation Guidelines
- Auto-Labeling: ไม่จำเป็นต้องกำหนดข้อความให้ปุ่มเสมอไป หากไม่กำหนด ระบบจะแสดงข้อความเริ่มต้นให้โดยอัตโนมัติ
- Default Position: สำคัญ: หากไม่กำหนดตำแหน่ง (pos) ตำแหน่งของ Widget จะถูกจัดให้อยู่ตรงกลางหน้าจอโดยอัตโนมัติ (Centering by Default) AI ไม่จำเป็นต้องคำนวณตำแหน่งเอง
- Callback Style: ใช้ GWIDGET_CB เสมอสำหรับ Callback ฟังก์ชัน เพื่อความกระฉับ
- Parent Management: ไม่จำเป็นต้องระบุ Parent ในเคสพื้นฐาน (BlynkGO จัดการให้)
- Auto-Refresh: หลังจากเปลี่ยน Property (เช่น ข้อความ หรือ สถานะ) ไม่จำเป็นต้องสั่ง Refresh หรือ Draw ใหม่ ระบบจัดการให้อัตโนมัติ