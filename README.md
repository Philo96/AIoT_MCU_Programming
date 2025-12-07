# Smart Home – AIoT MCU (ESP32 + MQTT + Local Broker)

## Overview
This project implements a complete smart‐home system using **ESP32**, **MQTT**, and a **local Mosquitto broker** running on your laptop. Several sensors and actuators are connected to the ESP32, allowing environmental monitoring and remote control through MQTT topics.

The design follows professional embedded‐systems practices, modular code structure, and a scalable MQTT topic tree. The project is fully compatible with Mosquitto, Node-RED, Home Assistant, or any MQTT dashboard.

---

## Key Features

### Smart Environment Monitoring
- Temperature  
- Humidity  
- Gas / Air Quality  
- Flame detection  
- Light level  
- Rain detection  
- Solar system status  

### Smart Actuation
- Fan (with direction + speed)
- Indoor floors lighting
- Outdoor landscape lighting
- RGB LED strip
- House door servo
- Garage door servo
- Windows (front and side)
- Villa gate (servo)

### Communication
- Local Mosquitto broker
- Automatic broker discovery via UDP Beacon
- Multiple MQTT topics (publish/subscribe)
- Structured topic hierarchy

### Software Architecture
- Arduino framework
- PlatformIO build system
- PubSubClient MQTT library
- ArduinoJson
- Code separation
- Callback-based control logic
- Individual topic publishing per sensor
- JSON payload aggregation

---

## Hardware Platform

### Microcontroller
- ESP32 DOIT Devkit V1 (30-pins)

### Sensors
- DHT22 (Temperature + Humidity)
- MQ135 (Gas)
- Flame (digital)
- LDR (analog)
- Rain (digital)

### Actuators
- Servo motors
- RGB LED strip
- DC fan
- Buzzer
- LEDs

### Pinout
See `application.h` for complete pin definitions.

---

## Project Folder Structure
```
|-- include/
|     └── application.h          <-- Declarations
|
|-- src/
|     ├── application.cpp        <-- Logic + MQTT + callback
|     └── main.cpp               <-- Main loop + sensors publish
|
|-- Messages.md                  <-- MQTT API examples
|-- platformio.ini               <-- Build configuration
```

---

## System Architecture
```
       +------------+           +-----------------------+
       |  ESP32     |  MQTT     |    Mosquitto Broker    |
       | (sensors)  +---------->+     Local Laptop       |
       | (actuators)|           +-----------------------+
       +------------+
```

---

## MQTT Topic Tree

### Sensors (publish)
```
home/sensors/temperature           (JSON only)
home/sensors/humidity
home/sensors/gas
home/sensors/ldr
home/sensors/rain
home/sensors/current
home/sensors/voltage
```

### Status
```
home/smart-system/status
```

### Actuators (subscribe)
```
home/actuators/fan
home/actuators/buzzer
home/actuators/lights/floor1
home/actuators/lights/floor2
home/actuators/lights/landscape
home/actuators/lights/rgb
home/actuators/motors/garage
home/actuators/motors/frontwindow
home/actuators/motors/sidewindow
home/actuators/motors/door
```

---

## Example Commands (Linux / Windows PowerShell)

### Turn on fan
```
mosquitto_pub -t home/actuators/fan -m "on"
```

### Open front window
```
mosquitto_pub -t home/actuators/motors/frontwindow -m "open"
```

### Listen to humidity
```
mosquitto_sub -t home/sensors/humidity
```

---

## Installation Guide

### ESP32 platform
```
PlatformIO extension in VSCode
Platform = espressif32
Board = esp32doit-devkit-v1
```

### Dependencies
```ini
lib_deps =
  knolleary/PubSubClient
  bblanchon/ArduinoJson
  adafruit/DHT sensor library
  phoenix1747/MQ135
  arduino-libraries/Servo
```

---

## Building and Flashing

1. Install VSCode + PlatformIO  
2. Open the project folder in VSCode  
3. Connect ESP32 via USB  
4. Press Build  
5. Press Upload  

---

## How It Works (short technical explanation)

1. ESP32 boots and initializes WiFi  
2. Uses UDP beacon discovery to locate local MQTT broker  
3. Subscribes to actuator topics  
4. Publishes sensor values every second  
5. Callback processes incoming messages and controls hardware  

---

## Code Design Principles

- No blocking delays (timer-based approach)  
- Clear callback handler for MQTT commands  
- Topic names reflect functional hierarchy  
- Application layer separated from main loop  
- Minimal memory footprint  
- Professional modular design  

---

## Developer Notes

- Solar system status is readable inside application logic  
- RGB supports two modes: brightness and color  
- Actuators are controlled directly in callback  
- Can be migrated easily to WiFi-MQTT clouds  
- Runs perfectly offline  
