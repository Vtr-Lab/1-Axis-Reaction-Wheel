# 1-Axis-Reaction-Wheel

An affordable, open-source reaction wheel balancing project inspired by spacecraft attitude control.

![Reaction Wheel Prototype](https://private-user-images.githubusercontent.com/236556372/655486252-a23e4a2b-94e7-4ec0-9525-de6a9dbe6ba6.png)

## The Perfect Weekend Project

This is the project I always wished I had when I first started tinkering and exploring the maker world in 9th grade.

Back then, I was always looking for a project that used accessible parts and straightforward assembly but still involved real engineering. I wanted something that went beyond simply following instructions — something that would help me understand the principles behind the machines and systems I was curious about.

This reaction wheel prototype is my attempt to build exactly that.

It demonstrates the same fundamental physical principle used by spacecraft for attitude control: **conservation of angular momentum**. By accelerating or decelerating an internal flywheel, the system generates torque that changes the orientation of the surrounding structure.

The project also introduces closed-loop control through a PID controller, a technique widely used in robotics, automation, and aerospace engineering.

Most importantly, I want this to be a project that other people can actually build, experiment with, and learn from.

Sophomore me finally gets to build something that freshman me dreamed about.

## About the Project

The goal is to create an affordable, one-axis reaction wheel that can balance along one axis of a cube-like frame.

### Key Features

- **Accessible hardware:** Uses commonly available electronic components and 3D-printed mechanical parts.
- **Reaction wheel physics:** Demonstrates how changing flywheel momentum produces reaction torque.
- **Closed-loop control:** Uses an ESP32 and MPU-6050 IMU to measure orientation and support PID-based balancing.
- **Low-cost construction:** Designed around inexpensive parts, simple fabrication, and accessible tools.
- **Hands-on learning:** Provides an opportunity to experiment with sensor measurements, motor control, feedback systems, and PID tuning.

### How It Works

The MPU-6050 measures the frame's motion. The ESP32 processes the sensor readings and uses a PID controller to calculate a motor command. The motor accelerates or decelerates the reaction wheel, generating a reaction torque on the frame.

The controller continuously repeats this process to counter disturbances and bring the frame toward its target angle.

This project demonstrates **one-axis attitude control**, rather than full three-axis spacecraft control.

## Hardware Overview

The current prototype uses:

- ESP32 DevKit
- MPU-6050 inertial measurement unit (IMU)
- DC TT motor
- TB6612FNG motor driver
- LM2596 buck converter
- LiPo battery
- Custom 3D-printed structural parts
- Bearings, fasteners, and counterweights

The frame and mechanical components are designed for fabrication with a consumer 3D printer. The current parts were printed in PETG using a Bambu Lab A1.

## Bill of Materials (BOM)

Prices are listed in New Taiwan dollars (NTD) and reflect the costs recorded for this prototype. Prices may vary by supplier.

| Item | Quantity | Price (NTD) |
|---|---:|---:|
| MPU-6050 | 1 | 53 |
| ESP32 DevKit | 1 | 143 |
| DC TT motor | 1 | 50 |
| TB6612FNG motor driver | 1 | 22 |
| LM2596 buck converter | 1 | 50 |
| LiPo battery | 1 | 210 |
| Perfboard (5 × 7 cm) | 1 | 15 |
| M4 × 50 mm screws | 4 | 4 |
| M4 nuts | 4 | 4 |
| M3 × 9 mm screws | 2 | 2 |
| M3 nuts | 2 | 2 |
| Zip ties | 3 | 3 |
| Counterweight nuts | 10 | 20 |
| Counterweight bolts | 10 | 20 |
| Bearing (24 mm OD × 8 mm ID × 8 mm thick) | 1 | 20 |
| XH2.54 3-pin connector | 1 | 4 |
| 3D-printed parts | 5 | 0 |
| **Total recorded cost** | | **NT$610** |

The recorded total is NT$610. This is the parts cost for the listed prototype, not necessarily the full cost of tools, shipping, spare parts, or future revisions.

## Product Overview

The current design is a **one-axis reaction wheel** that aims to balance along one side of a cube-like frame.

The project is intended as a learning platform as much as a working prototype. Its main goal is to make the underlying physics and control concepts understandable through a real, buildable system.

## Build Instructions

### Step 1: Assembly and Wiring

Follow the [Assembly Instructions](https://docs.google.com/document/d/1aEVjV9toqsNxJIusX6yg7DXPMAjhrYVz725-x2guhmM/edit?usp=sharing) for the step-by-step assembly and wiring process.

The guide covers the physical construction and electronics setup required to put the prototype together.

### Step 2: Firmware

The ESP32 firmware handles sensor readings, orientation estimation, motor commands, and the balancing control loop.

The code and setup instructions will be documented here as the firmware is finalized.

Planned documentation:

- Arduino IDE and ESP32 setup
- MPU-6050 configuration and sensor calibration
- Motor control and direction
- PID implementation
- Firmware upload and troubleshooting

### Step 3: PID Tuning

PID tuning determines how aggressively the system responds to tilt and how well it settles toward its target angle.

The tuning guide will cover:

- Understanding proportional, integral, and derivative gains
- Establishing a safe starting point
- Adjusting gains systematically
- Recognizing oscillation, overshoot, and slow correction
- Recording and comparing tuning results

The objective is not just to obtain a balancing system, but to understand why different controller settings produce different behavior.

## Demo

A demonstration video will be added here once the prototype's performance has been recorded.

## Future Development

Potential improvements include:

- Wireless PID tuning from a phone
- Real-time sensor and controller data visualization
- More detailed build documentation
- Improved mechanical and electrical integration
- Expansion toward multi-axis reaction wheel systems

## Contributing

Suggestions, improvements, and discussions are welcome. If you build your own version, experiment with the design, or find a way to improve the hardware or control algorithm, feel free to share your results.

The goal is to make reaction wheel control more approachable for students, makers, and anyone interested in engineering.

## Follow the Project

Follow my engineering projects on Instagram: **[@vtr_lab](https://www.instagram.com/vtr_lab/)**

Thanks for checking out the project.
