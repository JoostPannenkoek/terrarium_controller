# Plan for the terrarium controller project

## Project milestones

```
                    PROJECT START
                         │
                         ▼
              ┌────────────────────┐
              │ 1. AQCT-1          │
              │ reverse engineering│
              └──────────┬─────────┘
                         │
                         ▼
              ┌────────────────────┐
              │ 2. ESP32 prototype │
              │ lamp control       │
              └──────────┬─────────┘
                         │
                         ▼
              ┌────────────────────┐
              │ 3. Sensor prototype│
              │ I²C / 1-Wire / ADC │
              └──────────┬─────────┘
                         │
                         ▼
              ┌────────────────────┐
              │ 4. MQTT             │
              │ ESP32 ↔ Pi          │
              └──────────┬─────────┘
                         │
                         ▼
              ┌────────────────────┐
              │ 5. Pi backend       │
              │ FastAPI + SQLite    │
              └──────────┬─────────┘
                         │
                         ▼
              ┌────────────────────┐
              │ 6. Web interface    │
              └──────────┬─────────┘
                         │
                         ▼
              ┌────────────────────┐
              │ 7. PCB v0.1         │
              │ complete hardware   │
              └──────────┬─────────┘
                         │
                         ▼
              ┌────────────────────┐
              │ 8. Bring-up         │
              └──────────┬─────────┘
                         │
                         ▼
              ┌────────────────────┐
              │ 9. OTA + reliability│
              └──────────┬─────────┘
                         │
                         ▼
                 PRODUCTION v1.0
                         │
                         ▼
              ┌────────────────────┐
              │ Future expansion    │
              │ sensors / fans /    │
              │ pumps / automation  │
              └────────────────────┘
```

## M0 — AQCT-1 Reverse Engineering

Determine the exact electrical interface of the Skylight MIDSPOT V25, including power requirements, connector pinout, dimming method, voltage levels, frequency, and timing. Use an oscilloscope and/or logic analyzer to observe the original controller and document the interface. The result should be a verified specification that can be safely implemented by the ESP32.
Dimming method is to be reverse-engineerd using the AQCT-1 skylight-compatible dimmer (15 euro)

## M1 — ESP32 MIDSPOT Control

Build an initial ESP32 development-board prototype with the required level-shifting and driver circuitry. Implement basic lamp control, including on/off, brightness control, and smooth brightness transitions. Verify the generated control signal and confirm reliable operation of the lamp.

## M2 — Sensor Prototype

Connect and test the initial sensor set, such as an SHT41, DS18B20, and BH1750. Implement a generic sensor abstraction so that different sensor types can be added without changing the rest of the application. Test normal operation as well as disconnected and malfunctioning sensors.

## M3 — MQTT Communication

Set up Mosquitto on the Raspberry Pi and implement MQTT communication between the ESP32 and Raspberry Pi. Define the MQTT topic structure and JSON message formats for commands, device state, sensor data, and status information. Ensure that Wi-Fi and MQTT disconnections are handled automatically and do not interrupt autonomous operation.

## M4 — FastAPI Backend

Develop the Raspberry Pi backend using Python, FastAPI, Uvicorn, Pydantic, and SQLite. Implement device management, sensor data storage, lamp/actuator control, configuration, and scheduling APIs. Connect the backend to the ESP32 through MQTT.

## M5 — Web Interface

Create the initial browser-based dashboard using HTML, CSS, JavaScript, and Jinja2. Provide controls for the lamp and other actuators, display current sensor readings and device status, and use WebSockets for live updates. Add schedule configuration and historical sensor graphs once the basic interface is functional.

## M6 — PCB v0.1

Design the first custom controller PCB in KiCad using the validated hardware from the previous milestones. Include the ESP32, power supplies, MIDSPOT interface, sensor buses, ADC inputs, fan/AUX outputs, USB, debugging, and expansion connectors. Manufacture the PCB and prepare it for systematic bring-up.

## M7 — PCB Bring-Up

Validate the manufactured PCB subsystem by subsystem, starting with visual inspection and power rails before connecting the ESP32 and external loads. Test USB, ESP32 boot, Wi-Fi, sensor interfaces, ADCs, communication interfaces, and MOSFET outputs independently. Finally connect and test the MIDSPOT using the previously validated control interface.

## M8 — Full System Integration

Run the complete system on the custom PCB, combining the ESP32 firmware, sensors, MIDSPOT, Raspberry Pi backend, MQTT communication, database, and web interface. Verify that commands and sensor data flow correctly between the browser, Pi, ESP32, and hardware. Test the system as a complete terrarium controller under normal operating conditions.

## M9 — OTA and Reliability

Implement OTA firmware updates, watchdogs, automatic Wi-Fi/MQTT reconnection, sensor error handling, brownout recovery, and safe actuator states. The ESP32 should continue operating its local schedule when the Raspberry Pi or network is unavailable. Test failure scenarios deliberately to verify automatic recovery.

## M10 — Production v1.0

Freeze the hardware design, firmware interfaces, MQTT protocol, database schema, and web application. Produce final documentation covering installation, wiring, configuration, troubleshooting, firmware updates, and hardware pinouts. Tag the first stable hardware, firmware, and server releases as v1.0.

## M11 — Future Sensors and Automation

Expand the controller with additional sensors and actuators such as soil moisture, CO₂, UV, water level, pumps, misting systems, fans, and additional lighting. Add a configurable automation/rules engine that can react to sensor values and schedules. Optionally integrate the system with Home Assistant through MQTT.

## Shopping list

## Project Toolchain Summary

### Hardware

KiCad
ESP32-S3-WROOM-1
USB-C
I²C
1-Wire
SPI
UART
ADC
PWM
MOSFET drivers

### ESP32 Firmware

ESP-IDF
FreeRTOS
C/C++
CMake
Ninja
ESP-IDF MQTT
ESP-IDF Wi-Fi
ESP-IDF NVS
ESP-IDF OTA

### Raspberry Pi

Raspberry Pi Zero W
Raspberry Pi OS Lite
systemd
OpenSSH
Mosquitto

### Backend

Python 3
FastAPI
Uvicorn
Pydantic
Jinja2
SQLite

### Frontend

HTML5
CSS
JavaScript
Fetch API
WebSockets

### Development

Git
clang-format
Ruff
pre-commit
pytest
ESP-IDF Unity
Markdown
Mermaid / PlantUML

### Communication

Wi-Fi
MQTT
REST
WebSockets
JSON

### Optional

Home Assistant


## General project schematic
```
                              ┌──────────────────────┐
                              │       Browser        │
                              │                      │
                              │ Dashboard            │
                              │ Schedules            │
                              │ Sensors              │
                              │ Configuration        │
                              └──────────┬───────────┘
                                         │
                                    HTTP / WebSocket
                                         │
                                         ▼
┌───────────────────────────────────────────────────────────────────┐
│                       Raspberry Pi Zero W                         │
│                                                                   │
│  ┌────────────────┐       ┌───────────────┐                      │
│  │ Web Frontend   │◄─────►│ FastAPI       │                      │
│  │ HTML/CSS/JS    │       │ REST API      │                      │
│  └────────────────┘       │ WebSocket     │                      │
│                           └───────┬───────┘                      │
│                                   │                              │
│                           ┌───────▼───────┐                      │
│                           │    SQLite     │                      │
│                           └───────────────┘                      │
│                                   │                              │
│                           ┌───────▼───────┐                      │
│                           │    MQTT       │                      │
│                           │   Mosquitto   │                      │
│                           └───────┬───────┘                      │
└───────────────────────────────────┼───────────────────────────────┘
                                    │
                                  Wi-Fi
                                    │
                                    ▼
┌───────────────────────────────────────────────────────────────────┐
│                        ESP32 Controller (ESP-IDF)                 │
│                                                                   │
│  ┌─────────────┐       ┌────────────────┐                         │
│  │ ESP32-S3    │──────►│ Device Control │                         │
│  │             │       │                │                         │
│  │ Wi-Fi       │       │ Lamp           │                         │
│  │ FreeRTOS    │       │ Fans           │                         │ 
│  │ GPIO        │       │ Aux outputs    │                         │
│  └──────┬──────┘       └────────────────┘                         │
│         │                                                         │
│         ├── I²C ─────── Temperature / humidity / light sensors    │
│         ├── 1-Wire ──── DS18B20 sensors                           │
│         ├── ADC ─────── Analog sensors                            │
│         ├── SPI ─────── Future expansion                          │
│         └── UART ────── Future expansion / debugging              │ 
└───────────────────────────────────────────────────────────────────┘
```
## PCB draft schematic
```
                         TERRARIUM CONTROLLER
┌──────────────────────────────────────────────────────────────────────────┐
│                                                                          │
│                            ┌──────────────┐                              │
│                            │ ESP32-S3     │                              │
│                            │ WROOM-1      │                              │
│                            └──────┬───────┘                              │
│                                   │                                      │
│       ┌───────────────────────────┼─────────────────────────────┐        │
│       │                           │                             │        │
│       ▼                           ▼                             ▼        │
│  ┌───────────┐             ┌────────────┐                ┌───────────┐   │
│  │ MIDSPOT   │             │ SENSOR BUS │                │ EXPANSION │   │
│  │ INTERFACE │             │            │                │           │   │ 
│  └─────┬─────┘             └─────┬──────┘                └─────┬─────┘   │
│        │                         │                             │         │
│        ▼                         ▼                             ▼         │ 
│     12V LAMP               I²C / 1-Wire                  GPIO/ADC/SPI    │
│                                                                          │
│  ┌──────────────────────────────────────────────────────────────────┐    │
│  │                         POWER SUPPLY                             │    │
│  │                                                                  │    │
│  │  12V IN → Fuse → Reverse polarity → TVS → 12V rail               │    │ 
│  │                                      │                           │    │
│  │                                      └→ Buck → 5V → 3.3V → ESP32 │    │
│  └──────────────────────────────────────────────────────────────────┘    │
│                                                                          │
│  USB ─────── ESP32 programming/debug                                     │
│  UART ────── expansion/debug                                             │
│  STATUS ──── LEDs                                                        │
│  USER ────── button                                                      │
│                                                                          │
└──────────────────────────────────────────────────────────────────────────┘
```

## Controller board Firmware schematic

## Web UI software schematic
