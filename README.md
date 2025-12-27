# Digital_VLSI_RISC_Tapeout


This repository contains all the work I have done in the **Physical Design** workshop using **Openalne and Google skywater 130nm pdk** offered by **VSD VLSI** starting from December 19/2025 - December 28/2025.

This repository documents my **daywise progress** with tasks and assignments inside each day's directory.  



---

## DAYWISE SUMMARY

| Task | Description | 
|------|-------------|
| **DAY 1** |  [Physical Design Flow and Synthesis of RISCV core](Day_1/readme.md) | 
| **DAY 2** |  [Floorplanning and Placement](Day_2/readme.md) |
| **DAY 3** |  [Designing library cell and Cell Characterisation](Day_3/readme.md)
| **DAY 4** |  [STA Analysis and Clock Tree Synthesis](Day_4/readme.md)
| **DAY 5** |  [CMOS SPICE Simulation](Day_5/readme.md)


###  Key Learnings from Day 1
- Overview of entire **Physical Design Flow** including Synthesis, Floorplan, Placement, Clock Tree Synthesis , Routing and Signoff .  
- Synthesis of RISCV core using **Yosys** synthesiser in the Openlane .  
- Calculation of **Flop Ratio** in the synthesised netlist.

###  Key Learnings from Day 2
- Understand the theory behind **Floorplan and Placement**.  
- Run floorplan for the RISCV core , calculate **die area** and understand the floorplan layout.  
- Run placement for the design and understand various **library cells** in the placed design.

### Key Learnings from Day 3
- Generated **spice** file for a custom inverter design in magic.
- **Characterised** the inverter cell using spice simulations.
- Analysed and corrected **DRC rules** in varous designs in magic editor. 

### Key Learnings from Day 4
- Added the **custom inverter** in the RISCV core design.
- Analysed the **STA Report** of the updated synthesised netlist.
- Explored and Implemented **strategies** to fix timing errors.
- Ran **Clock Tree Synthesis** of the updated netlist and analysed the report

### Key Learnings from Week 4
- Introduction to **SPICE** and CMOS Structure.
- Low Node characterstics and CMOS **VTC** simulations.
- CMOS inverter **robustness** to power supply variations and Device Variations


##  Acknowledgment  

I am thankful to [**Kunal Ghosh**](https://github.com/kunalg123) and Team **[VLSI System Design (VSD)](https://vsdiat.vlsisystemdesign.com/)** for the opportunity to participate in the ongoing **RISC-V SoC Tapeout Program**.  

I also acknowledge the support of **RISC-V International**, **India Semiconductor Mission (ISM)**, **VLSI Society of India (VSI)**, and [**Efabless**](https://github.com/efabless) for making this initiative possible.  




---
