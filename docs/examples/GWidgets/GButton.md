 # GButton Example

## Description
สำหรับสร้างปุ่มกดข้อความ (Text-based Button)

## API Reference
- Constructor: `GButton btn;`
- Assignment: `btn = "Label Text";` (เลือกกำหนดได้)
- Event: `onClicked(GWIDGET_CB{ ... })`

# GButton Reference

## Basic Usage
```cpp
#include <BlynkGOv5.h>

GButton btn;

void setup(){
  BlynkGO.begin();
  btn = "ปุ่มกด";
  btn.onClicked(GWIDGET_CB{
    Serial.println("Clicked");
  });
}

void loop(){
  BlynkGO.update();
}
```

## Array Usage (Pointer Offset)
```cpp
#include <BlynkGOv5.h>

GButton btn[30];

void setup(){
  BlynkGO.begin();
  for(int i = 0; i < 30; i++){
    btn[i] = String("ปุ่ม " + String(i+1)).c_str();
    btn[i].onClicked(GWIDGET_CB{
      int ii = static_cast<GButton*>(widget) - &btn[0];
      Serial.println("Clicked: " + String(ii+1));
    });
  }
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