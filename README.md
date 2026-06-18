# Integrated Gas Safety and Environmental Monitoring System
<img width="4345" height="3178" alt="2025_06_18_16_52_IMG_2053" src="https://github.com/user-attachments/assets/e2fd704e-63b1-431f-a812-db3a1fab0346" />

![Status](https://img.shields.io/badge/Status-Completed-success)
![Platform](https://img.shields.io/badge/Platform-ESP8266-blue)
![Communication](https://img.shields.io/badge/Communication-ESP--NOW-orange)
![IoT](https://img.shields.io/badge/IoT-Blynk%20%7C%20ThingSpeak-green)
![Project](https://img.shields.io/badge/Project-Final%20Year%20Project-purple)

A Final Year Project built around **two ESP8266 nodes** for **gas safety**, **environment monitoring**, **remote visualization**, and **automatic valve control**.

---

## Table of Contents

- [Project Overview](#project-overview)
- [Project Goal](#project-goal)
- [System Highlights](#system-highlights)
- [System Architecture](#system-architecture)
- [Hardware Used](#hardware-used)
- [How the System Works](#how-the-system-works)
- [Communication Design](#communication-design)
- [Control Logic](#control-logic)
- [Data Structures](#data-structures)
- [Sensor Details](#sensor-details)
- [Multiplexer Use](#multiplexer-use)
- [Display and Dashboard](#display-and-dashboard)
- [Configuration Storage](#configuration-storage)
- [Testing and Calibration](#testing-and-calibration)
- [Google Drive Folder Structure](#google-drive-folder-structure)
- [Repository Folder Structure](#repository-folder-structure)
- [Project Media](#project-media)
- [Results](#results)
- [Future Improvements](#future-improvements)
- [Conclusion](#conclusion)
- [Author Information](#author-information)
- [Project Supervisor](#project-supervisor)
- [Academic Reference](#academic-reference)
- [License](#license)

---

## Project Overview

The **Integrated Gas Safety and Environmental Monitoring System** is a two-node IoT-based safety solution developed as a **Final Year Project (FYP)**. It is designed to monitor gas leakage, environmental conditions, battery health, and gas line pressure, while also providing automatic safety response when dangerous conditions are detected.

The system is built using **two ESP8266 boards**:

- **ESP1 (Kitchen Node):** monitors kitchen-side gas, temperature, humidity, and battery status.

<img width="3024" height="4032" alt="Kitchen_Node_Pic" src="https://github.com/user-attachments/assets/118e8b54-6f3a-440f-9df5-51d46d4a9d17" />


  
- **ESP2 (Main Valve Node):** monitors valve-side gas, temperature, humidity, pressure, and battery levels, while also receiving ESP1 data and controlling the gas valve.

<img width="3024" height="4032" alt="MainValve_Node_PIC" src="https://github.com/user-attachments/assets/81ec0919-efe9-4c94-bf87-9606875080ee" />



The boards communicate using **ESP-NOW**, and both connect to Wi-Fi for **Blynk**, **ThingSpeak**, and a **web dashboard**. ESP2 acts as the main control node and safety decision unit.

<img width="820" height="456" alt="block_diagram_Updated" src="https://github.com/user-attachments/assets/8f19cd5a-c587-4667-8a85-6fad09784922" />


---

## Project Goal

The goal of this project is to create a smart embedded safety system that:

- detects gas leakage in real time,
- monitors environmental conditions,
- checks power health through battery sensing,
- automatically closes the valve during danger,
- gives local warnings using an alarm LED,
- shows live data on LCD and dashboards,
- stores configuration securely in LittleFS,
- and allows remote monitoring through mobile and browser interfaces.

---

## System Highlights

- Dual-node ESP8266 architecture
- ESP-NOW communication between nodes
- Automatic gas valve shutdown
- Alarm LED for immediate warning
- Live 16×2 I2C LCD display
- Web dashboard for browser monitoring
- Blynk for mobile monitoring
- ThingSpeak for cloud charts and logging
- LittleFS for saving Wi-Fi and Blynk credentials
- Gas, temperature, humidity, pressure, and battery monitoring
- Calibration and testing support
- Professional modular project structure

---

## System Architecture

### ESP1 — Kitchen Node

ESP1 is responsible for the kitchen-side monitoring.

#### ESP1 reads:
- Kitchen gas level
- Temperature
- Humidity
- Battery percentage

#### ESP1 sends:
- Sensor readings to ESP2 through ESP-NOW

#### ESP1 also:
- connects to Wi-Fi,
- updates Blynk,
- stores credentials in LittleFS,
- and prints debugging information on Serial Monitor.

---

### ESP2 — Main Valve Node

ESP2 is the central safety controller.

#### ESP2 reads:
- Local gas level
- Temperature
- Humidity
- Pressure
- 5V battery
- 12V battery

#### ESP2 receives:
- Kitchen data from ESP1

#### ESP2 controls:
- gas valve relay
- alarm LED
- LCD pages
- web dashboard
- Blynk updates
- ThingSpeak updates

ESP2 compares gas levels from both nodes and decides whether the valve should remain open or close for safety.

---

## Hardware Used

| Component | Quantity | Purpose |
|---|---:|---|
| ESP8266 | 2 | Main controllers |
| MQ-4 Gas Sensor | 2 | Gas detection |
| DHT11 Sensor | 2 | Temperature and humidity |
| CD4051 Multiplexer | 2 | Multiple analog channel selection |
| Pressure Sensor | 1 | Gas line pressure monitoring |
| Relay Module | 1 | Valve control |
| Alarm LED | 1 | Local warning indicator |
| 16×2 I2C LCD | 1 | Local status display |
| Battery Monitoring Circuits | 3 | Power health monitoring |
| Wi-Fi Router | 1 | Network connectivity |

---

## How the System Works

1. Power is applied to both ESP8266 nodes.
2. Both boards load saved Wi-Fi and Blynk credentials from LittleFS.
3. Both nodes connect to Wi-Fi.
4. ESP1 reads kitchen gas, temperature, humidity, and battery level.
5. ESP1 sends its readings to ESP2 using ESP-NOW.
6. ESP2 reads its own gas, pressure, temperature, humidity, and battery values.
7. ESP2 combines both local and remote readings.
8. ESP2 compares gas values with safety thresholds.
9. If unsafe gas is detected, ESP2 closes the valve using a relay.
10. Alarm LED activates to provide local warning.
11. LCD, web dashboard, Blynk, and ThingSpeak all update in real time.
12. The system continues monitoring in a continuous loop.

---

## Communication Design

This project uses **two communication methods**:

### 1) ESP-NOW
ESP-NOW is used for direct communication between ESP1 and ESP2.

#### Why ESP-NOW is used:
- fast
- low latency
- does not require internet between nodes
- reliable for board-to-board communication
- well suited for safety-critical data exchange

ESP1 sends kitchen readings to ESP2 continuously.

### 2) Wi-Fi
Both ESP boards connect to Wi-Fi for:
- Blynk cloud access
- ThingSpeak access
- web dashboard access
- IP-based monitoring
- remote viewing and debugging

---

## Control Logic

The safety logic is implemented inside **ESP2**.

ESP2 checks:
- `remoteData.gas1` from ESP1
- `gas2` from its own local sensor

### Safe Condition
If both readings are below the threshold:
- valve remains open
- alarm remains off
- dashboard shows safe status

### Unsafe Condition
If either gas reading crosses the threshold:
- relay changes state
- valve closes
- alarm LED turns on
- warning appears on LCD
- dashboard updates immediately
- Blynk and ThingSpeak reflect the unsafe condition

This makes the system a **monitoring + protection** system rather than only a monitoring system.

---

## Data Structures

The communication between nodes uses structured packets.

### `MsgData`
Contains sensor and status values:

- `gas1`
- `temp1`
- `hum1`
- `batt1`
- `valve1`

ESP1 fills this packet and sends it to ESP2.

### `MsgConfig`
Contains configuration values:

- `ssid`
- `pass`
- `blynkAuth`

### `MsgAny`
This is the wrapper message type used to identify the packet category:

- sensor data packet
- configuration packet

This helps both boards understand what type of message is being transmitted.

---

## Sensor Details

### Gas Sensor on ESP1
Used to detect kitchen-side gas leakage.

### Gas Sensor on ESP2
Used to detect valve-side or external gas leakage.

### DHT11 on ESP1
Measures:
- temperature
- humidity

### DHT11 on ESP2
Measures:
- temperature
- humidity

### Pressure Sensor on ESP2
Monitors gas pressure near the valve side.

### Battery Sensors
Used to monitor:
- ESP1 battery
- ESP2 5V battery
- ESP2 12V battery

Battery monitoring is important because this is a safety-critical system. If power becomes weak, communication or actuation can fail.

---

## Multiplexer Use

ESP8266 has only **one analog input pin (A0)**, but this project needs multiple analog readings. That is why the **CD4051 multiplexer** is used.

### ESP1 Multiplexer Channels
- Channel 0 → gas sensor
- Channel 1 → battery sensing

### ESP2 Multiplexer Channels
- Channel 0 → gas sensor
- Channel 1 → pressure sensor
- Channel 2 → 5V battery sensing
- Channel 3 → 12V battery sensing

This design allows the ESP8266 to read multiple sensors using one ADC input.

---

## Display and Dashboard

### LCD Display
ESP2 uses a 16×2 I2C LCD to show live values locally.

#### LCD pages can show:
- IP address
- kitchen and valve-side temperature/humidity
- gas values from both nodes
- pressure value
- valve state
- battery values

<img width="4032" height="3024" alt="LCD-Dashboard_IP_Address" src="https://github.com/user-attachments/assets/274f2b90-3d18-436b-94b9-82b69d159b98" />
<img width="4032" height="3024" alt="LCD_ValveStatus" src="https://github.com/user-attachments/assets/39364cb7-3147-4126-b61d-4301a8378ff4" />
<img width="4032" height="3024" alt="LCD_humidity_pressure _Values" src="https://github.com/user-attachments/assets/d6fab3c1-9e10-49ec-a873-19805d4fc246" />
<img width="4032" height="3024" alt="LCD_gas_values" src="https://github.com/user-attachments/assets/a3ef515f-bf89-4c2f-bbc0-5aad6041c98b" />
<img width="4032" height="3024" alt="LCD_Battery_temperature" src="https://github.com/user-attachments/assets/fec9a6b5-8239-4e1b-8489-8f808fec07ac" />



### Web Dashboard
ESP2 hosts a browser-based dashboard that shows:

- kitchen node readings
- valve node readings
- gas levels
- temperature
- humidity
- pressure
- valve status
- battery status
- system state
- communication status

<img width="1280" height="686" alt="Both_Dashoboard" src="https://github.com/user-attachments/assets/9ff24fb8-b14f-48f2-9b57-9183ecd0f38b" />



### Blynk
Blynk is used for mobile monitoring and quick remote access.

<img width="1280" height="720" alt="Blynk_app" src="https://github.com/user-attachments/assets/ae040a8e-b123-4c8b-94a9-fd5e05b2e108" />



### ThingSpeak
ThingSpeak is used for:
- cloud monitoring
- sensor charts
- trend analysis
- data logging

<img width="687" height="776" alt="ThingSpeak" src="https://github.com/user-attachments/assets/9218edc0-150a-4ce1-a191-6b9343ef1b73" />


---

## Configuration Storage

The system uses **LittleFS** to store:
- Wi-Fi SSID
- Wi-Fi password
- Blynk authentication token

### Why LittleFS is important
- settings remain saved after power off
- no need to hardcode credentials every time
- easier device reconfiguration
- better portability and maintenance

---

## Testing and Calibration

This project includes a significant amount of testing and calibration.

### Calibration includes:
- MQ-4 sensor calibration
- analog reading conversion
- battery percentage scaling
- pressure sensor tuning
- threshold refinement

<img width="4160" height="3033" alt="MQ4_reading_Graph" src="https://github.com/user-attachments/assets/057daed2-563c-4227-b0e1-dfd75a9efebc" />


### Testing includes:
- ESP-NOW transmission test
- sensor response test
- valve relay control test
- LCD display validation
- Blynk dashboard validation
- ThingSpeak chart validation
- web dashboard validation
- full system integration testing

Testing is essential because this is not just a display project; it is a functional safety system.


---
## Google Drive Folder Structure

All project media is stored in Google Drive under the main folder:

### Main Folder
**FYP Integrated Gas Safety And Environmental Monitoring System**  
https://drive.google.com/drive/folders/1qoRRjSMr3AxVFJfdwFxN7i4svPKSzAJ0?usp=sharing

### Subfolders

| # | Folder Name | Purpose | Link |
|---|---|---|---|
| 1 | 3D design | 3D design images | https://drive.google.com/drive/folders/1a-ou74HaFoO2cb0XY6JeKt5SdCkw9nBH?usp=drive_link |
| 2 | circuit Diagrams | Circuit diagram image and block diagram | https://drive.google.com/drive/folders/1okalMdNHIgR-oY-KRBrf3GfIIlL36GDo?usp=drive_link |
| 3 | ide codes file | Progress code and testing code | https://drive.google.com/drive/folders/1O6FngsWnx5p1srv9ZfYK_QDCpUeVaKr1?usp=drive_link |
| 4 | sensor readings | Calculations, MQ4 readings, graphs | https://drive.google.com/drive/folders/1Qk3edBc7zWAkIAOWEmBwDjeRP-G4ZLgm?usp=drive_link |
| 5 | testing | Testing videos and validation clips | https://drive.google.com/drive/folders/1f_4miGLQI4njrcyTFYYqaW-ftAAy9DdG?usp=drive_link |
| 6 | user interface | LCD, ThingSpeak, Blynk, dashboard screenshots | https://drive.google.com/drive/folders/1YT4PhAEFVFfvxUDPXqQrAAtMaPTvx_2Z?usp=drive_link |
| 7 | Videos | General, progress, and testing videos | https://drive.google.com/drive/folders/1DP63bxwnZB5VqDh7ueS1oHErNVnSuDIe?usp=drive_link |
| 8 | Receipts | Hardware purchase receipts | https://drive.google.com/drive/folders/11QaqAA37UPZ6ohal6bNkEqetNfZ9b-C4?usp=drive_link |

---

## Repository Folder Structure

A clean GitHub structure for this project is recommended below:

```bash
FYP-Integrated-Gas-Safety-and-Environmental-Monitoring-System/
├── README.md
├── code/
│   ├── esp1-kitchen-node/
│   ├── esp2-main-valve-node/
│   ├── progress-1/
│   ├── progress-2/
│   ├── progress-3/
│   ├── progress-4/
│   └── testing-code/
├── images/
│   ├── 3d-design/
│   ├── circuit-diagrams/
│   ├── sensor-readings/
│   ├── testing/
│   └── user-interface/
├── videos/
│   ├── general/
│   ├── progress/
│   └── testing/
├── receipts/
└── docs/
    └── project-notes/
