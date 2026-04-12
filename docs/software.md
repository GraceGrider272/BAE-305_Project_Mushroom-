# Software

## Software Overview

This section describes the control logic, code structure, and implementation used in the small-scale mushroom media mixer prototype. The software is responsible for automating the dispensing and mixing process based on user input while ensuring reliable and repeatable system operation.

### Control System Architecture
The system is controlled using a microcontroller (e.g., Arduino Uno), which acts as the central processing unit. The microcontroller receives user input, executes programmed logic, and sends output signals to control the dispensing mechanisms, mixing motor, and indicator system.

The control system follows a simple sequence:

1. User inputs desired settings (ratio, start command)
2. System initiates dispensing sequence
3. Materials are released from hoppers based on timing logic
4. Mixing process begins for a set duration
5. System stops and indicates completion

A visual representation of this logic is shown in Figure 1 (Control Flow Diagram).

### Control Logic

The system operates using a time-based control approach to regulate material dispensing. Each hopper is assigned a specific activation time based on the desired ratio. The microcontroller activates each dispensing mechanism for a calculated duration to achieve the target proportions.

The general control logic includes:
* Reading user input (buttons or switches)
* Converting desired ratios into timing values
* Activating dispensing outputs (valves/gates)
* Running the mixing motor for a set time
* Monitoring system status (e.g., hopper empty indicators)

This approach allows for a simple and cost-effective implementation while maintaining acceptable accuracy for small-scale applications.

## Code Implementation

The system code is written in a microcontroller-compatible language (e.g., Arduino C/C++). The program is structured into the following main sections:

* Initialization (Setup Function): Configures input/output pins and initializes system state.
* Main Loop: Continuously checks for user input and executes the control sequence.
* Dispensing Functions: Controls timing and activation of hopper outputs.
* Indicator Functions: Handles LED or alert signals for system status.

The full code implementation is provided in Appendix A or linked through the project repository.

## Future Improvements

While the current software meets the needs of the prototype, several improvements can be made in future iterations:
* Integration of sensor feedback (e.g., load cells) for increased accuracy
* Closed-loop control instead of time-based operation
* Mobile or app-based interface for remote control
* Adjustable calibration settings for different media types
