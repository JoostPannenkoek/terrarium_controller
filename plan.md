# Plan for the terrarium controller project

## Plan steps

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
