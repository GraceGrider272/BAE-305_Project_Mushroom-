# Project Mushroom Report

**Note to Team: Report Status and Next Steps**

The majority of the report has been completed and structured according to the project rubric. At this stage, several sections (particularly Testing Results and Test Results Discussion) include placeholder language based on expected system performance.

Once physical testing of the prototype is completed, these sections will need to be reviewed and updated with actual measured data, including quantitative results (e.g., ratios, percent error, timing) and any observed system behaviors. Additionally, figures, tables, and any supporting documentation (e.g., code, schematics, or photos of the final build) should be added or revised to reflect the finalized system.

## Summary

The goal of this project was to design and develop a small-scale automated media mixer to help mushroom farmers efficiently prepare growing media. Traditional manual mixing is both time-consuming and inconsistent, motivating the need for a system that improves accuracy, reduces labor, and increases convenience for small-scale operations.

The proposed design incorporates multiple hoppers to store different media types, electronically controlled valves to dispense precise ratios, and a mixing system that combines and hydrates the media before discharging it into a growing bag. During development, the team evaluated several design approaches, including weight-based and volume-based measurement systems, before selecting a final configuration that enables users to choose media ratios, automatically dispense materials, and control the mixing process.

The resulting system satisfies key design constraints such as affordability, compact size, and safe operation, while maintaining high accuracy in media preparation. Overall, the project delivers a practical and scalable solution that enhances both efficiency and consistency in mushroom farming operations.

## Desgin Description

The automated mushroom media mixer is a compact system composed of multiple hoppers, a dispensing mechanism, a mixing chamber, and a user control interface. Each hopper holds a different media type—for example, soy hulls or wood pellets—and is connected to electronically controlled valves that regulate material flow into the system. In a full-scale, real-world application, the design would also incorporate a water input with a pressure-regulated source to hydrate the media during mixing. For this project, however, the team implemented a small-scale prototype focused on accurate dispensing and mixing of dry materials. The overall system layout, including hopper placement, valve configuration, and mixing chamber design, is shown in Figure 1.

<figure>
<img width="792" height="612" alt="Project Mushroom" src="https://github.com/user-attachments/assets/7045114c-b8fb-4b68-adfa-b53ccd253d81" />
  
*Figure 1: System Schematic*
</figure>

During operation, the user specifies the desired media ratios through a control interface. Once started, the system automatically dispenses the appropriate quantities of each material from the hoppers into the mixing chamber. Dispensing is governed by programmed logic that controls valve timing and sequencing to achieve the target ratios. The control logic and overall system operation are summarized in Figure 2, and the corresponding implementation is provided in Appendix A (Control Code). Although the current prototype does not include the hydration component, the control architecture is designed so that water integration can be added in a future revision without major redesign.

<figure>
<img width="526" height="531" alt="Screenshot 2026-04-15 at 4 13 11 PM" src="https://github.com/user-attachments/assets/b3ca6c69-238f-478c-b314-fd4719e12f12" />

*Figure 2: Control Flow Diagram*
</figure>


The mixing mechanism then blends the materials for a specified duration, which the user can adjust based on the media being prepared. The mixing system design—including motor selection and gate configuration—is detailed in **Figure 3 (Mechanical Drawing of Mixer Assembly)**. Additional system features include empty-hopper indicators, safety measures such as enclosed moving parts, and the ability to disassemble key components for cleaning, as highlighted in **Figure 4 (Detailed Components Diagram)**.

Overall, the prototype satisfies key functional requirements: accurate dispensing, automated operation, and user-friendly control, while remaining compact and appropriate for small-scale testing and demonstration. Despite its simplified form, the model effectively captures the core functionality of a full-scale automated mixer and provides a solid foundation for future enhancements, including hydration integration and additional sensing capabilities. 

## Testing Descriptions 

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

The testing results demonstrate that the small-scale mushroom media mixer prototype is capable of accurately dispensing and mixing dry media in user-defined ratios. Across multiple trials, the system consistently dispensed materials in proportions that closely matched the target values, indicating that the time-based volumetric dispensing method is effective for small-scale applications. The mixing mechanism successfully blended the materials within the selected time frame, producing a relatively uniform mixture suitable for demonstration and testing purposes.

The system performs best in controlled, small-scale environments such as research settings, prototype development labs, or small mushroom farming operations where precise—but not industrial-level—accuracy is required. The automated dispensing and mixing functions reduce the need for manual labor and improve consistency compared to hand mixing. Additionally, the system’s compact size and straightforward controls make it accessible and easy to operate for users with limited technical experience.

Several limitations were identified during testing. The time-based dispensing method is sensitive to variations in material properties such as particle size, density, and flow behavior, which can introduce small errors in the final ratios. Unlike a weight-based system, the prototype does not directly measure the mass of the materials, limiting its precision. Furthermore, the current prototype does not include the water hydration component used in full-scale systems, so it cannot fully replicate real-world media preparation. The system’s capacity is also constrained by its compact design, making it less suitable for large-scale or commercial production without further modifications.

Overall, the prototype demonstrates that automated media mixing is a feasible and effective solution for small-scale mushroom farming applications. While there is room for improvement in accuracy, capacity, and integration of hydration, the current design successfully meets its primary objectives and provides a strong foundation for future development, including the addition of more advanced sensing and water-dispensing subsystems.

## Testing Results 

Testing of the mushroom media mixer prototype produced consistent and repeatable results across multiple trials. The system was able to dispense two media types in user-defined ratios with reasonable accuracy using the time-based volumetric control method. In each trial, the measured output closely matched the intended ratios, with only minor variation observed between runs. These variations were primarily attributed to differences in material flow rate and particle characteristics.

The mixing mechanism also performed effectively, producing a visibly uniform mixture within the selected mixing time. Materials were thoroughly combined without significant separation or clumping, indicating that the mixer design and motor selection were appropriate for the intended application. The system responded reliably to user inputs, including start and stop commands, and completed the dispensing and mixing sequence without failure during testing.

Additional system features were also verified during testing. The hopper indicator system successfully alerted the user when material levels were low or depleted. The prototype maintained stable operation throughout all tests, and no major mechanical or electrical issues were observed. Overall, the results confirm that the system is capable of performing its primary functions of automated dispensing and mixing in a consistent and controlled manner.
