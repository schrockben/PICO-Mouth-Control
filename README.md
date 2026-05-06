# PICO-Mouth-Control
This is a mouth control video gamepad device
# PicoMouth Switch Controller

An adaptive, mouth-operated HID controller for the Nintendo Switch built on the Raspberry Pi Pico (RP2040). This project uses high-precision pressure sensors and high-speed logic to provide a low-latency gaming experience for individuals requiring alternative input methods.

## Technical Requirements

### Required Libraries
The following libraries and board packages must be installed before compiling the code:

1.  **arduino-pico Board Package:** * Open Arduino IDE -> File -> Preferences.
    * Add this URL to **Additional Boards Manager URLs**: 
        `https://github.com/earlephilhower/arduino-pico/releases/download/global/package_rp2040_index.json`
    * Go to Tools -> Board -> Boards Manager, search for **Raspberry Pi Pico/RP2040**, and install.
2.  **Adafruit TinyUSB Library:** Search and install via the Library Manager.
3.  **Adafruit ADS1X15:** Search and install via the Library Manager.
4.  **switch_tinyusb:** * Download the ZIP from [github.com/touchgadget/switch_tinyusb](https://github.com/touchgadget/switch_tinyusb).
    * Install via **Sketch -> Include Library -> Add .ZIP Library**.

### Critical IDE Settings
For the code to function as a Nintendo Switch controller, the following settings under the **Tools** menu are mandatory:

* **Board:** "Raspberry Pi Pico"
* **USB Stack:** "Adafruit TinyUSB"

---

## Hardware Configuration

| Component | Pico Pin (GPIO) |
| :--- | :--- |
| **HX710 SCK (Shared)** | GP15 |
| **Sensor 0 Data** | GP12 |
| **Sensor 1 Data** | GP13 |
| **Sensor 2 Data** | GP14 |
| **Joystick X-Axis** | GP26 (A0) |
| **Joystick Y-Axis** | GP27 (A1) |
| **Joystick Click (SW)** | GP16 |

---

## Nintendo Switch Button Mapping

The controller uses timed input logic to expand functionality across three pressure sensors.

| Input | Action Type | Switch Button | Function |
| :--- | :--- | :--- | :--- |
| **S0 Puff** | Quick Tap | **X** | Menu / Action |
| **S0 Puff** | Hold (> 3s) | **Home** | System Home (One-Shot Pulse) |
| **S0 Sip** | Quick Tap | **Y** | Secondary Action |
| **S0 Sip** | Hold (> 3s) | **Plus (+)** | Start / Pause (One-Shot Pulse) |
| **S1 Puff** | Any | **A** | Confirm / Accelerate |
| **S1 Sip** | Any | **B** | Back / Brake |
| **S2 Puff** | Any | **R + ZR** | Right Shoulder/Trigger |
| **S2 Sip** | Any | **L + ZL** | Left Shoulder/Trigger |
| **Stick Click** | Toggle | **R + ZR** | Latch Triggers (Hold/Release) |

---

## Key Features

### 1. One-Shot Pulse Logic
To prevent the Nintendo Switch from interpreting long-held breaths as a "Long Press" (which often triggers side menus or power options), the **Home** and **Plus** buttons use one-shot pulse logic. Once the 3-second threshold is met, the command is sent exactly once; you must release your breath and start a new action to trigger the button again.

### 2. Adaptive Baseline Tracking
The controller automatically adjusts to environmental changes and sensor drift. It constantly "drifts" the baseline to match the current rest state. This allows for interchangeable mouthpieces or sensors with different rest-voltages (e.g., 400k vs 2.1M) without requiring a software re-flash.

### 3. Stuck Sensor Recovery
If a mouthpiece is hot-swapped while the device is powered on, the baseline may shift drastically, causing "ghost" button presses. If a sensor remains in an active state for more than 5 seconds without fluctuation, the controller assumes a hardware shift has occurred and forces an immediate re-calibration of that specific sensor.

### 4. Trigger Latching
Clicking the joystick toggles a persistent hold on **R + ZR**. This is specifically designed for racing games or titles requiring constant trigger pressure, allowing the user to focus on steering and item management without maintained breath pressure.
