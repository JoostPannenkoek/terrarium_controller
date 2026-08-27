# ESP32 / MIDSPOT Prototype — Shopping List

## Development board
- [ ] **ESP32-S3-DevKitC-1-N8R8**
  - ESP32-S3
  - USB-C
  - Wi-Fi
  - Exposed GPIO
  - 1× required

## Breadboard
- [ ] **Joy-IT RB-Mount2-Set**
  - 830-point solderless breadboard
  - Breadboard mounting plate
  - 3.3 V / 5 V breadboard power supply
  - Jumper wires
  - Suitable for ESP32 prototyping
  - 1× required

## Logic / level shifting
- [ ] **74AHCT125**
  - Quad non-inverting buffer
  - 5 V supply
  - 3.3 V-compatible inputs
  - For experimental 3.3 V → 5 V PWM
  - 3–5× required

- [ ] **2N7000 N-channel MOSFET**
  - For open-drain/open-collector PWM experiments
  - 10× recommended

- [ ] **BSS138 N-channel MOSFET**
  - For alternative level-shifting experiments
  - 5× recommended

## Passive components
- [ ] **1/4 W resistor assortment**
  - Must include at least:
    - 100 Ω
    - 220 Ω
    - 330 Ω
    - 1 kΩ
    - 2.2 kΩ
    - 4.7 kΩ
    - 10 kΩ
    - 22 kΩ
    - 47 kΩ
    - 100 kΩ
  - 1× assortment

- [ ] **Ceramic capacitor assortment**
  - Must include:
    - 100 nF
    - 1 µF
    - 10 µF
  - 1× assortment

- [ ] **LED assortment**
  - 3/5 mm LEDs
  - For GPIO/PWM testing
  - 1× assortment

- [ ] **Potentiometers**
  - 10 kΩ: 2–5×
  - 100 kΩ: 2×

- [ ] **1N4148 signal diodes**
  - 10× recommended

- [ ] **1N5817 / 1N5819 Schottky diodes**
  - 5× recommended

## Power
- [ ] **12 V DC power supply**
  - 12 V output
  - ≥3 A
  - ≥36 W
  - For MIDSPOT
  - 1× required

- [ ] **Inline fuse holder**
  - Suitable for 12 V DC
  - 1–2× required

- [ ] **2 A and/or 3 A fuses**
  - 5× recommended

- [ ] **DC barrel jack → screw-terminal adapters**
  - Suitable for the chosen 12 V PSU
  - 2× recommended

## Wiring / connections
- [ ] **Male-to-male Dupont jumper wires**
  - Breadboard connections
  - 1× set

- [ ] **Male-to-female Dupont jumper wires**
  - ESP32/peripheral connections
  - 1× set

- [ ] **Female-to-female Dupont jumper wires**
  - Module-to-module connections
  - 1× set

- [ ] **2-pin screw terminals**
  - 5× recommended

- [ ] **3-pin screw terminals**
  - 5× recommended

## Optional test load
- [ ] **12 V LED strip / small 12 V lamp**
  - Low-cost sacrificial load
  - For testing MOSFET switching before connecting the MIDSPOT
  - 1× recommended

## Already owned
- [x] Multimeter
- [x] USB-C data cable
- [x] Oscilloscope
- [x] Logic analyzer

## Important
- The 12 V / 3 A supply powers the **MIDSPOT**, not through the breadboard power module.
- Keep the MIDSPOT's high-current 12 V path off the solderless breadboard.
- The PWM/control interface is currently **unknown**.
- The prototype should support testing:
  1. Direct 3.3 V PWM
  2. 5 V PWM via 74AHCT125
  3. Open-drain/open-collector PWM via 2N7000
- Do not connect PWM to the MIDSPOT until its connector pinout and control-input requirements have been established.
