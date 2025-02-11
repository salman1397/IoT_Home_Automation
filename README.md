# IoT_Home_Automation
# 🏠 Smart Home Automation System using ESP32 & ESP Rainmaker

![Home Automation Banner](assets/banner.gif)

## 🌟 Overview
This project is a **Smart Home Automation System** using **ESP32**, allowing you to control and monitor appliances in real-time via the **ESP Rainmaker App** over **MQTT Protocol**. Additionally, appliances can be controlled using:
- **IR Remote**
- **Touch Switch**
- **Google Home & Alexa Integration**

The system supports automation and remote access, ensuring a seamless and efficient smart home experience.

---

## 🚀 Features
✅ **Control appliances via ESP Rainmaker App, Alexa or G Home(WiFi/MQTT)**  
✅ **IR Remote & Touch Switch for local control**
✅ **Google Home & Alexa Integration for voice control and automation** 
✅ **Real-time appliance monitoring**
✅ **Temperature and Humidity Monitoring** 
✅ **Automation with PIR sensor**  
✅ **Works with or without WiFi**  
✅ **Custom PCB design for compact hardware**  

---

## 🛠️ Components Used
| Component | Description |
|-----------|------------|
| ![ESP32](assets/esp32.png) | ESP32 WiFi Module |
| ![IR Sensor](assets/ir_sensor.png) | IR Receiver for Remote Control |
| ![Touch Switch](assets/touch_switch.png) | Capacitive Touch Switch |
| ![Relay Module](assets/relay_module.png) | Relay Module for Switching Appliances |
| ![PIR Sensor](assets/pir_sensor.png) | PIR Motion Sensor for Automation |
| ![Power Supply](assets/power_supply.png) | Power Module for ESP32 |

---

## 📱 Mobile App (ESP Rainmaker)
Control and monitor your home appliances using the **ESP Rainmaker App** available for **Android & iOS**. Features include:
- Real-time device control
- Automation settings
- Voice command integration with Google Home & Alexa

![ESP Rainmaker](assets/esp_rainmaker_app.png)

---

## 🔧 Installation & Setup

### 1️⃣ Firmware & Code Upload
- Install **Arduino IDE** with ESP32 Board Manager.
- OpenThe Application and Install **ESP32 Board Maneger** `https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json`
- Install required libraries: `ESP Rainmaker`, `Adafruit Sensor`, `IRremote`, `Ace Button` etc.
- Flash the code to ESP32 using **USB**.

### 2️⃣ ESP Rainmaker Setup
- Install the **ESP Rainmaker App** from Play Store/App Store, and Create an account
- Press boot button of ESP32 for 10 Seconds for pairing mode
- Then Press + icon on the app and connect the device with Bluetooth
- After connect with bluetooth succesfully then required WiFi SSID and Password or Router
- Then automatically device added to the app

---

## 📡 Working Principle
1️⃣ ESP32 connects to **WiFi** and communicates with the ESP Rainmaker Cloud.  
2️⃣ Users send commands via the **ESP Rainmaker App** to control appliances.  
3️⃣ Appliances can also be controlled using **IR Remote** and **Touch Switch**.  
4️⃣ Voice assistants like **Alexa & Google Home** trigger automation scenes.  
5️⃣ **PIR Sensor** enables automatic switching based on motion detection.  
6️⃣ The system works even if WiFi is unavailable, ensuring **reliable control**.

---

## 📸 Demo & Images
![System Demo](assets/demo.gif)

---

## ⚙️ Future Enhancements
- 📡 **Add Bluetooth Control**
- 🌐 **Integration with Home Assistant**
- 🎙️ **Custom Voice Commands**
- 🔋 **Energy Monitoring Feature**

---

## 🏗️ Project Contributors
🔹 **[Your Name]** - Developer & Hardware Engineer  
🔹 **[Your GitHub](https://github.com/yourgithub)** - Repository Maintainer  

---

## 📜 License
This project is **open-source** and released under the **MIT License**.

---

## ⭐ Show Some Love!
If you liked this project, don't forget to **🌟 Star** the repo and **fork** it for future improvements!

[![GitHub Stars](https://img.shields.io/github/stars/yourgithub/home-automation.svg?style=social)](https://github.com/yourgithub/home-automation)
