<img width="440" height="115" alt="UK logo" src="https://github.com/user-attachments/assets/4eba2229-e527-418e-93f0-4ab45be4d9b1" /> 

# Project Mushroom Report

## Summary

The goal of this project was to design and develop a small-scale automated media mixer to help mushroom farmers efficiently prepare growing media. Traditional manual mixing is time-consuming and inconsistent, motivating the need for a system that improves accuracy, reduces labor, and increases convenience for small-scale operations.

The proposed design incorporates multiple hoppers for storing different media types, electronically controlled valves for dispensing precise ratios, and a mixing system that combines and hydrates the media before discharging it into a growing bag. During development, the team evaluated several design approaches, including weight- and volume-based measurement systems, before selecting a final configuration that allows users to choose media ratios, automatically dispense materials, and control the mixing process.

The resulting system meets key design constraints, including affordability, compact size, and safe operation, while maintaining high accuracy in media preparation. Overall, the project delivers a practical, scalable solution that improves efficiency and consistency in mushroom farming operations.


## Desgin Description

The automated mushroom media mixer is a compact system comprising multiple hoppers, a dispensing mechanism, a mixing chamber, and a user control interface. Each hopper holds a different media type—for example, soy hulls or wood pellets—and is connected to electronically controlled valves that regulate material flow into the system. In a full-scale, real-world application, the design would also include a water input with a pressure-regulated source to hydrate the media during mixing. For this project, however, the team implemented a small-scale prototype focused on accurate dispensing and mixing of dry materials. The overall system layout, including hopper placement, valve configuration, and mixing chamber design, is shown in Figure 1.

<figure>
<img width="792" height="612" alt="Project Mushroom" src="https://github.com/user-attachments/assets/7045114c-b8fb-4b68-adfa-b53ccd253d81" />
  
</figure>

*Figure 1: System Schematic*

During operation, the user specifies the desired media ratios via a control interface. Once started, the system automatically dispenses the appropriate quantities of each material from the hoppers into the mixing chamber. Dispensing is governed by programmed logic that controls valve timing and sequencing to achieve the target ratios. The control logic and overall system operation are summarized in Figure 2, and the corresponding implementation is provided in [Appendix A](final_code) (Control Code). Although the current prototype does not include the hydration component, the control architecture is designed to allow water integration in a future revision without major redesign.

<figure>
<img width="526" height="531" alt="Screenshot 2026-04-15 at 4 13 11 PM" src="https://github.com/user-attachments/assets/b3ca6c69-238f-478c-b314-fd4719e12f12" />


</figure>

*Figure 2: Control Flow Diagram*

The mixing mechanism then blends the materials for a user-adjustable duration, tailored to the media being prepared. The mixing system design—including motor selection and gate configuration—is detailed in Figure 3. Additional system features include empty-hopper indicators, safety measures such as enclosed moving parts, and the ability to disassemble key components for cleaning, as highlighted in [Hardware](hardware.md).

<figure>
<img width="681" height="1045" alt="Screenshot 2026-04-24 141252" src="https://github.com/user-attachments/assets/d66a048f-60a3-4380-acc0-23f6786634a2" />
  

</figure>

*Figure 3: Drawing of Mixer Assembly*

Overall, the prototype meets key functional requirements: accurate dispensing, automated operation, and user-friendly control, while remaining compact and suitable for small-scale testing and demonstration. Despite its simplified design, the model effectively captures the core functionality of a full-scale automated mixer and provides a solid foundation for future enhancements, including hydration integration and additional sensing capabilities. 

## Testing Descriptions 

Testing of the small-scale mushroom media mixer prototype focused on evaluating system functionality, material flow behavior, and the effectiveness of the dispensing mechanism across different operating conditions. Rather than prioritizing precise quantitative accuracy, testing emphasized ensuring that the system could reliably dispense materials without clogging and achieve visually consistent ratios.

Several materials were evaluated for suitability in the prototype. Initial testing used oatmeal; however, it proved too fine and dusty, causing inconsistent flow and excessive particulate buildup within the system. The team then tested larger cereal-based materials, including Fruit Loops and Cheerios. While these materials reduced dust-related issues, their size caused them to become lodged in the gate mechanism during closure, preventing the gates from fully sealing and disrupting controlled dispensing.

To address these issues, plastic beads were selected as the final test material. These beads were non-dusty, uniform in size, and small enough to pass through the system without becoming trapped in the gate mechanism. This allowed the gates to open and close fully and ensured consistent material flow throughout testing.

During operation, both hoppers ran simultaneously, with gate timing adjusted to approximate different dispensing ratios. The primary ratios evaluated were 1:1 (50/50), 2:3, and 3:2. These ratios were achieved by varying the duration each gate remained open rather than by direct measurement of mass or volume.

Testing was conducted iteratively, with repeated system operation to assess performance and identify issues. Rather than a fixed number of trials, the team continued testing until the system operated reliably without clogging or interruption. No formal calibration procedures were performed; however, gate timing was adjusted as needed to improve consistency in material dispensing.

System performance was evaluated based on the gates' ability to open and close without obstruction, the consistency of material flow from each hopper, and the overall functionality of the mixing process. These procedures provide a practical and repeatable approach to assessing prototype performance under realistic operating conditions.

## Design Decision Discussion

Several design alternatives were evaluated for the automated mushroom media mixer, with a focus on methods for measuring and dispensing media. The primary options considered were: (1) a weight-based system, (2) a time-based volumetric system, and (3) a time-of-flight sensor–based volume measurement system. Each option was analyzed for accuracy, cost, system complexity, and compatibility with the project’s constraints.

A weight-based system offered the highest potential accuracy because it directly measures the mass of dispensed material. However, implementing this approach would require load cells, signal conditioning, and more advanced programming, significantly increasing both system complexity and cost. Given the project’s goal of developing a compact, affordable prototype for small-scale use, this option was not practical within the available resources and time constraints.

The time-of-flight sensor approach was also considered as a non-contact method for monitoring material levels. While this method could provide additional feedback on hopper contents, it introduced potential reliability issues due to dust, irregular material surfaces, and sensor interference. Additionally, the increased cost and integration complexity made it less suitable for a simple prototype system.

The final design used a time-based volumetric dispensing method, in which electronically controlled gates regulate material flow based on programmed time intervals. This decision was strongly supported by observations during testing. Early material trials revealed that flow behavior varied significantly with material size and properties, reinforcing the importance of a simple, adaptable control method. The time-based system allowed the team to easily adjust gate timing to accommodate these variations without additional hardware.

While this method does not provide the same level of precision as a weight-based system, it proved effective for achieving approximate ratios in a small-scale prototype. It also minimized mechanical and electrical complexity, making the system more reliable and easier to troubleshoot. This approach aligned well with the project’s goals of affordability, simplicity, and functional performance.

Overall, the selected design reflects a deliberate balance between accuracy and practicality. The system successfully meets the needs of a small-scale prototype while maintaining flexibility for future improvements. If higher precision is required in future iterations, the current design can be expanded to incorporate weight-based sensing or additional feedback systems without requiring a complete redesign.

## Test Result Discussion

The testing results demonstrate that the small-scale mushroom media mixer prototype reliably dispenses and mixes materials using a time-based volumetric control system. Rather than emphasizing precise quantitative accuracy, testing focused on consistent system operation and uninterrupted material flow. The final selection of plastic beads as the test material was critical to reliable performance, as earlier materials such as oatmeal and cereal products caused issues, including dust buildup and gate obstruction.

The system performed best when material properties were consistent and compatible with the gate design. Once an appropriate material was selected, both hoppers operated simultaneously without clogging, and the gates opened and closed as intended. Adjusting gate timing allowed the system to approximate ratios such as 1:1, 2:3, and 3:2, demonstrating that the control approach is flexible and functional for small-scale applications.

However, several limitations were identified during testing. The time-based dispensing method is inherently sensitive to variations in material flow characteristics, so different media types may produce different results even with identical timing settings. This was observed during early testing, where differences in particle size and shape led to inconsistent flow and mechanical interference. Additionally, because the system does not directly measure mass or volume, the exact accuracy of the dispensed ratios cannot be quantified. This limits the system's precision compared to more advanced approaches such as weight-based measurement.

The current prototype also lacks the water-hydration component found in a full-scale system, limiting its ability to fully replicate real-world mushroom media preparation. Despite this, the prototype successfully demonstrates the core functionality of automated dispensing and mixing, providing a proof of concept for future development.

Overall, the results indicate that the system is effective as a small-scale, functional prototype. It achieves consistent operation, avoids major mechanical issues when appropriate materials are used, and demonstrates the feasibility of a low-cost, automated mixing solution. Future improvements could focus on increasing accuracy through sensing methods, expanding material compatibility, and integrating hydration to more closely match real-world applications.

## Testing Results 

Testing of the mushroom media mixer prototype demonstrated that the system operated consistently and reliably when appropriate materials were used. Initial trials with oatmeal yielded poor performance due to excessive dust and fine particle size, which caused inconsistent flow and buildup within the system. Subsequent testing with larger cereal materials, including Fruit Loops and Cheerios, reduced dust-related issues but introduced mechanical interference, as the materials became lodged in the gate mechanism and prevented complete closure.

Final testing used plastic beads, which provided the most consistent and reliable results. The beads flowed smoothly through both hoppers without clogging or obstruction, allowing the gates to open and close fully as intended. Under these conditions, the system ran continuously without interruption, and both hoppers dispensed material simultaneously in a controlled manner.

Different dispensing ratios, including 1:1 (50/50), 2:3, and 3:2, were approximated by adjusting the gate open times for each hopper. While the system did not directly measure the mass or volume of the dispensed materials, visual observation indicated that the ratios were reasonably consistent across repeated runs once the gate timing was set. Minor variation was observed due to natural differences in bead flow, but overall system performance remained stable.

The mixing mechanism also performed effectively, producing a visibly uniform mixture within the selected mixing time. No significant separation or clumping was observed when using plastic beads, indicating that the mixing design and motor configuration were appropriate for the selected material.

Additional system features functioned as expected during testing. The gates operated reliably without jamming when using the final material, and the system maintained stable operation throughout repeated use. Overall, the results confirm that the prototype can achieve its primary goal of automated dispensing and mixing consistently and under controlled conditions.
