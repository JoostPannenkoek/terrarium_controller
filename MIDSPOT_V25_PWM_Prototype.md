# Skylight MIDSPOT V25 PWM Prototype

## Purpose

This document describes a first breadboard/devboard prototype for controlling a Skylight MIDSPOT V25 terrarium lamp with PWM using a NUCLEO-F401RE.

The prototype is intended to reproduce the behavior of the compatible Skylight controller before designing a custom ESP32-based PCB.

> **Important:** The lamp current should **not** be routed through a solderless breadboard. Use the breadboard only for the Nucleo GPIO, gate resistor, and gate pull-down. Route the 12 V / lamp-current path through proper wire, screw terminals, and the MOSFET.

---

## Known Lamp / Supply Specifications

### Skylight MIDSPOT V25

Known electrical specifications:

- Supply voltage: **12 V DC**
- Rated current: approximately **1.2 A**
- Rated power: approximately **14 W**
- Brightness is controllable using the compatible controller.

### Original Power Supply

Label/specification:

- Input: **100–240 V AC**
- Frequency: **50/60 Hz**
- Output: **12.0 V DC**
- Plug: **5.5 mm × 2.1 mm DC barrel plug**

Measured polarity:

- **Center contact: +12 V**
- **Outer sleeve: GND / 0 V**

Measured unloaded/full-output voltage was approximately:

- **12.40 V**

Therefore the supply is:

**5.5 × 2.1 mm, center-positive**

---

## Connector

The original connector appears to be a standard coaxial DC barrel connector.

### Power supply side

Use:

- **5.5 × 2.1 mm female DC barrel jack to 2-pin screw terminal**

This allows the original power adapter to be connected without cutting its cable.

### Lamp side

Use:

- **5.5 × 2.1 mm male DC barrel plug to 2-pin screw terminal**

This allows the prototype to connect to the lamp using its original connector.

Because 5.5 × 2.1 mm and 5.5 × 2.5 mm connectors can look nearly identical, verify fit before purchasing many adapters.

---

## Compatible Controller Measurements

Measured DC voltage from the compatible controller:

| Brightness setting | Measured voltage |
|---:|---:|
| 10% | 4.13 V |
| 30% | 5.20 V |
| 40% | 6.40 V |
| 50% | 7.60 V |
| 60% | 8.80 V |
| 70% | 10.10 V |
| 90% | 12.10 V |
| 100% | 12.27 V |

Additional measurements:

- 10% brightness, lamp disconnected: **4.22 V**
- 100% brightness, lamp disconnected: **12.41 V**

These measurements are consistent with a controller that may be using PWM, although an oscilloscope measurement is still required to confirm waveform shape, frequency, and duty cycle.

---

# Prototype Architecture

The prototype uses **low-side N-channel MOSFET switching**.

The lamp receives +12 V continuously on its positive terminal.

The MOSFET rapidly connects and disconnects the lamp negative terminal to ground.

The NUCLEO-F401RE generates the PWM signal.

---

## Overall Wiring

```text
             ORIGINAL 12 V POWER SUPPLY
                5.5 × 2.1 mm plug
                         |
                         v
              Female barrel adapter
                 +             -
                 |             |
                 |             +-----------------------+
                 |                                     |
                 |                               NUCLEO GND
                 |                                     |
                 |                                     |
                 |                               MOSFET SOURCE
                 |                                     |
                 |                                  [ MOSFET ]
                 |                                     |
                 |                               MOSFET DRAIN
                 |                                     |
                 |                                     |
                 +------------------+                  |
                                    |                  |
                              Lamp center (+)     Lamp sleeve (-)
                                    |                  |
                                    +------- LAMP -----+
```

---

# MOSFET Gate Wiring

```text
NUCLEO-F401RE

PWM GPIO
   |
   |
 [100 ohm]
   |
   +---------------------- MOSFET Gate
   |
 [10 kohm]
   |
   |
  GND
```

The MOSFET source is connected to the same ground:

```text
12 V PSU GND
     |
     +---------------- NUCLEO GND
     |
     +---------------- MOSFET Source
```

This common ground is required so that the Nucleo's 3.3 V GPIO signal has the correct reference voltage relative to the MOSFET source.

---

# Full Electrical Schematic

```text
                          +12 V
                            |
                            |
                  +---------+---------+
                  |                   |
             PSU center          Lamp center
                                      |
                                      |
                                  +---+---+
                                  | LAMP  |
                                  +---+---+
                                      |
                                 Lamp sleeve
                                      |
                                      |
                                   DRAIN
                                     |
                              +------+------+
                              | N-MOSFET    |
                              +------+------+
                                     |
                                   SOURCE
                                     |
                                     +---------------- PSU GND
                                     |
                                     +---------------- NUCLEO GND


NUCLEO PWM GPIO
       |
     100 ohm
       |
       +----------------------------- GATE
       |
     10 kohm
       |
      GND
```

---

# Why the Resistors Are Needed

## 100 ohm gate resistor

Placed between the Nucleo GPIO and MOSFET gate.

Purpose:

- Limits instantaneous gate charge/discharge current.
- Reduces ringing and electrical noise.
- Protects the MCU GPIO from unnecessarily large transient currents.

Recommended value:

- **100 ohm**
- Values roughly between **47 and 220 ohm** would also normally work for this prototype.

## 10 kohm gate pull-down

Connected between MOSFET gate and ground.

Purpose:

- Keeps the MOSFET OFF while the MCU is booting.
- Prevents the gate from floating.
- Keeps the lamp OFF if the GPIO becomes high impedance.

Recommended value:

- **10 kohm**

---

# MOSFET Requirements

Use an **N-channel logic-level MOSFET**.

Minimum desired specifications:

- Gate drive compatible with **3.3 V**
- Drain-source voltage rating: at least **20 V**
- Preferably **30 V or more**
- Continuous current rating: comfortably above **1.2 A**
- Low `RDS(on)` specified at approximately **2.5 V or 3.3 V gate drive**
- Suitable for PWM switching

Recommended margin:

- Current capability: at least **3–5 A**
- Voltage capability: **30 V**
- Low on-resistance

Example suitable MOSFET family:

- **AO3400A**

Other similar 3.3 V logic-level MOSFETs are also acceptable.

Avoid MOSFET modules based on devices such as:

- **IRF520**

The IRF520 is not well suited to direct 3.3 V GPIO drive.

---

# NUCLEO-F401RE

The NUCLEO-F401RE uses **3.3 V GPIO logic**.

For the prototype:

- Power the Nucleo from USB.
- Do **not** connect the 12 V lamp supply to the Nucleo 3.3 V or 5 V power pins.
- Connect only the grounds together.

Required MCU connections:

- One PWM-capable GPIO
- One GND pin

A timer-capable GPIO should be selected so that STM32 hardware PWM can be used.

Initial experimental PWM settings:

- Frequency: approximately **1 kHz**
- Start duty cycle: approximately **10%**
- Increase gradually while testing

Once an oscilloscope is available, measure the original controller's:

- PWM frequency
- High voltage
- Low voltage
- Duty cycle at each brightness setting
- Whether switching is high-side or low-side

Then reproduce those values more accurately with the Nucleo.

---

# Power Wiring

The lamp consumes approximately **1.2 A**.

Do **NOT** route the lamp current through:

- Breadboard power rails
- Thin Dupont jumpers
- Nucleo headers

Instead use:

- Screw terminals
- Proper stranded hookup wire
- Barrel-to-screw-terminal adapters

Recommended wire:

- **0.5–0.75 mm²**
- Approximately **18–20 AWG**

---

# Fuse

Add an inline fuse in the +12 V path during development.

Recommended:

- **2 A fuse**
- Inline fuse holder

Example:

```text
PSU +12 V
   |
 [2 A fuse]
   |
   +---------------- Lamp +
```

This protects the prototype from accidental shorts.

---

# Optional Supply Decoupling

A capacitor may be placed near the MOSFET / lamp connection across the 12 V supply.

Recommended:

- **100–470 µF electrolytic**
- Rated for at least **25 V**

Also useful:

- **100 nF ceramic capacitor**

Connection:

```text
+12 V -----||----- GND
```

Observe electrolytic capacitor polarity.

---

# Breadboard Layout

Only the control circuitry needs to be on the breadboard.

```text
NUCLEO GPIO
    |
   100R
    |
    +------ MOSFET Gate
    |
   10k
    |
   GND
```

The following should stay off the solderless breadboard:

```text
12 V PSU +
12 V PSU GND high-current path
Lamp +
Lamp -
MOSFET drain/source high-current connections
```

If using a small MOSFET module with proper screw terminals, the MOSFET module itself can provide the high-current connection point.

---

# Shopping List

Order with 

| Owned | Item | Qty | Specification / Notes |
| --- |---|---:|---|
| 1 | Breadboard | 1 | 830-point solderless breadboard is sufficient |
| 1 | Male-male jumper wire set | 1 | For Nucleo GPIO/GND connections |
| 0 | PWM MOSFET module w. screw terminals | 2–5 | 3.3 V gate compatible, ≥30 V preferred, ≥3 A preferred. |
| 0 | 100 ohm resistors | 10 | MOSFET gate series resistor |
| 0 | 10 kohm resistors | 10 | MOSFET gate pull-down |
| 0 | 5.5 × 2.1 mm female barrel-to-screw adapter | 1 | PSU input |
| 0 | 5.5 × 2.1 mm male barrel-to-screw adapter | 1 | Lamp output |
| 0 | Inline fuse holder | 1 | For prototype protection |
| 0 | 2 A fuses | 5 | Spare fuses recommended |
| 1 | 18–20 AWG / 0.5–0.75 mm² wire | 1 set | Lamp-current wiring |
| 0 | 2-pin screw terminal blocks | Several | Useful for secure power wiring |

## Already Available

- NUCLEO-F401RE
- Original 12 V MIDSPOT power supply
- Skylight MIDSPOT V25 lamp
- Compatible Skylight controller
- Multimeter
- Oscilloscope access
- Logic analyzer access

---

# Pre-Power Checklist

Before connecting the lamp:

1. Verify the barrel adapter polarity with continuity mode while everything is unpowered.
2. Confirm:
   - center = +12 V
   - sleeve = ground
3. Verify the MOSFET pinout from its datasheet:
   - Gate
   - Drain
   - Source
4. Check that the MOSFET source connects to PSU ground.
5. Check that the lamp negative connects to MOSFET drain.
6. Check that the lamp positive connects directly to fused +12 V.
7. Check that Nucleo GND connects to PSU ground.
8. Check that the GPIO reaches the MOSFET gate through 100 ohm.
9. Check that a 10 kohm resistor connects gate to ground.
10. Confirm that no +12 V connection reaches any Nucleo GPIO or power pin.

---

# First Test Procedure

Before connecting the actual lamp:

1. Power the Nucleo from USB only.
2. Generate approximately 1 kHz PWM.
3. Measure the GPIO with a multimeter or oscilloscope.
4. Confirm approximately:
   - low = 0 V
   - high = 3.3 V
5. Connect the MOSFET control circuit.
6. Verify the MOSFET switches correctly.
7. Connect the 12 V supply.
8. Verify polarity again.
9. Start with a low PWM duty cycle.
10. Connect the lamp only after the above checks pass.

When oscilloscope access is available, first measure the original compatible controller and compare its waveform with the Nucleo-generated signal.

---

# Important Safety Notes

- Never connect 12 V directly to a Nucleo GPIO.
- Never connect 12 V directly to the Nucleo 3.3 V pin.
- Do not measure resistance or continuity on a powered circuit.
- Avoid accidental short circuits across the 12 V adapter.
- Do not carry the approximately 1.2 A lamp current through a solderless breadboard.
- Verify MOSFET pinout; pin order differs between packages and manufacturers.
- Begin testing at low duty cycle and inspect the MOSFET for excessive heating.

