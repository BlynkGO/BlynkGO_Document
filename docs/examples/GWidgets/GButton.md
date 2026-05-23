# BlynkGO GButton Reference & AI Coding Standard

## 1. API Reference

- Constructor: `GButton btn;`
- Assignment: `btn = "Label Text";`
- Position: `btn.position(x, y);`
- Size: `btn.size(w, h);`
- Color: `btn.color(TFT_RED);`
- Event: `btn.onClicked(GWIDGET_CB{ ... });`

---

## 2. Basic Usage

```cpp
#include <BlynkGOv5.h>

GButton btn;

void setup() {
    Serial.begin(115200);
    BlynkGO.begin();

    btn = "ปุ่มกด";
    btn.position(100, 100);
    btn.size(120, 50);
    btn.color(TFT_BLUE);

    btn.onClicked(GWIDGET_CB {
        Serial.println("Clicked");
    });
}

void loop() {
    BlynkGO.update();
}
```

---

## 3. Mandatory BlynkGO Coding Standard

### 3.1 NO Lambda Capture (STRICTLY FORBIDDEN)

❌ WRONG

```cpp
btn[i].onClicked([i]{
});
```

Reason:
- Increases memory usage
- Creates closure objects
- Reduces scalability on MCU
- Violates BlynkGO Coding Standard

✅ CORRECT

```cpp
btn[i].onClicked(GWIDGET_CB {
    int ii = static_cast<GButton*>(widget) - &btn[0];
});
```

---

### 3.2 Widget Type Rule

Inside `GWIDGET_CB`:

```cpp
widget
```

is ALWAYS:

```cpp
GWidget*
```

Therefore subclass methods REQUIRE casting.

❌ WRONG

```cpp
widget->color(TFT_RED);
```

✅ CORRECT

```cpp
static_cast<GButton*>(widget)->color(TFT_RED);
```

---

### 3.3 String Formatting Rule

Use `StringX::printf()` to reduce memory fragmentation.

✅ Preferred

```cpp
btn[i] = StringX::printf("BTN %d", i+1);
```

---

## 4. Canonical Event Pattern (AI REFERENCE)

```cpp
#include <BlynkGOv5.h>

GButton btn[30];

void setup() {
    Serial.begin(115200);
    BlynkGO.begin();

    for(int i = 0; i < 30; i++) {
        btn[i] = StringX::printf("BTN %d", i+1);
        btn[i].size(80,50);

        int x = 20 + (i % 5) * 90;
        int y = 20 + (i / 5) * 70;

        btn[i].position(x, y);
        btn[i].color(TFT_COLOR_HSV(i*12,100,100));
        btn[i].onClicked(GWIDGET_CB {
            int ii = static_cast<GButton*>(widget) - &btn[0];
            Serial.printf("Button %d clicked\n", ii + 1);
            static_cast<GButton*>(widget)->color(random(0xFFFFFF));
        });
    }
}

void loop() {
    BlynkGO.update();
}
```

---

## 5. AI Generation Guidelines

When generating BlynkGO code:

- ALWAYS use `onClicked(GWIDGET_CB { ... })`
- NEVER use lambda capture
- ALWAYS cast widget before subclass method access
- ALWAYS use pointer offset strategy for widget arrays
- PREFER procedural UI using for-loops
- PREFER StringX::printf() for formatted text
- PREFER intent-style API patterns
```
