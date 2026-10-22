# 1-Axis Reaction Wheel

A low-cost, open-source reaction wheel that brings satellite attitude-control principles into a hands-on engineering project.

<!-- Replace this path with an image committed to the repository. -->
![1-Axis Reaction Wheel Prototype](images/prototype.png)

## Table of Contents

- [About the Project](#about-the-project)
- [How It Works](#how-it-works)
- [Hardware](#hardware)
- [Bill of Materials](#bill-of-materials)
- [Mechanical Design](#mechanical-design)
- [Assembly and Wiring](#assembly-and-wiring)
- [Firmware and Control](#firmware-and-control)
- [PID Tuning](#pid-tuning)
- [Demo](#demo)
- [Future Development](#future-development)
- [Follow the Project](#follow-the-project)

---

## About the Project

This is the project I always wished I had when I first started tinkering and exploring the maker world in ninth grade.

Back then, I was looking for something that could be built with accessible components and straightforward assembly, while still teaching real engineering concepts. I wanted a project that went beyond simply following instructions and gave me the opportunity to experiment with mechanical design, embedded systems, sensors, and control theory.

This reaction wheel is my attempt to build exactly that.

It uses the same fundamental physical principle employed by satellites to control their orientation: **conservation of angular momentum**. An internal flywheel accelerates or decelerates to generate a reaction torque, allowing the structure to counteract changes in its orientation.

The project also incorporates PID control, a technique used throughout robotics, automation, and control engineering.

Sophomore me finally gets to build what freshman me dreamed about.

## How It Works

The system combines three main elements:

1. **Orientation sensing:** An MPU-6050 measures angular velocity and acceleration to estimate the system's tilt.
2. **Feedback control:** An ESP32 processes the sensor readings and calculates the correction required to reduce the tilt error.
3. **Reaction wheel actuation:** A motor spins the flywheel to generate torque in the opposite direction, helping the system recover its balance.

The controller continuously repeats this process to correct disturbances and maintain the target orientation.

## Hardware

The current design uses a dedicated motor with an integrated driver and encoder feedback, controlled by a custom ESP32-based PCB.

| Component | Purpose |
|---|---|
| ESP32 DevKit V1 | Main controller and wireless communication |
| MPU-6050 IMU | Measures acceleration and angular velocity |
| Nidec 24H055M020 motor | Drives the reaction wheel; includes an integrated motor driver |
| Custom PCB | Connects the controller, sensor, motor-control signals, and power circuitry |
| Mini560 5 V / 5 A buck converter | Steps down the 3S battery voltage to power the ESP32 |
| 3S LiPo battery | Supplies the motor and system power |
| 330 µF, 25 V capacitor | Helps stabilize the battery input and absorb supply transients |
| Resistors | Signal conditioning, including the motor feedback voltage divider |
| 2N3904 NPN transistor | Switches the motor's active-low brake input |
| Connectors and wiring | Connect the battery, motor, and other hardware |
| Bearings, fasteners, and counterweights | Support the rotating assembly and provide the required flywheel mass |
| 3D-printed structural parts | Form the frame, motor mount, and mechanical assembly |

### Motor and Control Interface

The Nidec motor is different from a conventional bare DC motor: it has an integrated driver and accepts control signals rather than connecting directly to a typical H-bridge.

The ESP32 interfaces with the motor through:

- **PFM:** Controls the motor command through pulse-frequency modulation.
- **Direction:** Selects the direction of rotation.
- **Brake:** Controls the motor's braking input.
- **FG feedback:** Provides a motor feedback signal for monitoring rotation.
- **Enable:** Enables the motor through the motor's specified power connection.

The custom PCB also includes signal-conditioning circuitry to interface the motor feedback with the ESP32's 3.3 V GPIO.

## Bill of Materials

The following list describes the current PCB-based design. Prices should be filled in using the actual purchase costs for this revision rather than carried over from the earlier TT motor prototype.

| Item | Quantity | Notes |
|---|---:|---|
| ESP32 DevKit V1 | 1 | Main controller |
| MPU-6050 module | 1 | IMU |
| Nidec 24H055M020 | 1 | Integrated-driver motor |
| Mini560 5 V / 5 A buck converter | 1 | Battery-to-5 V conversion |
| 3S LiPo battery | 1 | 11.1 V nominal, 12.6 V fully charged |
| Custom PCB | 1 | Controller, power, and signal interface |
| 330 µF, 25 V capacitor | 1 | Battery input capacitor |
| Resistors | As required | Feedback divider and transistor circuitry |
| 2N3904 NPN transistor | 1 | Brake switching |
| Motor and battery connectors | As required | Match the PCB footprints and wiring |
| Wires and solder | As required | Electrical assembly |
| Bearings | As required | Flywheel support |
| Fasteners and counterweights | As required | Mechanical assembly |
| 3D-printed parts | As required | Frame and mounting components |

**Note:** This list reflects the newer motor and PCB architecture. It does not include the old TB6612FNG driver, TT motor, or LM2596 module from the original prototype.

## Mechanical Design

The reaction wheel assembly consists of a structural frame, motor mount, rotating flywheel, bearing support, and counterweights.

The mechanical design needs to balance several considerations:

- Sufficient flywheel inertia to generate useful reaction torque.
- Secure motor and bearing mounting.
- Low friction in the rotating assembly.
- Reliable retention of the counterweights at operating speed.
- Enough clearance to prevent the flywheel from contacting the frame.

The structural parts are designed for 3D printing, making the assembly accessible to makers with desktop printers.

## Assembly and Wiring

### Step 1 — Build the Mechanical Assembly

Print the structural components, install the bearings and motor, and assemble the flywheel and counterweights.

Check that the wheel rotates freely and that all rotating hardware is secured before powering the motor.

### Step 2 — Assemble the Electronics

Install the ESP32, IMU, buck converter, and supporting components. Connect the motor's power and control interfaces to the appropriate PCB connections.

The motor supply, controller supply, and sensor circuitry must be wired according to the schematic. Keep high-current motor return paths away from sensitive sensor signal routing, while maintaining the required common electrical reference.

### Step 3 — Connect the Battery

Use a compatible 3S LiPo battery and verify the power connections, polarity, and regulated 5 V output before connecting the controller.

Do not connect or disconnect the battery while the circuit is powered, and keep the flywheel clear of hands and loose objects during testing.

### Detailed Assembly Guide

Follow the assembly document for the detailed mechanical and electrical build process:

[Open the Assembly Instructions](https://docs.google.com/document/d/1aEVjV9toqsNxJIusX6yg7DXPMAjhrYVz725-x2guhmM/edit?usp=sharing)

## Firmware and Control

The ESP32 runs the sensor-processing and control logic.

The firmware is intended to handle:

- IMU initialization and orientation estimation.
- Motor command generation.
- Direction and braking control.
- Feedback monitoring.
- PID control and parameter adjustment.
- Wireless communication for future tuning and visualization features.

**Firmware:** Add the current, tested Arduino sketch and its required library dependencies to the repository before treating this section as a complete setup guide.

## PID Tuning

PID control combines three terms to calculate the correction applied to the system.

- **Proportional (P):** Responds to the current orientation error.
- **Integral (I):** Accumulates error over time to correct persistent offsets.
- **Derivative (D):** Responds to the rate of change of error to help reduce oscillation.

A practical tuning process is to begin with conservative settings, increase proportional gain until the system responds meaningfully, and then adjust derivative gain to manage oscillation. Add integral gain only when persistent error requires it.

Start with the flywheel securely mounted, a clear test area, and conservative motor commands. Tune one parameter at a time and record the results.

## Demo

Add a video showing the completed assembly, startup procedure, and balancing performance.

- **Build video:** To be added.
- **Balancing demonstration:** To be added.
- **Control and tuning demonstration:** To be added.

## Future Development

Planned improvements include:

- A phone-friendly interface for wireless control.
- Live IMU and motor feedback plots.
- Adjustable PID parameters without reflashing the firmware.
- A simple 2D visualization of the reaction wheel's orientation.
- More detailed build documentation and troubleshooting guides.
- Further experiments with reaction wheel sizing, control response, and system stability.

The long-term goal is to make the project useful not only as a working prototype, but also as a teaching platform for mechanical engineering, embedded systems, and control theory.

## Contributing

Suggestions, improvements, and documented experiments are welcome. If you build your own version, consider sharing your hardware changes, tuning results, and lessons learned so others can reproduce and improve the design.

## Follow the Project

Follow along with the build process and future updates on Instagram:

**Instagram:** [@vtr_lab](https://www.instagram.com/vtr_lab/)

Thanks for checking out the project!
