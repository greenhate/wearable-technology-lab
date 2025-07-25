# 🟢 Hardware Development — قسم الهاردوير

> "الأجهزة الذكية هي بوابة العبور بين العالم الرقمي والواقع الإنساني."

مرحبًا بك في قسم تطوير الهاردوير الخاص بمشروعنا.  
هنا نركز على بناء سوار ذكي متكامل يجمع بين الدقة الحيوية، الاتصالات اللاسلكية، والاستدامة في الأداء.

---

## ⚙️ المكونات الأساسية للمشروع

- **وحدات المعالجة الدقيقة**:
  - ESP32: لدعم البلوتوث والواي فاي مع أداء منخفض استهلاك الطاقة.
- **مستشعرات حيوية**:
  - مستشعر نبض القلب: MAX30100 / MAX30102.
  - مستشعر درجة حرارة الجلد: DS18B20.
  - مستشعر حركة متعدد المحاور (IMU): مثل MPU6050.
- **أنظمة الاتصال**:
  - Bluetooth Low Energy (BLE) للنطاق الشخصي.
  - Wi-Fi للاتصال السحابي أو المحلي.
- **مصادر الطاقة**:
  - بطاريات Li-Po صغيرة الحجم مع حماية دوائر الشحن.
- **دارات إلكترونية داعمة**:
  - منظمات جهد، وحدات إدارة طاقة، شواحن مدمجة.

---

## 🔍 اعتبارات التصميم

| الجانب | الاعتبارات |
|:---|:---|
| استهلاك الطاقة | استخدام وضعيات النوم العميق (Deep Sleep) وتقنيات توفير الطاقة. |
| الراحة عند الارتداء | تصميم خفيف، مرن، وآمن للبشرة. |
| المتانة | مقاومة للتعرق والرطوبة، قدرة على التحمل أثناء الاستخدام اليومي. |
| الاتصال | ثبات الاتصال عبر BLE وعدم انقطاع الإرسال. |

---

## 🧩 مثال عملي — توصيل مستشعر نبض القلب مع ESP32

### التوصيل الكهربائي:

| الطرف | متصل مع ESP32 |
|:---|:---|
| VCC | 3.3V |
| GND | GND |
| SDA | GPIO21 |
| SCL | GPIO22 |

### كود تجريبي بسيط:

```cpp
#include <Wire.h>
#include "MAX30100_PulseOximeter.h"

MAX30100_PulseOximeter pox;

void onBeatDetected() {
  Serial.println("💓 نبضة قلب مكتشفة!");
}

void setup() {
  Serial.begin(115200);
  Wire.begin();
  
  if (!pox.begin()) {
    Serial.println("⚠️ فشل بدء تشغيل مستشعر النبض");
    while (1);
  }
  
  pox.setOnBeatDetectedCallback(onBeatDetected);
}

void loop() {
  pox.update();
}
