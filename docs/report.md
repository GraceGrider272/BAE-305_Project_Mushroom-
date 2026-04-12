# Project Mushroom Report

## Summary

The goal of this project was to design and develop a small-scale automated media mixer to help mushroom farmers efficiently prepare growing media. Traditional manual mixing is both time-consuming and inconsistent, motivating the need for a system that improves accuracy, reduces labor, and increases convenience for small-scale operations.

The proposed design incorporates multiple hoppers to store different media types, electronically controlled valves to dispense precise ratios, and a mixing system that combines and hydrates the media before discharging it into a growing bag. During development, the team evaluated several design approaches, including weight-based and volume-based measurement systems, before selecting a final configuration that enables users to choose media ratios, automatically dispense materials, and control the mixing process.

The resulting system satisfies key design constraints such as affordability, compact size, and safe operation, while maintaining high accuracy in media preparation. Overall, the project delivers a practical and scalable solution that enhances both efficiency and consistency in mushroom farming operations.

## Desgin Description

The automated mushroom media mixer is a compact system composed of multiple hoppers, a dispensing mechanism, a mixing chamber, and a user control interface. Each hopper holds a different media type—for example, soy hulls or wood pellets—and is connected to electronically controlled valves that regulate material flow into the system. In a full-scale, real-world application, the design would also incorporate a water input with a pressure-regulated source to hydrate the media during mixing. For this project, however, the team implemented a small-scale prototype focused on accurate dispensing and mixing of dry materials. The overall system layout, including hopper placement, valve configuration, and mixing chamber design, is shown in **Figure 1 (System Schematic/Engineering Drawing)**.

During operation, the user specifies the desired media ratios through a control interface. Once started, the system automatically dispenses the appropriate quantities of each material from the hoppers into the mixing chamber. Dispensing is governed by programmed logic that controls valve timing and sequencing to achieve the target ratios. The control logic and overall system operation are summarized in **Figure 2 (Control Flow Diagram)**, and the corresponding implementation is provided in Appendix A (Control Code). Although the current prototype does not include the hydration component, the control architecture is designed so that water integration can be added in a future revision without major redesign.

The mixing mechanism then blends the materials for a specified duration, which the user can adjust based on the media being prepared. The mixing system design—including motor selection and gate configuration—is detailed in **Figure 3 (Mechanical Drawing of Mixer Assembly)**. Additional system features include empty-hopper indicators, safety measures such as enclosed moving parts, and the ability to disassemble key components for cleaning, as highlighted in **Figure 4 (Detailed Components Diagram)**.

Overall, the prototype satisfies key functional requirements: accurate dispensing, automated operation, and user-friendly control, while remaining compact and appropriate for small-scale testing and demonstration. Despite its simplified form, the model effectively captures the core functionality of a full-scale automated mixer and provides a solid foundation for future enhancements, including hydration integration and additional sensing capabilities. 

## Testing Descriptions (This section is to be edited after actually testing the model)

Testing of the small-scale mushroom media mixer prototype focused on evaluating dispensing accuracy, system functionality, and the overall performance of the automated mixing process. The primary goal was to verify that the system could consistently dispense and mix materials in user-defined ratios.

The testing setup included the prototype mixer system, a digital scale (±0.01 g accuracy), a stopwatch for timing measurements, and standard measuring containers. The digital scale was used to measure the mass of dispensed materials, allowing comparison between expected and actual ratios. The system was powered using a standard wall outlet and operated under typical indoor conditions.

To test dispensing accuracy, predetermined ratios of two media types were input into the system. The system was then activated, and materials were dispensed into a collection container. The mass of each material was measured separately using the digital scale. This process was repeated for multiple trials to evaluate consistency. The percent error between the expected and measured values was calculated to assess system accuracy.

To evaluate system functionality, the start/stop controls were tested to ensure reliable operation. The mixing mechanism was also observed during operation to confirm that materials were adequately blended over the selected time period. Additionally, the hopper indicator system was tested by running the system until one hopper was emptied, verifying that the alert mechanism functioned as intended.

Each test was conducted multiple times to ensure repeatability, and results were recorded for comparison. The procedures outlined allow for consistent replication of testing and provide a clear method for evaluating the performance of the prototype system.

## Design Decision Discussion

Several design alternatives were evaluated for the automated mushroom media mixer, focusing on different methods of measuring and dispensing media. The main options considered were: (1) a weight-based system, (2) a time-based volumetric system, and (3) a time-of-flight sensor–based volume measurement system. Each alternative was assessed in terms of accuracy, cost, complexity, and consistency with project constraints.

A weight-based system offered the greatest potential accuracy because it directly measures the mass of dispensed media. However, it required more sophisticated components, including load cells and signal-conditioning electronics, which increased both the cost and complexity of the system. Given the requirement that the design be affordable for small-scale farmers, this option was not chosen for the prototype. The time-of-flight approach, while providing a non-contact method for monitoring material levels, would also raise costs and introduce reliability risks, particularly due to dust and particulates inside the hoppers.

The final design therefore uses a time-based volumetric dispensing method, where electronically actuated valves control flow according to calibrated time intervals. This solution strikes a balance between simplicity, cost, and functional performance. Although it is less precise than a fully weight-based system, it is adequate for small-scale applications and substantially reduces hardware and control complexity. It is also well-suited to a compact prototype and is compatible with the project’s power and affordability limits.

Overall, the design reflects deliberate trade-offs between accuracy and cost, as well as between system complexity and ease of use. The chosen configuration emphasizes a practical, scalable solution that addresses user needs while staying accessible to small-scale farmers. In addition, the selected architecture leaves room for future enhancements—such as adding weight-based sensors or automated water dispensing—without necessitating a complete redesign, supporting long-term system evolution.

## Test Result Discussion
## Testing Results 
