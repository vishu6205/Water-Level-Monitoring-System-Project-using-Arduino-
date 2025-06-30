# 💧 Water Level Monitoring System using Arduino (Single Water Sensor with Buzzer)

## 📌 Project Overview

This project creates a simple **water level detector** using an Arduino Uno, a single water level sensor, and a buzzer. The system monitors the water level in a tank and provides an **audible alert** when water is detected. An **optional LED** can serve as a visual alert. This cost-effective solution is ideal for **basic overflow** or **low-water warnings**.

---

## 🧰 Required Components

| Component                  | Quantity | Description                                                              |
|---------------------------|----------|--------------------------------------------------------------------------|
| Arduino Uno               | 1        | Microcontroller that reads the sensor and controls output                |
| Water Level Sensor (S, +, -) | 1     | Detects water presence; sends HIGH when submerged                        |
| Buzzer                    | 1        | Sounds an alarm when water is detected                                   |
| LED (Optional)            | 1        | Visual indicator when water is detected                                  |
| 220-ohm Resistor (Optional)| 1       | Limits current to protect the LED                                        |
| Breadboard                | 1        | Temporary base for building the circuit without soldering                |
| Jumper Wires              | As needed| Connect components on the breadboard and Arduino                         |
| USB Cable / Power Supply  | 1        | Uploads code and powers the Arduino board                                |

---

## ⚙️ Working Principle

The **water level sensor** detects the presence of water at a specified height. When water bridges the sensor’s terminals, it sends a **HIGH** signal to the Arduino. The Arduino then:

- Triggers a **buzzer** to sound for 5 seconds
- Optionally turns on an **LED** for visual indication

This allows you to monitor if water has reached a critical level.

---

## 🔌 Circuit Connections

### 🔹 Water Level Sensor → Arduino

- `S` (Signal) → `A0`  
- `+` (VCC) → `5V`  
- `–` (GND) → `GND`  

### 🔹 Buzzer → Arduino

- Positive → `Pin 8`  
- Negative → `GND`  

### 🔹 (Optional) LED → Arduino

- Anode (+) → `Pin 8` (via 220-ohm resistor)  
- Cathode (–) → `GND`  

---

## 👨‍💻 Arduino Code (No Display)

```cpp
int level;
const int analog_0 = A0;
int l1 = 13;
int l2 = 12;
int l3 = 11;
int l4 = 10;
int buzzer = 8;

unsigned long buzzerStartTime = 0;
bool buzzerActive = false;

void setup() {
  Serial.begin(9600);
  pinMode(l1, OUTPUT);
  pinMode(l2, OUTPUT);
  pinMode(l3, OUTPUT);
  pinMode(l4, OUTPUT);
  pinMode(buzzer, OUTPUT);
}

void loop() {
  level = analogRead(analog_0);
  Serial.println(level);

  // Turn off all LEDs
  digitalWrite(l1, LOW);
  digitalWrite(l2, LOW);
  digitalWrite(l3, LOW);
  digitalWrite(l4, LOW);

  if (level > 500 && level < 550) {
    digitalWrite(l1, HIGH);
  } 
  else if (level > 600 && level < 620) {
    digitalWrite(l2, HIGH);
  } 
  else if (level > 620 && level < 630) {
    digitalWrite(l3, HIGH);
  } 
  else if (level > 630 && level < 650) {
    digitalWrite(l4, HIGH);

    // Activate buzzer for 5 seconds
    if (!buzzerActive) {
      digitalWrite(buzzer, HIGH);
      buzzerStartTime = millis();
      buzzerActive = true;
    }
  }

  // Turn off buzzer after 5 seconds
  if (buzzerActive && (millis() - buzzerStartTime >= 5000)) {
    digitalWrite(buzzer, LOW);
    buzzerActive = false;
  }

  delay(200);
}
