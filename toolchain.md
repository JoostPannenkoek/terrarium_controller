## Technology Stack for terrarium controller project

### Embedded / ESP32

- **ESP32-S3-WROOM-1**
  - Wi-Fi/Bluetooth MCU module
  - https://www.espressif.com/en/products/modules/esp32-s3/esp32-s3-wroom-1

- **ESP-IDF**
  - Official Espressif development framework for the ESP32
  - https://docs.espressif.com/projects/esp-idf/en/latest/esp32s3/

- **FreeRTOS**
  - Real-time operating system used by ESP-IDF
  - https://docs.freertos.org/

- **ESP-IDF Wi-Fi**
  - ESP32 Wi-Fi networking stack
  - https://docs.espressif.com/projects/esp-idf/en/latest/esp32s3/api-guides/wifi.html

- **ESP-IDF MQTT**
  - MQTT client used by the ESP32
  - https://docs.espressif.com/projects/esp-idf/en/latest/esp32s3/api-reference/protocols/mqtt.html

- **ESP-IDF NVS**
  - Non-volatile configuration storage
  - https://docs.espressif.com/projects/esp-idf/en/latest/esp32s3/api-reference/storage/nvs_flash.html

- **ESP-IDF OTA**
  - Over-the-air firmware updates
  - https://docs.espressif.com/projects/esp-idf/en/latest/esp32s3/api-reference/system/ota.html

- **ESP-IDF USB**
  - Native USB support of the ESP32-S3
  - https://docs.espressif.com/projects/esp-idf/en/latest/esp32s3/api-reference/peripherals/usb_device.html

- **ESP-IDF GPIO**
  - GPIO control and interrupt handling
  - https://docs.espressif.com/projects/esp-idf/en/latest/esp32s3/api-reference/peripherals/gpio.html

- **ESP-IDF LEDC**
  - PWM generation for lamp/fan control
  - https://docs.espressif.com/projects/esp-idf/en/latest/esp32s3/api-reference/peripherals/ledc.html

- **ESP-IDF ADC**
  - Analog sensor inputs
  - https://docs.espressif.com/projects/esp-idf/en/latest/esp32s3/api-reference/peripherals/adc/index.html

- **ESP-IDF I²C**
  - I²C sensor communication
  - https://docs.espressif.com/projects/esp-idf/en/latest/esp32s3/api-reference/peripherals/i2c.html

- **ESP-IDF SPI**
  - SPI expansion/peripheral communication
  - https://docs.espressif.com/projects/esp-idf/en/latest/esp32s3/api-reference/peripherals/spi_master.html

- **ESP-IDF UART**
  - Serial communication and expansion
  - https://docs.espressif.com/projects/esp-idf/en/latest/esp32s3/api-reference/peripherals/uart.html

- **ESP-IDF Unity**
  - Embedded unit testing
  - https://docs.espressif.com/projects/esp-idf/en/latest/esp32s3/api-guides/unit-tests.html


### Hardware / PCB

- **KiCad**
  - Schematic capture, PCB layout and manufacturing files
  - https://www.kicad.org/

- **KiCad Documentation**
  - https://docs.kicad.org/

- **ESP32-S3-WROOM-1 Hardware**
  - Module datasheet and hardware design information
  - https://www.espressif.com/sites/default/files/documentation/esp32-s3-wroom-1_wroom-1u_datasheet_en.pdf

- **I²C**
  - Sensor bus
  - https://www.nxp.com/docs/en/user-guide/UM10204.pdf

- **1-Wire**
  - Digital sensor bus, primarily for DS18B20 sensors
  - https://www.analog.com/en/resources/technical-articles/1wire-communication-through-software.html

- **USB Type-C**
  - ESP32-S3 programming/debug interface
  - https://www.usb.org/usb-type-cr-cable-and-connector-specification


### Raspberry Pi

- **Raspberry Pi Zero W**
  - Main controller/server
  - https://www.raspberrypi.com/products/raspberry-pi-zero-w/

- **Raspberry Pi OS**
  - Operating system for the Raspberry Pi
  - https://www.raspberrypi.com/documentation/computers/os.html

- **systemd**
  - Service/process management on Raspberry Pi OS
  - https://systemd.io/

- **OpenSSH**
  - Remote administration of the Raspberry Pi
  - https://www.openssh.com/


### Backend

- **Python 3**
  - Backend programming language
  - https://docs.python.org/3/

- **FastAPI**
  - REST API and WebSocket backend framework
  - https://fastapi.tiangolo.com/

- **Uvicorn**
  - ASGI application server for FastAPI
  - https://www.uvicorn.org/

- **Pydantic**
  - Data validation and API models
  - https://docs.pydantic.dev/

- **Jinja2**
  - HTML templating engine
  - https://jinja.palletsprojects.com/


### Messaging / IoT

- **MQTT**
  - Communication protocol between Raspberry Pi and ESP32
  - https://mqtt.org/

- **Eclipse Mosquitto**
  - MQTT broker running on the Raspberry Pi
  - https://mosquitto.org/documentation/

- **MQTT.js**
  - Optional browser-side MQTT client if direct MQTT/WebSocket communication is later desired
  - https://mqttjs.com/


### Database

- **SQLite**
  - Local database for device configuration, schedules and sensor history
  - https://www.sqlite.org/docs.html

- **Python sqlite3**
  - Python interface to SQLite
  - https://docs.python.org/3/library/sqlite3.html


### Frontend

- **HTML5**
  - Web application structure
  - https://developer.mozilla.org/en-US/docs/Web/HTML

- **CSS**
  - Web application styling
  - https://developer.mozilla.org/en-US/docs/Web/CSS

- **JavaScript**
  - Browser-side application logic
  - https://developer.mozilla.org/en-US/docs/Web/JavaScript

- **WebSockets**
  - Real-time communication between browser and FastAPI
  - https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API

- **Fetch API**
  - Browser → REST API communication
  - https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API

- **Jinja2Templates**
  - FastAPI/Jinja2 integration
  - https://fastapi.tiangolo.com/reference/templating/


### API / Data Formats

- **REST**
  - Device and configuration API architecture
  - https://developer.mozilla.org/en-US/docs/Glossary/REST

- **OpenAPI**
  - Automatic API specification generated by FastAPI
  - https://www.openapis.org/

- **JSON**
  - Data interchange format between components
  - https://www.json.org/

- **JSON Schema**
  - Optional validation/schema definition for API/MQTT payloads
  - https://json-schema.org/


### Time / Scheduling

- **NTP**
  - Network time synchronization
  - https://www.rfc-editor.org/rfc/rfc5905

- **IANA Time Zone Database**
  - Time zone handling for schedules
  - https://www.iana.org/time-zones

- **Python zoneinfo**
  - Time-zone support in the backend
  - https://docs.python.org/3/library/zoneinfo.html


### Development / Build System

- **Git**
  - Version control
  - https://git-scm.com/doc

- **CMake**
  - ESP-IDF build system
  - https://cmake.org/documentation/

- **Ninja**
  - Build execution tool used by ESP-IDF
  - https://ninja-build.org/

- **Python virtual environments**
  - Isolated Python development environments
  - https://docs.python.org/3/library/venv.html

- **pip**
  - Python package management
  - https://pip.pypa.io/en/stable/


### Code Quality

- **clang-format**
  - C/C++ code formatting
  - https://clang.llvm.org/docs/ClangFormat.html

- **Ruff**
  - Python linting and formatting
  - https://docs.astral.sh/ruff/

- **pre-commit**
  - Automated Git hooks for code quality checks
  - https://pre-commit.com/


### Testing

- **ESP-IDF Unity**
  - Unit testing for ESP32 firmware
  - https://docs.espressif.com/projects/esp-idf/en/latest/esp32s3/api-guides/unit-tests.html

- **pytest**
  - Python unit/integration testing
  - https://docs.pytest.org/

- **FastAPI TestClient**
  - API testing
  - https://fastapi.tiangolo.com/tutorial/testing/


### Documentation

- **Markdown**
  - Project documentation
  - https://www.markdownguide.org/

- **Mermaid**
  - Optional diagrams directly inside Markdown
  - https://mermaid.js.org/

- **PlantUML**
  - Optional architecture/sequence diagrams
  - https://plantuml.com/


### Optional Future Integration

- **Home Assistant**
  - Optional home-automation integration through MQTT
  - https://www.home-assistant.io/
  - esphome

- **Home Assistant MQTT**
  - MQTT device integration
  - https://www.home-assistant.io/integrations/mqtt/
