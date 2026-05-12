#  ESP32-CAM AI Live Stream Plus Motion Detection
### *Live A/V Streaming, HC-SR501 Motion Intelligence, and Browser Recording*

This project transforms an ESP32-CAM into a professional-grade smart doorbell. It features high-fidelity digital audio via I2S, real-time MJPEG video, and hardware-level motion detection. The unified web dashboard allows for live monitoring and one-click browser-side recording—no SD card required.

---

## 🛠 Materials & Purchase Links

The following components are required for this build. Each link leads directly to specific search results on AliExpress to ensure you find the correct parts.

| Component | Role | AliExpress Direct Link |
| :--- | :--- | :--- |
| **ESP32-CAM + MB Shield** | Main MCU & USB Programmer | [Buy on AliExpress](https://www.aliexpress.com/w/wholesale-ESP32-CAM-MB.html) |
| **INMP441 Microphone** | I2S Digital Audio Input | [Buy on AliExpress](https://www.aliexpress.com/w/wholesale-INMP441.html) |
| **HC-SR501 Sensor** | PIR Motion Detection | [Buy on AliExpress](https://www.aliexpress.com/w/wholesale-HC-SR501.html) |
| **OV2640 Sensor** | 2MP Camera Module | [Buy on AliExpress](https://www.aliexpress.com/w/wholesale-OV2640-module.html) |
| **Jumper Wires** | F-F & M-F Ribbon Cables | [Buy on AliExpress](https://www.aliexpress.com/w/wholesale-jumper-wires.html) |
| **100µF Capacitor** | Power Rail Stabilization | [Buy on AliExpress](https://www.aliexpress.com/w/wholesale-100uf-capacitor.html) |

---

## 📐 Wiring & Pin Configuration

The firmware is optimized to use specific GPIO pins to prevent interference with the camera's internal 8-bit bus and the PSRAM.

### 1. Audio (INMP441 I2S Microphone)
Digital audio requires precise timing. Ensure connections are secure to minimize interference.
| Mic Pin | ESP32-CAM GPIO | Description |
| :--- | :--- | :--- |
| **VDD** | 3.3V | Power Input |
| **GND / L/R** | GND | Ground (L/R to GND = Left Channel) |
| **SCK** | **GPIO 14** | Serial Clock |
| **WS** | **GPIO 15** | Word Select |
| **SD** | **GPIO 12** | Serial Data |

### 2. Motion (HC-SR501 PIR Sensor)
| PIR Pin | ESP32-CAM GPIO | Note |
| :--- | :--- | :--- |
| **VCC** | **5V** | Required for full sensor range |
| **OUT** | **GPIO 13** | Signal Input |
| **GND** | GND | Common Ground |

### 3. Onboard Indicators
| Component | GPIO | Function |
| :--- | :--- | :--- |
| **Flash LED** | **GPIO 4** | High-intensity white LED (Motion Alert) |
| **Indicator LED** | **GPIO 33** | Small red LED (System Status) |

---

## ⚙️ Software Setup

### Required Environment
1.  **Arduino IDE:** v2.0 or higher recommended.
2.  **ESP32 Core:** Ensure you have the `esp32` by Espressif Systems board package installed (v2.0.x or later).
3.  **Required Libraries:**
    * `esp_camera.h` (Built-in)
    * `driver/i2s.h` (Built-in)
    * `WebServer.h` (Built-in)

### Flashing Instructions
1.  Open `ESP32-CAM_Audio.ino`.
2.  Update the **WiFi credentials** (`ssid` and `password`) to match your home network.
3.  **Board Selection:** Select **AI Thinker ESP32-CAM**.
4.  **Partition Scheme:** Select **Huge APP (3MB No OTA/1MB SPIFFS)**. This is necessary because the web server and camera libraries are large.
5.  **Upload:** If using the MB Shield, hold the **IO0 button** while plugging in the USB to enter bootloader mode. Click Upload.

---

## 🚀 How to Use

1.  **Boot Up:** Once flashed, open the Serial Monitor (115200 baud) and press the Reset button on the ESP32.
2.  **Access the Dashboard:** Copy the IP address shown in the Serial Monitor and paste it into your browser followed by `:86` (e.g., `http://192.168.1.50:86`).
3.  **Live View:** You will see the synchronized MJPEG video and WAV audio stream.
4.  **Record Clips:** Click the **"RECORD CLIP"** button. This will record the live stream directly to your browser's download folder as a `.webm` file.

---

## 🔧 Hardware Tuning & Troubleshooting

### Motion Sensor (HC-SR501)
* **False Triggers:** The ESP32's WiFi antenna can interfere with the PIR sensor. Keep the sensor at least 15cm away from the module or shield the back of the PIR board with aluminum foil.
* **Sensitivity:** Turn the **Sx** (Sensitivity) potentiometer clockwise to increase detection distance.
* **Time Delay:** Turn the **Tx** potentiometer fully counter-clockwise (shortest delay) to allow the code to handle the timing logic.

### Power Issues
* **Brownout Error:** If the device keeps restarting when the camera starts, your power supply is insufficient. Use a 5V 2A adapter and place a **100µF capacitor** between the 5V and GND pins of the ESP32-CAM.
* **Audio Noise:** Ensure the INMP441 `GND` and `L/R` pins are both connected to a clean Ground. Keep I2S wires short. If the microphopne doesn't work disconnect the L/R pin from ground this can fix it if you have a  INMP441 from AliExpress

---
<img width="915" height="649" alt="image" src="https://github.com/user-attachments/assets/66b83786-45d5-44a0-9c8e-e7a3f7823689" />
