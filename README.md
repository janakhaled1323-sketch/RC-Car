# RC-Car
First-Place Winner 🏆 | ESP32-based 4WD Robot Soccer Car developed during the Pixels Arduino &amp; Robotics program. Features wireless mobile control, L298N driver, and custom 3D-printed SolidWorks chassis. 🤖⚽
**First-Place Winning Project** 🏆
Developed as part of our comprehensive **60-hour Arduino and Robotics program**!

---

### 🛠️ Hardware Components & Features

* **Microcontroller:** ESP32 (handles wireless communication and core processing).
* **Motor Driver:** 1x L298N Motor Driver (controls DC motor direction and PWM speed).
* **Actuators:** 4x DC Motors configured for powerful 4WD movement.
* **Power System:** Three 3.7V 18650 Li-ion batteries.
* **Status Indicator:** Onboard LED (pin 2) indicating active movement vs. stop state.
* **Mechanical Design:** Custom 3D-modeled chassis designed and simulated using SolidWorks, manufactured using 3D printing (PLA).

---

### 💻 Code Explanation

The firmware is built in C++ using the Arduino IDE environment, utilizing the ESP32's native `BluetoothSerial` library for wireless control via a mobile application.

Here is a breakdown of how the code works:

1. **Libraries & Object Initialization:**
* Includes `BluetoothSerial.h` to initialize the Bluetooth serial communication object (`SerialBT`) with the device name `"Syber Beasts"`.


2. **Pin Configuration (`#define` & `setup()`):**
* **Speed Pins (PWM):** `speedMottorA` (Pin 13), `speedMottorB` (Pin 12), `speedMottorC` (Pin 14), and `speedMottorD` (Pin 27) are assigned to control motor speeds using analog PWM outputs.
* **Direction Pins:** Pins are mapped to the L298N driver to independently control the direction of the 4 DC motors (divided into right and left sides).
* **PinModes:** All motor control, speed, and LED pins are configured as `OUTPUT`, and motors are initialized to a stopped state (`Speed = 0`).


3. **Bluetooth Command Reception (`loop()`):**
* The program continuously checks for incoming Bluetooth data (`SerialBT.available()`).
* It captures control characters representing different movements (`'F'`, `'B'`, `'L'`, `'R'`, `'G'`, `'H'`, `'I'`, `'J'`, and `'S'`).


4. **Non-Blocking Execution & Motor Control (`switch-case`):**
* A non-blocking timer (`millis() - lastPrint >= 50`) executes the active movement command every 50 milliseconds to ensure smooth and stable robot responsiveness without blocking the CPU.
* **Forward (`'F'`) / Backward (`'B'`):** All 4 motors run simultaneously at a balanced speed (PWM = 128) with the status LED turned `HIGH`.
* **Turns (`'L'` / `'R'`):** Differential steering is applied by running motors on one side while stopping or reversing the other side.
* **Diagonal Movements (`'G'`, `'H'`, `'I'`, `'J'`):** Implements differential PWM speeds (e.g., 128 on one side and 64 on the other) to allow smooth curved or diagonal trajectories.
* **Stop (`'S'`):** Cuts power to all motor speed pins (PWM = 0), sets direction pins to `LOW`, and turns off the status LED.
