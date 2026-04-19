# 🔋 Real-Time Battery Monitoring and Protection System for Electric Vehicle

## 📌 Project Overview
The **Real-Time Battery Monitoring and Protection System for Electric Vehicle** is an Arduino-based prototype Battery Management System (BMS) designed to improve the **safety, efficiency, and reliability** of lithium-ion battery packs used in electric vehicles.

The system continuously monitors important battery parameters such as:

- Battery Voltage
- Charging/Discharging Current
- Temperature
- Humidity

It also provides **automatic protection** by disconnecting the battery under unsafe conditions such as:
- Over-discharge
- Over-current
- Over-temperature

---

## 🎯 Objectives
- Monitor battery performance in real time  
- Protect battery from unsafe operating conditions  
- Display live battery data on LCD  
- Improve battery lifespan  
- Enhance EV battery safety  

---

## ⚙️ Features
✅ Real-time voltage monitoring  
✅ Current sensing  
✅ Temperature and humidity monitoring  
✅ Automatic battery cut-off using relay  
✅ LCD display for live data  
✅ Compact prototype design  
✅ Expandable for IoT integration  

---

## 🛠️ Components Used

| Component | Description |
|----------|-------------|
| Arduino UNO | Main microcontroller |
| ACS712 | Current sensor |
| Voltage Sensor Module | Battery voltage measurement |
| DHT11 | Temperature and humidity sensor |
| 16x2 LCD with I2C | Display module |
| Relay Module | Battery protection switching |
| 2N2222 Transistor | Relay driver |
| 1N4007 Diode | Flyback protection |
| LM2596 | Buck converter |
| Li-ion Battery Pack | Power source |

---

## 🔌 Working Principle

### Step 1:
The battery supplies power to the system.

### Step 2:
LM2596 converts battery voltage to **5V** for Arduino and sensors.

### Step 3:
Sensors continuously measure:
- Voltage
- Current
- Temperature
- Humidity

### Step 4:
Arduino processes sensor values.

### Step 5:
Values are displayed on the LCD screen.

### Step 6:
If voltage drops below the safe limit:
- Relay disconnects the battery
- Battery is protected from damage

---

## 🧠 Protection Logic

The system protects against:

- 🔻 Under-voltage
- 🔥 Over-temperature
- ⚡ Over-current
- 🔋 Battery over-discharge

---

## 📷 Circuit Diagram
Add your circuit image here:

<img width="710" height="446" alt="image" src="https://github.com/user-attachments/assets/8c2fd44a-9f34-40cd-a1d5-8cfe412e8057" />

## 📷 Hardware Prototype
<img width="494" height="371" alt="image" src="https://github.com/user-attachments/assets/3fa5ac61-49d8-490c-817c-fae6b783aa40" />


## 💻 Arduino Code
#include <DHT.h>

// --- DHT11 setup ---
#define DHTPIN 2          // DHT11 data pin connected to D2
#define DHTTYPE DHT11
DHT dht(DHTPIN, DHTTYPE);

// --- Sensor pins ---
#define VOLT_PIN A1       // Voltage sensor output
#define CURR_PIN A0       // ACS712 output
#define RELAY_PIN 8       // Relay driver transistor base (through resistor)

// --- Constants for calibration ---
const float VREF = 5.0;       // Arduino reference voltage
const float R1 = 30000.0;     // Voltage divider top resistor (ohms)
const float R2 = 10000.0;     // Voltage divider bottom resistor (ohms)
const float V_SENS_RATIO = (R1 + R2) / R2;   // Scaling factor

// --- For ACS712 calibration ---
const float ACS_OFFSET = 2.5;     // 0A ≈ 2.5V output
const float SENSITIVITY = 0.066;  // For 30A module (adjust if needed)

void setup() {
  Serial.begin(9600);
  dht.begin();

  pinMode(RELAY_PIN, OUTPUT);
  digitalWrite(RELAY_PIN, LOW);  // Relay initially OFF

  delay(1000);

  Serial.println(" IoT BMS Monitoring Started (No LCD Mode)");
  Serial.println("------------------------------------------");
}

void loop() {

  // --- Measure Voltage ---
  int rawV = analogRead(VOLT_PIN);
  float vOut = (rawV * VREF) / 1023.0;
  float batteryVoltage = vOut * V_SENS_RATIO;

  // --- Measure Current ---
  int rawI = analogRead(CURR_PIN);
  float vI = (rawI * VREF) / 1023.0;
  float current = (vI - ACS_OFFSET) / SENSITIVITY;

  // --- Measure Temperature & Humidity ---
  float humidity = dht.readHumidity();
  float temperature = dht.readTemperature();

  // --- Protection condition ---
  bool fault = false;

  if (batteryVoltage > 12.6 || batteryVoltage < 9.0)
    fault = true;

  if (temperature > 50)
    fault = true;

  // --- Control relay ---
  if (fault) {
    digitalWrite(RELAY_PIN, LOW);   // Turn OFF (protection)
  } else {
    digitalWrite(RELAY_PIN, HIGH);  // Turn ON
  }

  // --- Display in Serial Monitor ---
  Serial.print("Voltage: ");
  Serial.print(batteryVoltage);
  Serial.println(" V");

  Serial.print("Current: ");
  Serial.print(current);
  Serial.println(" A");

  Serial.print("Temp: ");
  Serial.print(temperature);
  Serial.println(" °C");

  Serial.print("Humidity: ");
  Serial.print(humidity);
  Serial.println(" %");

  Serial.print("Relay: ");
  Serial.println(fault ? "OFF (Protection Active)" : "ON");

  Serial.println("------------------------------------------");

  delay(2000);
}

```bash
code/battery_monitor.ino
```

---

## 🚀 Future Improvements
- IoT cloud monitoring
- Mobile app integration
- Battery health prediction
- State of Charge (SOC) estimation
- State of Health (SOH) calculation
- Machine learning fault detection

---

## 📚 Applications
This system can be used in:

- Electric Vehicles
- Solar battery storage
- UPS systems
- Portable electronics
- Battery research labs
- Renewable energy systems

---

## 🏆 Advantages
✔ Improves battery safety  
✔ Extends battery life  
✔ Low-cost implementation  
✔ Real-time monitoring  
✔ Easy to expand  

---

## 👨‍💻 Author
**Reetesh**

Electrical Engineering Student  
Interested in:
- Electric Vehicles
- Embedded Systems
- Battery Management Systems
- Arduino Projects

---
This project is for educational and academic purposes.
