# Integrated Gas Safety and Environmental Monitoring System

This project is a two-node IoT-based gas safety and environmental monitoring system built using ESP8266 microcontrollers. It monitors gas levels, temperature, humidity, pressure, and battery voltage, and automatically controls the gas valve when unsafe conditions are detected.

## Project Overview

- **ESP1 (Kitchen Node):** Monitors kitchen gas, temperature, humidity, and battery level.
- **ESP2 (Main Valve Node):** Monitors gas, temperature, humidity, pressure, 5V battery, and 12V battery.
- Communication between both nodes is done using **ESP-NOW**.
- The system also uses **Blynk**, **ThingSpeak**, and a **web dashboard** for remote monitoring.
- A **16x2 I2C LCD** is used for local display.
- A **relay** is used to close the gas valve in unsafe conditions.
- An **alarm LED** provides local warning.

## Features

- Dual-node monitoring system
- ESP-NOW communication
- Automatic valve control
- Alarm LED alert
- LCD display
- Web dashboard
- Blynk mobile monitoring
- ThingSpeak monitoring
- LittleFS configuration storage
- Battery monitoring
- Sensor calibration and testing

## Hardware Used

- ESP8266 boards
- DHT11 sensors
- MQ4 gas sensors
- CD4051 multiplexer
- Pressure sensor
- Relay module
- Alarm LED
- 16x2 I2C LCD
- Battery sensing circuits

## Project Media

### 3D Design
Project 3D design pictures are available in the `images/3d-design/` folder.

### Circuit Diagrams
Circuit diagram and block diagram are available in the `images/circuit-diagrams/` folder.

### Sensor Readings
Calibration data, MQ4 readings, and graph images are available in the `images/sensor-readings/` folder.

### User Interface
LCD, ThingSpeak, Blynk, and dashboard screenshots are available in the `images/user-interface/` folder.

### Testing
Testing photos are available in the `images/testing/` folder.

## Code Files

- ESP1 kitchen node code
- ESP2 main valve node code
- progress versions
- testing code

## Videos

Video demos and progress videos are available in the `videos/` folder or through linked cloud storage.

## Working Principle

1. ESP1 reads kitchen sensor data.
2. ESP1 sends data to ESP2 using ESP-NOW.
3. ESP2 reads its own sensors.
4. ESP2 compares gas readings with safe thresholds.
5. If gas is unsafe, the relay closes the valve and the alarm LED turns on.
6. Data is shown on LCD, web dashboard, Blynk, and ThingSpeak.

## Conclusion

This project provides a smart and reliable safety solution for gas leakage detection and environmental monitoring with real-time alerts and automatic valve control.
