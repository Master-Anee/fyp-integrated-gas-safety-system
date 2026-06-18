# Integrated Gas Safety and Environmental Monitoring System
<img width="4345" height="3178" alt="2025_06_18_16_52_IMG_2053" src="https://github.com/user-attachments/assets/663a981b-60ce-458b-b437-4f604f0eec4e" />

![Status](https://img.shields.io/badge/Status-Completed-success)
![Platform](https://img.shields.io/badge/Platform-ESP8266-blue)
![Communication](https://img.shields.io/badge/Communication-ESP--NOW-orange)
![IoT](https://img.shields.io/badge/IoT-Blynk%20%7C%20ThingSpeak-green)
![License](https://img.shields.io/badge/Use-Educational%20Project-lightgrey)

---

## Table of Contents

- [Project Overview](#project-overview)
- [Problem Statement](#problem-statement)
- [Project Title](#project-title)
- [Project Objectives](#project-objectives)
- [System Summary](#system-summary)
- [Main Features](#main-features)
- [Hardware Architecture](#hardware-architecture)
- [Software Architecture](#software-architecture)
- [Communication Flow](#communication-flow)
- [Control and Safety Logic](#control-and-safety-logic)
- [Data Structures Used](#data-structures-used)
- [Sensors and Their Purpose](#sensors-and-their-purpose)
- [Multiplexer Use](#multiplexer-use)
- [Web Dashboard](#web-dashboard)
- [Blynk and ThingSpeak Integration](#blynk-and-thingspeak-integration)
- [LCD Display Pages](#lcd-display-pages)
- [Configuration Storage with LittleFS](#configuration-storage-with-littlefs)
- [Project Folders and Media](#project-folders-and-media)
- [Hardware List](#hardware-list)
- [Code Files](#code-files)
- [Testing and Calibration](#testing-and-calibration)
- [Results](#results)
- [Future Improvements](#future-improvements)
- [Conclusion](#conclusion)
- [Project Media Links](#project-media-links)
- [Author](#author)

---

## Project Overview

The **Integrated Gas Safety and Environmental Monitoring System** is a two-node IoT-based safety and monitoring solution built using **ESP8266** microcontrollers. The project is designed to monitor gas leakage and environmental conditions in real time, while also providing automatic safety action when dangerous conditions are detected.

The system is divided into two smart nodes:

- **ESP1 (Kitchen Node)**: monitors the kitchen-side environment.
- <img width="3024" height="4032" alt="Kitchen_Node_Pic" src="https://github.com/user-attachments/assets/de1b3c77-d320-44aa-8695-354722ed2cdf" />

- **ESP2 (Main Valve Node)**: monitors the valve-side environment, receives data from ESP1, makes safety decisions, and controls the gas valve.
<img width="3024" height="4032" alt="MainValve_Node_PIC" src="https://github.com/user-attachments/assets/1aee6b76-3eb7-49c4-a526-6d04f290870d" />

These two nodes communicate using **ESP-NOW**, which provides fast and reliable local wireless communication without needing a central server between them.

The project also includes:

- **LCD display** for local monitoring
- **Web dashboard** for browser-based monitoring
- **Blynk** for mobile monitoring
- **ThingSpeak** for cloud-based data visualization
- **LittleFS** for storing Wi-Fi and Blynk credentials permanently
- **Relay-based automatic valve control**
- **Alarm LED** for warning indication

This project is not only a monitoring system. It is also a **real-time safety control system**.

---

## Problem Statement

Gas leakage is a serious safety risk in homes, kitchens, and gas-based environments. In many systems, users only receive passive monitoring without automatic response. This project solves that problem by combining:

- local sensor monitoring,
- wireless inter-node communication,
- cloud and mobile dashboards,
- and automatic valve shutdown.

The goal is to create a system that can:

- detect gas leakage quickly,
- alert the user immediately,
- monitor environmental conditions,
- and take action before the situation becomes dangerous.

---

## Project Title

**Final Year Project: Integrated Gas Safety and Environmental Monitoring System**

---

## Project Objectives

The main objectives of this project are:

1. Detect gas leakage at two different points of the system.
2. Monitor temperature and humidity in both nodes.
3. Measure pressure on the valve-side node.
4. Monitor battery health for reliability.
5. Provide automatic valve control during unsafe gas conditions.
6. Send data between two ESP8266 boards using ESP-NOW.
7. Display live data on LCD, web dashboard, Blynk, and ThingSpeak.
8. Store configuration data using LittleFS so it survives power loss.
9. Build a complete IoT-based smart safety platform.

---

## System Summary

This project uses two ESP8266 boards:

### ESP1 — Kitchen Node
ESP1 is responsible for reading local kitchen data:

- gas level
- temperature
- humidity
- battery percentage

It sends this data to ESP2 using ESP-NOW.

### ESP2 — Main Valve Node
ESP2 is the central unit and safety controller. It:

- receives kitchen data from ESP1,
- reads its own gas, temperature, humidity, pressure, and batteries,
- checks whether gas levels are safe,
- closes the valve if necessary,
- activates the alarm LED,
- updates the LCD,
- serves the web dashboard,
- and sends selected values to Blynk and ThingSpeak.

---

## Main Features

- Dual-node monitoring architecture
- Fast local communication using ESP-NOW
- Automatic gas valve shutdown
- Real-time alarm LED warning
- Live LCD display with multiple pages
- Web dashboard for browser monitoring
- Blynk integration for phone monitoring
- ThingSpeak integration for data visualization
- LittleFS-based credential storage
- Battery monitoring for safety and reliability
- Sensor calibration and testing support
- Modular design for code and hardware expansion

---

## Hardware Architecture

The system is divided into two main hardware sections.

### 1) ESP1 — Kitchen Node

#### Hardware used
- ESP8266
- DHT11 sensor
- MQ-4 gas sensor
- CD4051 multiplexer
- Battery sensing circuit
- Status LED
- Wi-Fi connection
- ESP-NOW transmitter
- Blynk client
- LittleFS storage

#### Main role
ESP1 monitors the kitchen environment and transmits data to ESP2.

#### Values measured by ESP1
- `gas1`
- `temp1`
- `hum1`
- `batt1`

---

### 2) ESP2 — Main Valve Node

#### Hardware used
- ESP8266
- DHT11 sensor
- MQ-4 gas sensor
- Pressure sensor
- CD4051 multiplexer
- 5V battery sensing circuit
- 12V battery sensing circuit
- Relay module
- Alarm LED
- 16×2 I2C LCD
- Wi-Fi connection
- ESP-NOW receiver
- Blynk client
- Web server
- LittleFS storage

#### Main role
ESP2 is the main decision-making unit. It receives kitchen data, reads local sensors, controls the valve, and updates all interfaces.

#### Values measured by ESP2
- `gas2`
- `temp2`
- `hum2`
- `pressure`
- `batt5`
- `batt12`

---

## Software Architecture

The software is designed as a full IoT stack:

### ESP1 software responsibilities
- initialize Wi-Fi and ESP-NOW
- load credentials from LittleFS
- read local sensors
- convert analog readings to useful values
- fill the data message structure
- send the data packet to ESP2
- update Blynk values
- print serial debug information
- monitor communication timeout

### ESP2 software responsibilities
- initialize Wi-Fi, LCD, Blynk, LittleFS, and ESP-NOW
- receive kitchen values from ESP1
- read local sensor values
- apply safety thresholds
- control the relay and alarm LED
- update LCD pages
- serve the web dashboard
- provide JSON data endpoint
- update Blynk and ThingSpeak
- print all system data to Serial Monitor

---

## Communication Flow

The system uses two communication layers:

### A) ESP-NOW between ESP1 and ESP2
ESP-NOW is used for direct board-to-board data transfer.

Why ESP-NOW is useful here:
- fast
- low latency
- does not require internet for node-to-node communication
- reliable for local wireless exchange
- ideal for safety-related applications

### B) Wi-Fi to Router
Both boards also connect to Wi-Fi so they can access:
- Blynk
- ThingSpeak
- web dashboard
- IP-based monitoring
- cloud features

---

## Control and Safety Logic

The core of the project is the safety logic inside ESP2.

ESP2 continuously checks:
- kitchen gas value (`remoteData.gas1`)
- local gas value (`gas2`)

### Safe condition
If both values are inside the safe range:
- valve remains open
- alarm LED stays off
- dashboard shows safe status

### Unsafe condition
If either gas sensor exceeds the threshold:
- relay changes state to close the valve
- alarm LED activates
- warning appears on LCD
- web dashboard updates
- Blynk and ThingSpeak are refreshed

This makes the system a **smart automatic gas protection system**, not just a monitoring system.

---

## Data Structures Used

The project uses a simple and clear message design.

### `MsgData`
This structure contains sensor and state data.

Fields:
- `gas1`
- `temp1`
- `hum1`
- `batt1`
- `valve1`

ESP1 fills this structure and sends it to ESP2.

### `MsgConfig`
This structure carries configuration settings.

Fields:
- `ssid`
- `pass`
- `blynkAuth`

### `MsgAny`
This is the wrapper message that identifies the packet type.

Fields:
- `type`
- data payload

The packet type tells the receiver whether the packet contains:
- sensor data
- configuration data

This keeps communication organized and extensible.

---

## Sensors and Their Purpose

### Gas Sensor on ESP1
Used to detect gas leakage near the kitchen source.

### Gas Sensor on ESP2
Used to detect gas leakage near the valve or outside side.

### DHT11 on ESP1
Measures:
- temperature
- humidity

### DHT11 on ESP2
Measures:
- temperature
- humidity

### Pressure Sensor on ESP2
Monitors gas line pressure and helps identify abnormal pressure behavior.

### Battery Sensors
Used to monitor system power health:
- ESP1 battery
- ESP2 5V battery
- ESP2 12V battery

Battery monitoring is important because if the supply becomes weak, the sensors, relay, Wi-Fi, and alarm may not work correctly.

---

## Multiplexer Use

ESP8266 has only one analog input pin: **A0**.

Because the project needs multiple analog readings, the **CD4051 multiplexer** is used.

### Why the multiplexer is needed
It allows the ESP8266 to read multiple analog sensors using a single ADC input.

### ESP1 MUX channels
- Channel 0: gas sensor
- Channel 1: battery sensor

### ESP2 MUX channels
- Channel 0: gas sensor
- Channel 1: pressure sensor
- Channel 2: 5V battery
- Channel 3: 12V battery

This is one of the key design decisions that makes the project compact and practical.

---

## Web Dashboard

ESP2 hosts a web dashboard that shows the complete state of the system.

### Dashboard displays
- Kitchen node gas level
- Kitchen node temperature
- Kitchen node humidity
- Kitchen node battery
- Main valve node gas level
- Main valve node temperature
- Main valve node humidity
- Pressure value
- Battery values
- Valve state
- Alarm status
- IP address
- Communication status

### Dashboard benefits
- easy to access from browser
- no special app required
- useful during testing and demonstration
- provides complete real-time visibility

ESP2 also exposes a JSON endpoint, which can be used later for:
- mobile app integration
- custom tools
- automation systems
- data logging systems

---

## Blynk and ThingSpeak Integration

### Blynk
Blynk is used for mobile monitoring.

It provides:
- live readings on phone
- simple remote monitoring
- warning display
- convenient access from anywhere

### ThingSpeak
ThingSpeak is used for:
- cloud monitoring
- data logging
- charts and graphs
- sensor trend analysis

These platforms are not the decision-making brain of the system.  
The decision-making remains inside **ESP2**, which is important for safety.

---

## LCD Display Pages

ESP2 uses a 16×2 I2C LCD to show local system information.

### LCD page 0
- IP address
- network status

### LCD page 1
- temperature and humidity from both nodes

### LCD page 2
- gas readings from both nodes

### LCD page 3
- pressure
- valve state

### LCD page 4
- battery values

The LCD is useful because it gives direct local feedback even if the browser or phone is not available.

---

## Configuration Storage with LittleFS

LittleFS is used to store:
- Wi-Fi SSID
- Wi-Fi password
- Blynk authentication token

### Why LittleFS is important
- saves configuration permanently
- no need to hardcode credentials every time
- survives power failure
- useful for updating settings later
- makes the system more flexible

This is a strong feature because it makes the project more complete and practical.

---

## Project Folders and Media

The project media is organized inside the following Google Drive structure:

**Main folder:**  
`FYP Integrated Gas Safety And Environmental Monitoring System`

### Subfolders
1. **3D design**
   - 3D design pictures

2. **circuit Diagrams**
   - circuit diagram picture
   - block diagram

3. **ide codes file**
   - progress 1 code
   - progress 2 code
   - progress 3 code
   - progress 4 code
   - testing code

4. **sensor readings**
   - calculations
   - MQ-4 sensor readings
   - graphs

5. **testing**
   - testing 1
   - testing 2: valve connection to gas
   - testing 3: dashboard checking

6. **user interface**
   - LCD display images
   - ThingSpeak screenshots
   - Blynk screenshots
   - personal dashboard screenshots

7. **Videos**
   - general video
   - progress 1
   - progress 2
   - progress 3
   - progress 4
   - testing videos

---

## Hardware List

| Component | Purpose |
|---|---|
| ESP8266 x2 | Main microcontrollers |
| MQ-4 Gas Sensors | Gas detection |
| DHT11 x2 | Temperature and humidity monitoring |
| CD4051 Multiplexers x2 | Multiple analog channel selection |
| Pressure Sensor | Gas pressure monitoring |
| Relay Module | Gas valve switching |
| Alarm LED | Warning indicator |
| 16×2 I2C LCD | Local display |
| Battery Monitoring Circuit | Power health monitoring |
| Wi-Fi Router | Network connectivity |

---

## Code Files

The source code is organized into development stages.

### ESP1 code
- kitchen node code
- sensor reading code
- ESP-NOW send logic
- Blynk update logic
- LittleFS config logic

### ESP2 code
- main valve node code
- ESP-NOW receiver logic
- valve control logic
- LCD display logic
- dashboard and JSON server logic
- Blynk and ThingSpeak update logic

### Progress versions
- progress 1
- progress 2
- progress 3
- progress 4

### Testing code
- final testing firmware
- valve integration testing
- dashboard testing build

---

## Testing and Calibration

A major part of this project is proper testing and calibration.

### Sensor calibration includes:
- MQ-4 response readings
- analog value conversion
- battery percentage conversion
- pressure reading checks
- threshold tuning

### Testing includes:
- ESP-NOW communication test
- gas sensor response test
- valve relay switching test
- LCD display verification
- Blynk dashboard verification
- ThingSpeak chart verification
- web dashboard validation
- integration testing of full system

### Testing media included
- testing photos
- testing videos
- sensor reading graphs
- calibration calculations

This is important because a safety system must not only compile correctly, but must also behave correctly in real-world conditions.

---

## Results

The project successfully demonstrates:

- real-time gas monitoring
- environmental condition monitoring
- dual-node wireless communication
- automatic safety response
- local and remote visualization
- battery health monitoring
- local LCD feedback
- browser dashboard access
- mobile dashboard access

The final system behaves as a complete intelligent safety solution for gas monitoring and automatic valve control.

---

## Future Improvements

Possible improvements for future versions:

- GSM/SMS alert system
- mobile notification system
- data logging to a database
- predictive gas fault analysis
- improved sensor calibration
- backup communication channel
- battery backup optimization
- mobile app customization
- ML-based anomaly detection
- enhanced safety logic with multi-level thresholds

---

## Conclusion

The **Integrated Gas Safety and Environmental Monitoring System** is a complete IoT-based smart safety project designed to monitor gas leakage and environmental conditions while protecting the system through automatic valve control.

It combines:
- ESP8266 microcontrollers
- ESP-NOW communication
- Wi-Fi connectivity
- Blynk
- ThingSpeak
- LittleFS
- LCD display
- Web dashboard
- Relay-based valve automation

The final result is a professional and practical embedded system that is suitable for a university Final Year Project and also strong enough to present in a portfolio.

---

## Project Media Links

### Google Drive Folder
[View Project Media Folder](https://drive.google.com/drive/folders/1qoRRjSMr3AxVFJfdwFxN7i4svPKSzAJ0?usp=sharing)

---

## Author

**Anees**  
Final Year Project  
Integrated Gas Safety and Environmental Monitoring System
