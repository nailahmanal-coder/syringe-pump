# Dual Syringe Infusion Pump using ESP32

## Overview

This project implements a **dual-channel syringe infusion pump** using an ESP32 microcontroller, A4988 stepper motor drivers, and NEMA 17 stepper motors. The system allows independent control of two syringe plungers via a simple web interface hosted on the ESP32.

The goal is to achieve **controlled fluid delivery (mL)** by converting motor steps into volume using calibration.

---

## Hardware Components

* ESP32 Development Board
* 2 × A4988 Stepper Driver Modules
* 2 × NEMA 17 Stepper Motors
* 20 mL Syringes (fat type)
* External 12V Power Supply (for motors)
* Breadboard and jumper wires

---

## System Architecture

* ESP32 hosts a **local web server**
* User inputs step values via browser
* ESP32 generates STEP/DIR signals
* A4988 drivers control motors
* Motors push/pull syringe plungers

---

## Features Implemented

* Independent control of **two motors**
* Web-based interface (no external app required)
* Forward and reverse motor control
* Step-based movement control
* Basic calibration for volume estimation

---

## Calibration

### Measured Data

* Full stroke (0 → 20 mL): ~13,000 steps

### Derived Constant

```
steps_per_ml = total_steps / total_volume
steps_per_ml = 13000 / 20 = 650 steps/mL
```

### Interpretation

* 650 steps ≈ 1 mL
* 1300 steps ≈ 2 mL
* 13000 steps ≈ 20 mL

---

## Control Logic

### Direction Control

* Positive steps → Forward (dispense fluid)
* Negative steps → Reverse (refill syringe)

### Motor Movement

Each step pulse:

* HIGH → LOW transition on STEP pin
* Controlled using microsecond delays

---

## Web Interface

The ESP32 serves a simple HTML page where users can:

* Enter number of steps for each motor
* Submit values to move motors

Access via:

```
http://<ESP32_IP_ADDRESS>
```

---

## Code Structure

### Key Functions

* `moveMotor()` → Handles direction and stepping
* `handleMove()` → Processes user input from web UI
* `setup()` → Initializes WiFi and server
* `loop()` → Handles incoming client requests

---

## Important Hardware Notes

* ENABLE pin on A4988 must be **LOW** (or connected to GND)
* RST and SLP must be connected together and tied to **3.3V**
* All grounds must be **common**:

  * ESP32 GND
  * Driver GND
  * Power supply GND
* VMOT must be powered with **12V**

---

## Limitations (Current)

* Uses step count instead of direct mL input
* Blocking motor control (sequential movement)
* No real-time flow rate control yet
* Calibration is approximate (can be improved)

---

## Future Improvements

* Input in **mL instead of steps**
* Flow rate control (mL/min)
* Non-blocking motor control
* Improved calibration accuracy
* Better UI (buttons instead of manual input)

---

## Summary

This project successfully demonstrates:

* Hardware interfacing (ESP32 + A4988 + stepper motors)
* Web-based control system
* Basic calibration for volume-based actuation

It forms a foundation for a **low-cost programmable infusion pump system**.
