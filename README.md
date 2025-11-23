**RoboCleaner V1.0 — IoT House Cleaning Robot**

- **Description:**: Simple Arduino-based house-cleaning robot supporting autonomous obstacle avoidance and remote control via serial/Bluetooth. Uses three ultrasonic sensors, an IR sensor, ESC for brush/motor speed, and H-bridge motor control pins.

**Features**
- **Autonomous Mode:**: Obstacle detection using three ultrasonic sensors (left/front/right) and behavior logic to avoid collisions.
- **Manual/Remote Control:**: Accepts single-character serial commands (e.g., `F`, `B`, `L`, `R`) — can be used with USB serial or a Bluetooth-to-serial module. A companion Android APK `Bluetooth-RCcontroller.apk` is included in `software/`.
- **Motor Speed Control:**: ESC controlled via Servo library on `pin 13`, throttle read from analog `A0` and mapped to ESC microsecond range.

**Hardware (suggested components)**
- **MCU:** Arduino Uno (or compatible)
- **Ultrasonic sensors:** 3 x HC-SR04 (left, front, right)
- **IR sensor / Bump sensor:** 1 x digital IR or bump sensor
- **ESC + Brush/Motor:** Electronic Speed Controller connected to `pin 13`
- **Motor driver / H-bridge:** To drive left/right wheels using 4 digital outputs
- **Power source:** Battery pack (compatible with ESC and motors)

**Pinout / Wiring (as used in `RoboCleaner.ino`)**
- **Ultrasonic Left:**: `trig` = `2`, `echo` = `3`
- **Ultrasonic Front:**: `trig` = `4`, `echo` = `5`
- **Ultrasonic Right:**: `trig` = `6`, `echo` = `7`
- **IR / Bump sensor (digital):**: `8`
- **Motor control outputs (H-bridge):**: `9`, `10`, `11`, `12` (control wheel directions)
- **ESC signal (brush/motor speed):**: `13` (uses `Servo` library, writes microseconds)
- **Analog throttle input:**: `A0` (analogRead mapped to 1000–2000 µs for ESC)
- **Serial:** `Serial.begin(9600)` — telemetry and remote commands

**Behavior overview (logic summary)**
- The sketch measures distance from three ultrasonic sensors and reads the IR sensor.
- When IR sensor is HIGH the robot executes a predefined avoidance maneuver and toggles internal state `a`.
- Based on combinations of `distanceleft`, `distancefront`, `distanceright`, the robot sets the four motor pins to move forward, backward, turn left/right, or stop.
- Serial commands accepted (single ASCII characters):
  - `F` Forward
  - `B` Backward
  - `R` Right
  - `L` Left
  - `I`, `J`, `G`, `H` — diagonal or partial maneuvers (see sketch)

**Files of interest**
- `software/RoboCleaner.ino` : Main Arduino sketch (autonomy + serial control).
- `software/Bluetooth-RCcontroller.apk` : Android APK for Bluetooth remote control.
- `docs/Robo_Cleaner Final Report.docx` : Project report and additional documentation.

**How to upload / run**
- Open `software/RoboCleaner.ino` in the Arduino IDE.
- Select the correct board (e.g., `Arduino Uno`) and COM port.
- Click `Upload`.
- Open Serial Monitor at `9600` baud to view distance telemetry and send single-character commands for manual control.

Optional: using a Bluetooth module (e.g., HC-05)
- Connect the HC-05/HC-06 TX/RX to the Arduino RX/TX (pins `0` and `1`) or use a software serial port (if avoiding USB conflict).
- Pair the Android device and use `Bluetooth-RCcontroller.apk` to send commands; ensure the app sends characters matching the sketch commands.

**Safety & setup notes**
- Calibrate ESC as instructed by your ESC manual (the sketch already sends an initialization sequence in `setup()`).
- Ensure motor power (battery) is connected to ESC only after ESC initialization steps to avoid unexpected spinning.
- Use proper power isolation between Arduino (5V) and motor battery; common ground required.

**Further details**
- The `docs/Robo_Cleaner Final Report.docx` contains the project report, diagrams, and deeper explanations — open it for wiring diagrams and testing results.


