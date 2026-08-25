# Smart Irrigation System Using Raspberry Pi

An automated, sensor-driven embedded solution designed to optimize water usage in agricultural and gardening applications. Built as part of the **ECE2011** course project at **Presidency University**, this system monitors soil volumetric water content in real-time and automatically activates a water pump when moisture levels drop below designated thresholds[cite: 5].

---

## 📌 Project Information

* **Institution:** Presidency University | School of Engineering[cite: 5]
* **Course Code:** ECE2011[cite: 5]
* **Group ID:** IPR 129[cite: 5]
* **Faculty Supervisor:** Ms. Yogeetha (Assistant Professor, School of Engineering)[cite: 5]
* **Project Team:**
  * K. Bharathi[cite: 5]
  * Manisha R[cite: 5]
  * Avala Kavya Sree[cite: 5]
  * Madhumitha E[cite: 5]
  * Lavanya K[cite: 5]
  * Tejaswini CN[cite: 5]

---

## 🎯 Key Objectives

* **Automated Water Management:** Replaces manual watering schedules with automated, real-time sensor control[cite: 5].
* **Resource Conservation:** Prevents over-watering and reduces water waste through precision moisture monitoring[cite: 5].
* **Scalable IoT Design:** Provides a modular foundation capable of logging data locally or expanding to cloud platforms and mobile alerts[cite: 5].

---

## 🛠️ Hardware Specifications

| Component | Specifications | Functional Role |
| :--- | :--- | :--- |
| **Raspberry Pi** | Model 3 / 4 (Quad-Core ARM Cortex-A53, 1GB–4GB RAM, GPIO)[cite: 5] | Central controller processing sensor inputs and switching relays[cite: 5]. |
| **Soil Moisture Sensor** | Capacitive / Resistive (3.3V / 5V)[cite: 5] | Detects moisture content near the root zone[cite: 5]. |
| **Relay Module** | 1-Channel 5V Relay (10A Load Rating)[cite: 5] | Isolates Raspberry Pi GPIO from external power driving the pump[cite: 5]. |
| **Water Pump & Tubing** | Submersible / Centrifugal (5V/12V, 150–250 L/h)[cite: 5] | Supplies water to plant roots upon sensor trigger[cite: 5]. |
| **Power Supply** | Regulated 5V 2.5A/3A Power Adapter & MicroSD Card[cite: 5] | Powers the Pi controller and stores execution code[cite: 5]. |
| **Display (Optional)** | 16x2 LCD or OLED Display[cite: 5] | Displays real-time operational status[cite: 5]. |

---

## 🔌 Hardware Wiring & Pin Mapping

| Component Module | Component Pin | Raspberry Pi GPIO Connection |
| :--- | :--- | :--- |
| **Soil Moisture Sensor** | VCC | Pin 1 (3.3V) or Pin 2 (5V)[cite: 4] |
| **Soil Moisture Sensor** | GND | Pin 6 (GND)[cite: 4] |
| **Soil Moisture Sensor** | DATA / OUT | GPIO 17 (Pin 11)[cite: 4] |
| **5V Relay Module** | VCC | Pin 2 or Pin 4 (5V)[cite: 4] |
| **5V Relay Module** | GND | Pin 9 or Pin 14 (GND)[cite: 4] |
| **5V Relay Module** | IN (Signal) | GPIO 18 (Pin 12)[cite: 4] |
| **Water Pump** | Power Leads | Wired in series via Relay NO (Normally Open)[cite: 4] |

---

## 📐 System Flow Diagram

```text
+-----------------------+        +------------------------+       +----------------------+
| Soil Moisture Sensor  | -----> |      Raspberry Pi      | <---> |     Power Supply     |
| (GPIO 17 / Analog)    |        |  (Central Controller)  |       |   (5V Regulated)     |
+-----------------------+        +------------------------+       +----------------------+
                                             |
                                             v
                                   +-------------------+
                                   |   Relay Module    |
                                   | (GPIO 18 Control) |
                                   +-------------------+
                                             |
                                             v
                                   +-------------------+
                                   |    Water Pump     |
                                   | (Irrigation Unit) |
                                   +-------------------+
                                             |
                                             v
                                   +-------------------+
                                   |  Water Reservoir  |
                                   +-------------------+
```[cite: 4]

---

## 🚀 Setup & Installation

### 1. Raspberry Pi Setup
1. Flash **Raspberry Pi OS** onto a MicroSD card using the Raspberry Pi Imager[cite: 4].
2. Boot the Pi, connect to Wi-Fi/Ethernet, and open a terminal[cite: 4].
3. Update system repositories:
   ```bash
   sudo apt-get update && sudo apt-get upgrade -y
   ```[cite: 4]

### 2. Assembly & Code Execution
1. Connect hardware components according to the pin mapping table above[cite: 4].
2. Create the Python script file:
   ```bash
   nano irrigation_system.py
   ```[cite: 4]
3. Paste the firmware code into the file and save (`Ctrl+O`, `Enter`, `Ctrl+X`)[cite: 4].
4. Run the script with superuser privileges:
   ```bash
   sudo python3 irrigation_system.py
   ```[cite: 4]

---

## 💻 Firmware Code (`irrigation_system.py`)

```python
import RPi.GPIO as GPIO
import time

# Pin Configuration (BCM Numbering)
SOIL_SENSOR_PIN = 17  # GPIO pin for soil moisture sensor
RELAY_PIN = 18        # GPIO pin for 5V relay module

# Setup GPIO Mode
GPIO.setmode(GPIO.BCM)
GPIO.setup(SOIL_SENSOR_PIN, GPIO.IN)
GPIO.setup(RELAY_PIN, GPIO.OUT)

def read_soil_moisture():
    # Reads digital state from soil sensor.
    # Returns True if soil is dry, False if wet.
    if GPIO.input(SOIL_SENSOR_PIN) == 0:
        print("[STATUS] Soil State: DRY (Sensor Signal LOW)")
        return True
    else:
        print("[STATUS] Soil State: WET (Sensor Signal HIGH)")
        return False

# Main Execution Loop
try:
    print("==============================================")
    print("   Smart Irrigation Automation Started")
    print("==============================================")
    
    while True:
        soil_dry = read_soil_moisture()
        
        if soil_dry:
            GPIO.output(RELAY_PIN, GPIO.HIGH)  # Activate relay & pump
            print("[ACTION] Water Pump: ON")
        else:
            GPIO.output(RELAY_PIN, GPIO.LOW)   # Deactivate relay & pump
            print("[ACTION] Water Pump: OFF")
            
        print("----------------------------------------------")
        time.sleep(10)  # Polling interval (10 seconds)

except KeyboardInterrupt:
    print("\n[INFO] Program terminated manually by user.")

finally:
    GPIO.cleanup()
    print("[INFO] GPIO pins cleaned up successfully.")
```[cite: 4]

---

## 🔮 Future Enhancements

* **Cloud Dashboards:** Integrate with platforms like ThingSpeak, Blynk, or custom Flask web dashboards for live telemetry[cite: 5].
* **Weather Forecasting:** Poll weather API services to automatically suppress irrigation cycles when rain is anticipated[cite: 5].
* **Mobile Alerts:** Trigger real-time notifications via Telegram Bot or IFTTT when water levels are low[cite: 5].<img width="217" height="267" alt="Screenshot 2026-08-25 172923" src="https://github.com/user-attachments/assets/f2e3fb20-e87d-4167-bd37-2b292f90ec02" />
<img width="597" height="749" alt="Screenshot 2026-08-25 172937" src="https://github.com/user-attachments/assets/2c90a3a0-7a11-4468-90f1-920b25570b23" />
<img width="502" height="702" alt="Screenshot 2026-08-25 170815" src="https://github.com/user-attachments/assets/dd273e37-c888-432e-a72c-56e37dfc52e7" />
<img width="392" height="414" alt="Screenshot 2026-08-25 170807" src="https://github.com/user-attachments/assets/a06e2ec1-edf0-45a6-aaae-f6873ce7a722" />
<img width="505" height="711" alt="Screenshot 2026-08-25 170733" src="https://github.com/user-attachments/assets/86ba1484-5ab8-489a-84db-4c77e8e94696" />
<img width="506" height="715" alt="Screenshot 2026-08-25 170719" src="https://github.com/user-attachments/assets/b2054336-2526-469d-8f70-8d1d1c961de9" />
