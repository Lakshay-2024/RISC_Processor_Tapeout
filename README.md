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
| **DAY 5** |  [Routing and Post Route STA](Day_5/readme.md)


###  Key Learnings from Day 1
- Overview of entire **Physical Design Flow** including Synthesis, Floorplan, Placement, Clock Tree Synthesis , Routing and Signoff .  
- Synthesis of RISCV core using **Yosys** synthesiser in Openlane .  
- Calculation of **Flop Ratio** in the synthesised netlist.

###  Key Learnings from Day 2
- Understand the theory behind **Floorplan and Placement**.  
- Run floorplan for the RISCV core , calculate **die area** and analyse the floorplan layout.  
- Run placement for the design and explore various **library cells** in the placed design.

### Key Learnings from Day 3
- Generate **spice** file for a custom inverter design in magic.
- **Characterise** the inverter cell using spice simulations.
- Analyse and correct **DRC rules** in varous designs in magic editor. 

### Key Learnings from Day 4
- Add the **custom inverter** in the RISCV core design.
- Analyse the **STA Report** of the updated synthesised netlist.
- Explore and Implement **strategies** to fix timing errors.
- Run **Clock Tree Synthesis** of the updated netlist and analyse the report.

### Key Learnings from Day 5
- Understand the various **Routing algorithms** and strategies.
- Run **routing** for the RISCV core design and understand the routing issues.
- Extract the **parasitic data** after routing and do STA analysis of final routed design


##  Acknowledgment  

I am thankful to [**Kunal Ghosh**](https://github.com/kunalg123) and Team **[VLSI System Design (VSD)](https://vsdiat.vlsisystemdesign.com/)** for guiding me through the Physical Design Workshop with Openlane and Google skywater 130nm pdk.  

I also acknowledge the support of **RISC-V International**, **India Semiconductor Mission (ISM)**, **VLSI Society of India (VSI)**, and [**Efabless**](https://github.com/efabless) for making this initiative possible.  




---
