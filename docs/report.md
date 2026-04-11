# Project Mushroom Report

## Summary

The goal of this project was to design and develop a small-scale automated media mixer to help mushroom farmers efficiently prepare growing media. Traditional manual mixing is both time-consuming and inconsistent, motivating the need for a system that improves accuracy, reduces labor, and increases convenience for small-scale operations.

The proposed design incorporates multiple hoppers to store different media types, electronically controlled valves to dispense precise ratios, and a mixing system that combines and hydrates the media before discharging it into a growing bag. During development, the team evaluated several design approaches, including weight-based and volume-based measurement systems, before selecting a final configuration that enables users to choose media ratios, automatically dispense materials, and control the mixing process.

The resulting system satisfies key design constraints such as affordability, compact size, and safe operation, while maintaining high accuracy in media preparation. Overall, the project delivers a practical and scalable solution that enhances both efficiency and consistency in mushroom farming operations.

## Desgin Description

The automated mushroom media mixer was desingmed as a compact system consisting of multple hoppers, a dispensing mechanism, a mixing chamber, and a user control interface. Each hopper stores a different media, such as soy hulls or wood pellets, and is connected to electronically controlled vales that regulate the flow of material into the system. In a full-scale real worlf applicaiton, the system would also include a water inpit component with a pressure-regulated source to hydrate the media during mixing. However, for the purpose of this project, the tram developed a small-scale prototype that focuses on the accurate dispensing and mixing of dry materials. The overall system layout, including happerplacement, value configuration, and mixing chamber desgin, is shown is **Figure 1 (systerm schematic/engineering drawing)**. 

The prototrype system perates by allowing the iser to input desried media ratios throught a contril mechanism. Once intiated, the system automatically dispenses the correct amounts of each material from the happers into the mixing chambers. This dispensing process is contrilled throught programmed logoc, which regulares valve timing and seyencing to achieve the desired ratios. The control logic and system peration are outlinged in **Figure 2 (control flow diagram)**, and the implementation is provided in **Appendix A (control code)**. While the prototye does not incule the hydration compinent, the control system is desinged such that water integration could be added in a future iteration without significant redesign. 



## Testing Descriptions 
## Design Decision Discussion
## Test Result Discussion
## Testing Results 
