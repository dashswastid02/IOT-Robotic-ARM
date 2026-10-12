# IoT-Based Robotic Arm Using ESP32

A portfolio implementation of a Wi-Fi-controlled four-servo robotic arm. The ESP32 hosts a local web page so a phone or computer connected to its Wi-Fi access point can control the base, shoulder, elbow, and gripper.

> **Project provenance:** This is a new reference implementation based on the project title in the author's resume. It is not verified original source code from an earlier build. Test and adapt it to your actual hardware before claiming specific results.

## Features
- ESP32 access-point mode; no router or cloud account required
- Browser-based control interface
- Four servo channels: base, shoulder, elbow, gripper
- Angle limits enforced in firmware
- Responsive UI for phones and laptops
- No external web framework required

## Hardware
- ESP32 development board
- 3 standard hobby servo motors plus 1 gripper servo (choose servos suitable for your arm)
- Robotic arm frame and gripper
- External regulated servo supply sized for the servos
- Jumper wires and breadboard/terminal distribution
- USB cable for programming the ESP32

## Wiring (example)
| Function | ESP32 GPIO | Servo signal |
|---|---:|---|
| Base | 13 | Signal wire |
| Shoulder | 14 | Signal wire |
| Elbow | 25 | Signal wire |
| Gripper | 26 | Signal wire |

**Power safety**
1. Do **not** power the servos from the ESP32 3.3 V pin.
2. Use a separate regulated supply matching the servos' voltage and current requirements.
3. Connect the external supply ground to ESP32 GND (common ground).
4. Connect each servo's signal wire to the listed GPIO.
5. Check the servo's voltage/current requirements and the ESP32 board pinout before powering on.
6. Start with the arm unloaded and use conservative angle limits. Mechanical geometry varies; the defaults may need adjustment.

Typical servo wire colors vary by manufacturer; verify the datasheet rather than relying on color alone.

## Software setup
1. Install the Arduino IDE.
2. Add ESP32 board support using the official Espressif Arduino-ESP32 installation instructions.
3. In Arduino IDE, select your ESP32 board and its serial port.
4. Install the **ESP32Servo** library from Library Manager.
5. Open `firmware/IoT_Robotic_Arm_ESP32.ino`.
6. Upload it to the board.
7. Open Serial Monitor at **115200 baud**.
8. Connect your phone/laptop to Wi-Fi network `RoboticArm-ESP32` using password `armcontrol123`.
9. Open `http://192.168.4.1` in a browser.

Change the AP credentials near the top of the sketch before using it in a shared environment. This demo is intended for a trusted local network, not public internet exposure.

## How it works
1. The ESP32 starts a Wi-Fi access point.
2. The built-in HTTP server serves a single-page control UI.
3. Sliders send the selected servo angle to `/move`.
4. Firmware validates the servo index and angle, clamps to the configured range, then updates the servo.

## Customization
Edit the `SERVO_MIN` and `SERVO_MAX` arrays in the sketch to match your arm's safe range. If a servo moves in the opposite direction, reverse the mapping in the UI or adjust the firmware mapping. Never force a servo against a mechanical stop.

## Troubleshooting
- **Page won't load:** verify you're connected to `RoboticArm-ESP32`, then browse to `http://192.168.4.1`.
- **ESP32 resets when servos move:** the servo supply may be undersized or wiring may be poor. Use a suitable external supply and common ground.
- **Servo jitters:** check power quality, wiring, mechanical binding, and the servo library/pin compatibility.
- **One axis moves incorrectly:** verify GPIO wiring, servo index, and angle limits.
- **No Wi-Fi access point:** check Serial Monitor for startup messages and confirm the selected board/port.

## Suggested GitHub repository structure
```text
iot-robotic-arm/
├── README.md
├── LICENSE
├── .gitignore
├── firmware/
│   └── IoT_Robotic_Arm_ESP32.ino
├── docs/
│   └── wiring-and-testing.md
└── media/
    └── README.md
```

## Demo checklist
- [ ] Photograph the actual assembled hardware
- [ ] Capture a short video showing each axis moving
- [ ] Record the board and servo models
- [ ] Confirm safe angle ranges on your specific arm
- [ ] Add a wiring photo/diagram
- [ ] Document any changes from this reference implementation

## License
MIT; see `LICENSE`.
