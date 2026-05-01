# Hardware

## Hardware Overview

This section describes the hardware components used in the small-scale mushroom media mixer prototype, as well as how they are integrated into the overall system. The hardware was selected based on cost, availability, and compatibility with the project constraints, with an emphasis on simplicity and reliability for small-scale applications.

## Main Components

The prototype system consists of the following primary hardware components:

* Microcontroller: Used to control system operations, including valve timing, motor control, and user inputs.

* Hoppers (Material Storage): Containers used to hold separate media types (e.g., soy hulls and wood pellets) prior to dispensing.

* Dispensing Mechanism (Valves/Gates): Electronically controlled gates or valves regulate the flow of material from each hopper into the mixing chamber.

* User Input Controls: Computer screen menu allows the user to start/stop the system and select mixing settings.
  
* Indicator System: The computer screen will print an alert message that notifies the user when a hopper is empty or when the system is active.
  
* Power Supply: 5V required, can easily be plugged into a computer's USB port.

### Optional / Future Components (Full-Scale System)
The prototype does not yet include all features of a full-scale system. The following components are planned for future integration:

* Water Delivery System: A pressure-regulated water source for hydrating media during mixing.
  
* Advanced Sensors: Load cells or level sensors for improved accuracy in measuring material quantities.

## System Integration

All components are connected through the microcontroller, which serves as the central control unit. The microcontroller receives user inputs, executes the programmed logic, and sends signals to actuate the dispensing mechanisms and mixing motor. The system is designed to be modular, allowing components to be easily modified or upgraded.
