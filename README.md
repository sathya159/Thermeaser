# ThermEaser: Smart Thermal Massage Belt

**ThermEaser** is an innovative, Bluetooth-controlled thermal massage belt designed to deliver both hot and cold massages based on customizable temperature settings. This project integrates electronic components, sensor-driven feedback, and user interface features to enhance pain relief and relaxation. The belt adjusts massage intensity and temperature based on real-time inputs, offering an efficient solution for physical therapy and comfort.

---

## Table of Contents
- [Project Overview](#project-overview)
- [Features](#features)
- [Components Used](#components-used)
- [3D Printed Design](#3d-printed-design)
- [Circuit Design](#circuit-design)
- [Software Implementation](#software-implementation)
- [User Guide](#user-guide)
- [Demonstration Video](#demonstration-video)
- [Experimental Results](#experimental-results)
- [Future Enhancements](#future-enhancements)
- [Lessons Learned](#lessons-learned)
- [Acknowledgments](#acknowledgments)

---

## Project Overview
The **ThermEaser** is designed for individuals suffering from muscle tension, offering relief through customizable massage functions. Utilizing an Arduino-based control system, ThermEaser operates with temperature sensors, actuators, and a Bluetooth application for remote control. Users can adjust the massage mode (hot or cold), set target temperatures, and control massage speed via a user-friendly smartphone interface.

## Features
- **Dual Mode Operation**: Provides both Hot and Cold massage modes.
- **User-Friendly Interface**: Bluetooth control via a smartphone app.
- **Adjustable Speed Control**: Users can fine-tune massage speed based on preferences.
- **Temperature Control**: Set desired temperature for hot massages; system auto-regulates heat.
- **Safety Features**: Includes over-temperature alarms and automatic shutoff to prevent overheating.
- **Real-time Feedback**: LCD displays current mode, status, and any safety alerts.

## Components Used
- **Microcontroller**: Arduino Uno for core processing and control.
- **Temperature Sensor**: TMP36 for accurate temperature measurement.
- **Actuators**: DC motor with cam mechanism for massage function.
- **Bluetooth Module**: HM10 for wireless communication with the smartphone app.
- **LCD Display**: Displays system status, mode, and safety alerts.
- **Buzzer**: Provides audible alerts in case of overheating.
- **LED Indicator**: Represents heat activation in the prototype.

For a full list of materials, refer to Appendix A in the project documentation.

## 3D Printed Design
To enhance the massaging effectiveness, we experimented with various designs and determined that a 3D printed part provided a superior massage sensation compared to other prototypes. The 3D printed structure allowed for more effective transmission of the massage force, improving user comfort and satisfaction. This design choice highlights the adaptability of ThermEaser in delivering optimized performance.

![3D Printed Part](images/3d_printed_part.png)  <!-- Update path with actual image location -->

## Circuit Design
The **ThermEaser** circuit integrates multiple sensors and actuators. Key connections:
- Temperature sensor connected to analog pin A5.
- Bluetooth communication through digital pins D2 and D3.
- Motor control on PWM pin D9 for variable speed.
- Overheating alarm using a buzzer at pin D8.
- LED for heating status on pin D13.

![Circuit Design](images/circuit_design.png) <!-- Update path with actual image location -->

Refer to the circuit schematic in the `docs` folder for detailed connections.

## Software Implementation
ThermEaser’s software comprises initialization, sensor monitoring, Bluetooth communication, and safety handling:
- **Initialization**: Sets up LCD, Bluetooth, and sensor communication.
- **Bluetooth Commands**: Parses commands for temperature adjustment, speed control, and mode selection.
- **Temperature Monitoring**: Reads sensor input and adjusts heat output based on user settings.
- **Safety Control**: Activates alarm and system shutoff if temperature exceeds safe limits.

### Flowchart
See the flowchart in Appendix C for a detailed view of the system’s software workflow.

## User Guide
1. **Connect to Bluetooth**: Open the ArduinoBlue app and connect to the device.
2. **Select Mode**: Choose between Cold and Hot massage modes.
3. **Adjust Settings**: Use sliders for speed and text input for temperature (Hot mode only).
4. **Monitor Alerts**: The system will display alerts on the LCD if safety thresholds are reached.

![User Guide](images/user_guide.png) <!-- Update path with actual image location -->

### App Interface
Here’s a snapshot of the Bluetooth app interface used to control ThermEaser:

![App Interface](images/app_interface.png) <!-- Update path with actual image location -->

## Demonstration Video
Watch a [video demonstration of ThermEaser in action](https://www.youtube.com/watch?v=-fOEsNSXCSk) to see how the device operates, including Bluetooth control, massage modes, and temperature safety features.

## Experimental Results
ThermEaser was rigorously tested to ensure functionality, reliability, and user safety:
- **Cold Massage**: Motor runs at user-selected speed; heat remains off.
- **Hot Massage**: Temperature sensor regulates heating based on target temperature.
- **Overheat Protection**: Buzzer activates and system pauses if temperature exceeds 105°F.

## Future Enhancements
- **Custom Bluetooth App**: A dedicated app for streamlined user interaction.
- **Higher Torque Motors**: Improved massaging effectiveness with higher torque.
- **Enhanced User Interface**: Separate speed control page based on massage mode.

## Lessons Learned
This project emphasized the importance of integrating hardware and software. Challenges included:
- **Bluetooth Communication**: Selecting an app compatible with all required features.
- **Temperature Regulation**: Ensuring safe operation without excessive temperature fluctuations.

## Acknowledgments
This project was completed as part of the ME 522 Mechatronics course. Special thanks to project contributors and course instructors.

---
