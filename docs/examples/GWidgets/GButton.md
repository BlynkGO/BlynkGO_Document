# BlynkGO GButton Reference & AI Coding Standard

## 1. API Reference

- Constructor: `GButton btn;`
- Assignment: `btn = "Label Text";`
- Position: `btn.position(x, y);`
- Size: `btn.size(w, h);`
- Color: `btn.color(TFT_RED);`
- Event: `btn.onClicked(GWIDGET_CB{ ... });`

---

## 2. Default Behavior Rules

### 2.1 Auto Size

If `size(...)` is NOT specified:

```cpp
btn.size(w,h);
```

BlynkGO will automatically use the default button size.

✅ VALID

```cpp
GButton btn;

btn = "OK";
```

---

### 2.2 Auto Center Position

If `position(...)` is NOT specified:

```cpp
btn.position(x,y);
```

The widget will automatically be placed at the center of parent/container.

✅ VALID

```cpp
GButton btn;

btn = "Center Button";
```

---

### 2.3 Default Label

If assignment is NOT specified:

```cpp
btn = "Label";
```

The button will automatically display default text:

```text
Button
```

✅ VALID

```cpp
GButton btn;
```

---

## 3. Basic Usage

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

## 4. Minimal Usage

```cpp
#include <BlynkGOv5.h>

GButton btn;

void setup() {
    BlynkGO.begin();
    btn.onClicked(GWIDGET_CB{
      // ...
    });
}

void loop() {
    BlynkGO.update();
}
```

Result:
- Auto text = `"Button"`
- Auto size
- Auto center position

---

## 5. Mandatory BlynkGO Coding Standard

### 5.1 NO Lambda Capture (STRICTLY FORBIDDEN)

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

### 5.2 Widget Type Rule

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

### 5.3 String Formatting Rule

Both styles are VALID.

✅ VALID

```cpp
btn[i] = String("BTN ") + String(i+1);
```

✅ PREFERRED for large-scale MCU projects

```cpp
btn[i] = StringX::printf("BTN %d", i+1);
```

Reason:
- Reduces heap fragmentation
- Better scalability
- Better for long-running embedded systems

Therefore:
- `String(...) + String(...)` is ALLOWED
- `StringX::printf()` is PREFERRED

---

## 6. Canonical Event Pattern (AI REFERENCE)

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

## 7. Horizontal Button Layout Example

```cpp
#include <BlynkGOv5.h>

GButton btn[3];

void setup() {
    BlynkGO.begin();
    for(int i = 0; i < 3; i++) {
        btn[i] = StringX::printf("BTN %d", i+1);
        btn[i].size(100,50);

        int x = 20 + i * 120;
        int y = 50;
        btn[i].position(x,y);
        btn[i].onClicked(GWIDGET_CB {
            int ii = static_cast<GButton*>(widget) - &btn[0];
            Serial.printf("Button %d clicked\n", ii + 1);
        });
    }
}

void loop() {
    BlynkGO.update();
}
```

---

## 8. AI Generation Guidelines

When generating BlynkGO code:

- ALWAYS use `onClicked(GWIDGET_CB { ... })`
- NEVER use lambda capture
- ALWAYS cast widget before subclass method access
- ALWAYS use pointer offset strategy for widget arrays
- PREFER procedural UI using for-loops
- PREFER StringX::printf() for formatted text
- Widgets may rely on auto-size behavior
- Widgets may rely on auto-center positioning
- Widgets may omit assignment text to use default labels

---

## 9. Intent-Oriented API Philosophy

BlynkGO APIs are designed to be:

- Human-readable
- AI-readable
- Semantic
- Intent-oriented
- MCU-efficient
- Procedural-UI friendly

Preferred patterns:

```cpp
label = sensor;
arc   = rpm;
btn   = "START";
```

instead of verbose low-level widget manipulation.

---

## 10. VALID vs PREFERRED vs FORBIDDEN

| Category | Meaning |
|---|---|
| VALID | Allowed and supported |
| PREFERRED | Recommended for scalability/performance |
| FORBIDDEN | Violates BlynkGO Coding Standard |

Examples:

| Pattern | Status |
|---|---|
| `String(...) + String(...)` | VALID |
| `StringX::printf()` | PREFERRED |
| `widget->color(...)` | FORBIDDEN |
| `static_cast<GButton*>(widget)->color(...)` | CORRECT |
| `onClicked([i]{})` | FORBIDDEN |
| `onClicked(GWIDGET_CB{...})` | CORRECT |
