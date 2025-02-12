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
|![ESP32](https://raw.githubusercontent.com/salman1397/IoT_Home_Automation/main/images/esp32.png) | ESP32 WiFi Module |
| ![IR](https://raw.githubusercontent.com/salman1397/IoT_Home_Automation/main/images/ir.jpg) | IR Receiver for Remote Control |
| ![BC547](https://raw.githubusercontent.com/salman1397/IoT_Home_Automation/main/images/bc547.jpg)  | BC547 NPN Transistor |
| ![dht11](https://raw.githubusercontent.com/salman1397/IoT_Home_Automation/main/images/dht11.jpg)  | DHT11 Temperature & Humidity Sensor |
| ![IR](https://raw.githubusercontent.com/salman1397/IoT_Home_Automation/main/images/pc817.jpg) | PC817 Optocoupler |
| ![IR](https://raw.githubusercontent.com/salman1397/IoT_Home_Automation/main/images/ps.jpg)  | Power Supply Module |
|![IR](https://raw.githubusercontent.com/salman1397/IoT_Home_Automation/main/images/relay.jpg) | Relay For Switching Application |
|![IR](https://raw.githubusercontent.com/salman1397/IoT_Home_Automation/main/images/pir.jpg) | PIR Motion Sensor for Automation |

---

## 📱 Mobile App (ESP Rainmaker)
Control and monitor your home appliances using the **ESP Rainmaker App** available for **Android & iOS**. Features include:
- Real-time device control
- Automation settings
- Voice command integration with Google Home & Alexa

![ESP Rainmaker](https://raw.githubusercontent.com/salman1397/IoT_Home_Automation/main/images/screen1.png)

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
- After connect with bluetooth succesfully then required WiFi SSID and Password of Router for WiFi Provisioning
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

## 📸 Demo
![System Demo](assets/demo.gif)
<a href="https://www.youtube.com/watch?v=RzM9oUeDlkI">
    <img src="https://cdn-icons-png.flaticon.com/512/1384/1384060.png" width="40" height="40">
</a>

---

## 📫 Let's Connect!

<p align="left">
<a href="https://www.linkedin.com/in/salman151397">
    <img src="https://cdn-icons-png.flaticon.com/512/174/174857.png" width="40" height="40">
</a>
<span style="display:inline-block; width: 250px;"></span>
<a href="https://www.youtube.com/@SmartTechInsights-e9j">
    <img src="https://cdn-icons-png.flaticon.com/512/1384/1384060.png" width="40" height="40">
</a>
<span style="display:inline-block; width: 250px;"></span>
<a href="mailto:salman151397@gmail.com">
    <img src="https://cdn-icons-png.flaticon.com/512/732/732200.png" width="40" height="40">
</a>
</p>





[![GitHub Stars](https://img.shields.io/github/stars/salman1397/IoT_Home_Automation.svg?style=social)](https://github.com/salman1397/IoT_Home_Automation/HomeAutomation/HomeAutomation.ino)
