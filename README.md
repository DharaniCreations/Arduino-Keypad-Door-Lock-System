# 🔐 Arduino Keypad Door Lock System

A simple and effective security system using Arduino Uno, a 4x4 matrix keypad, and a relay-controlled electric lock. This project allows you to unlock a door using a secure PIN code entered through the keypad.

## 📌 Project Overview

This project is a password-protected electronic door lock system built using an Arduino microcontroller. It reads input from a keypad and unlocks an electric lock if the correct PIN is entered. The system includes a relay module to control the lock, and can be enhanced with an LCD display and a buzzer for feedback.

---

## 🧰 Components Used

| Component              | Quantity |
|------------------------|----------|
| Arduino Uno            | 1        |
| 4x4 Matrix Keypad      | 1        |
| Relay Module (5V)      | 1        |
| Electric Lock (12V)    | 1        |
| Power Supply (12V)     | 1        |
| Jumper Wires           | Several  |
| Breadboard             | 1        |
| (Optional) LCD Display | 1        |
| (Optional) Buzzer      | 1        |

---

## 🔌 Circuit Connections

**Keypad → Arduino:**
- R1 → Pin 9  
- R2 → Pin 8  
- R3 → Pin 7  
- R4 → Pin 6  
- C1 → Pin 5  
- C2 → Pin 4  
- C3 → Pin 3  
- C4 → Pin 2  

**Relay Module → Arduino:**
- VCC → 5V  
- GND → GND  
- IN  → Pin 10

**Electric Lock:**
- Connected via relay's Normally Open (NO) terminal  
- Powered using 12V power supply (relay acts as switch)

---

## ⚙️ How It Works

1. User enters a PIN code using the keypad.
2. Arduino compares the input with a predefined password.
3. If the PIN is correct:
   - Relay activates for a few seconds.
   - Lock opens temporarily, then re-locks.
4. If the PIN is incorrect:
   - Access is denied.
   - (Optional) Buzzer or message alerts the user.

---

## 🧠 Features

- Secure PIN-based access
- Customizable unlock time
- Optional LCD and buzzer feedback
- Easy to modify or expand
- Great for home or school security applications

---

## 🧾 Example Arduino Code (Simple Version)

```cpp
#include <Keypad.h>

const byte ROWS = 4;
const byte COLS = 4;
char keys[ROWS][COLS] = {
  {'1','2','3','A'},
  {'4','5','6','B'},
  {'7','8','9','C'},
  {'*','0','#','D'}
};
byte rowPins[ROWS] = {9, 8, 7, 6};
byte colPins[COLS] = {5, 4, 3, 2};

Keypad keypad = Keypad(makeKeymap(keys), rowPins, colPins, ROWS, COLS);

String password = "1234";
String input = "";

int relayPin = 10;

void setup() {
  pinMode(relayPin, OUTPUT);
  digitalWrite(relayPin, LOW);
  Serial.begin(9600);
}

void loop() {
  char key = keypad.getKey();
  if (key) {
    if (key == '#') {
      if (input == password) {
        Serial.println("Access Granted");
        digitalWrite(relayPin, HIGH);
        delay(3000);
        digitalWrite(relayPin, LOW);
      } else {
        Serial.println("Access Denied");
      }
      input = "";
    } else {
      input += key;
    }
  }
}
