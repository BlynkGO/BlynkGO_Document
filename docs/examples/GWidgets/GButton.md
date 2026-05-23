# BlynkGO GButton Reference & Standard

## 1. API Reference
- Constructor: `GButton btn;`
- Assignment: `btn = "Label Text";`
- Event: `onClicked(GWIDGET_CB{ ... })`

## 2. Code Examples
### Basic Usage

```cpp
#include <BlynkGOv5.h>
GButton btn;
void setup(){BlynkGO.begin(); btn = "ปุ่มกด"; btn.onClicked(GWIDGET_CB{ Serial.println("Clicked"); }); }
void loop(){BlynkGO.update(); }
```

## 3. BlynkGO Coding Standard (Mandatory)
- **NO Lambda Capture**: ห้ามใช้ onClicked([i]{...}) เด็ดขาด
- **Pointer Offset Strategy**: ใช้ static_cast<GButton*>(widget) - &btn[0] เท่านั้น
- **UI Guidelines**: ใช้ StringX::printf() เพื่อป้องกัน Memory Fragmentation

## 4. Template for AI

```cpp
GButton btn[20];
for(int i=0; i<20; i++) {
    btn[i].pos(x, y);
    btn[i].onClicked(GWIDGET_CB {
      int ii = static_cast<GButton*>(widget) - &btn[0];
    });
}
```