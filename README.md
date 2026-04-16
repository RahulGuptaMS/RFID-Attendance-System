# 📡 RFID Attendance System using Arduino

## 📌 Project Description

This project is an RFID-based attendance system that automatically marks attendance when a user scans their RFID card. It uses an Arduino, RFID module (RC522), LCD display, LEDs, and a buzzer to provide real-time feedback.

---

## 🎯 Objective

The main objective of this project is to:

* Automate attendance using RFID cards
* Reduce manual effort and errors
* Provide quick and secure identification

---

## 🧰 Components Required

* Arduino UNO
* RFID Module (RC522)
* RFID Cards/Tags
* 16x2 LCD (I2C)
* Green LED
* Red LED
* Buzzer
* Breadboard
* Jumper Wires

---

## 🔌 Circuit Connections

### 📡 RFID RC522 → Arduino

| RC522 Pin | Arduino Pin |
| --------- | ----------- |
| SDA       | D10         |
| SCK       | D13         |
| MOSI      | D11         |
| MISO      | D12         |
| RST       | D9          |
| GND       | GND         |
| 3.3V      | 3.3V ⚠️     |

---

### 📺 LCD (I2C) → Arduino

| LCD Pin | Arduino Pin |
| ------- | ----------- |
| VCC     | 5V          |
| GND     | GND         |
| SDA     | A4          |
| SCL     | A5          |

---

### 💡 LEDs & Buzzer

* Green LED → D6
* Red LED → D7
* Buzzer → D5

---

## ⚙️ Working Principle

1. System starts and LCD displays **"Scan Card..."**
2. User scans RFID card
3. RFID module reads unique UID
4. Arduino compares UID with stored data
5. If UID matches:

   * Attendance marked
   * Name displayed on LCD
   * Green LED turns ON
6. If UID does not match:

   * "Access Denied" displayed
   * Red LED and buzzer activated

---

## 🧾 Arduino Code

```cpp
#include <SPI.h>
#include <MFRC522.h>
#include <Wire.h>
#include <LiquidCrystal_I2C.h>

#define SS_PIN 10
#define RST_PIN 9

MFRC522 rfid(SS_PIN, RST_PIN);
LiquidCrystal_I2C lcd(0x27, 16, 2);

#define GREEN_LED 6
#define RED_LED 7
#define BUZZER 5

String uid1 = "DBCA106";
String uid2 = "13BD354";

void setup() {
  Serial.begin(9600);
  SPI.begin();
  rfid.PCD_Init();

  pinMode(GREEN_LED, OUTPUT);
  pinMode(RED_LED, OUTPUT);
  pinMode(BUZZER, OUTPUT);

  lcd.init();
  lcd.backlight();
  lcd.print("Scan Card...");
}

void loop() {

  if (!rfid.PICC_IsNewCardPresent()) return;
  if (!rfid.PICC_ReadCardSerial()) return;

  String uid = "";

  for (byte i = 0; i < rfid.uid.size; i++) {
    uid += String(rfid.uid.uidByte[i], HEX);
  }

  uid.toUpperCase();
  lcd.clear();

  if (uid == uid1) {
    lcd.print("Rahul & Uttam");
    digitalWrite(GREEN_LED, HIGH);
    delay(2000);
    digitalWrite(GREEN_LED, LOW);
  }

  else if (uid == uid2) {
    lcd.print("Deepanshu Kom");
    digitalWrite(GREEN_LED, HIGH);
    delay(2000);
    digitalWrite(GREEN_LED, LOW);
  }

  else {
    lcd.print("Access Denied");
    digitalWrite(RED_LED, HIGH);
    digitalWrite(BUZZER, HIGH);
    delay(2000);
    digitalWrite(RED_LED, LOW);
    digitalWrite(BUZZER, LOW);
  }

  lcd.clear();
  lcd.print("Scan Card...");
}
```

---

## ✅ Features

* Automatic attendance system
* Real-time LCD display
* Audio-visual feedback (LED + Buzzer)
* Fast and secure identification

---

## 📊 Advantages

* Saves time
* Reduces manual errors
* Easy to use
* Low cost

---

## ⚠️ Limitations

* RFID card can be lost
* Limited storage
* No online database

---

## 🚀 Future Scope

* Add WiFi (ESP8266/ESP32)
* Store data in Google Sheets
* Add fingerprint or face recognition
* Mobile app integration

---

## 👨‍💻 Author

Your Name

---
