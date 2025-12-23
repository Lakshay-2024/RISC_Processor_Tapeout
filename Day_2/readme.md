## Section 2 - Good floorplan vs bad floorplan and introduction to library cells (16/03/2024 - 17/03/2024)

### Theory

### Implementation

Section 2 tasks:- 
1. Run 'picorv32a' design floorplan using OpenLANE flow and generate necessary outputs.
2. Calculate the die area in microns from the values in floorplan def.
3. Load generated floorplan def in magic tool and explore the floorplan.
4. Run 'picorv32a' design congestion aware placement using OpenLANE flow and generate necessary outputs.
5. Load generated placement def in magic tool and explore the placement.

```math
Area\ of\ die\ in\ microns = Die\ width\ in\ microns * Die\ height\ in\ microns
```

* All section 2 logs, reports and results can be found in following run folder:

[Section 2 Run - 17-03_12-06](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/tree/main/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/17-03_12-06)

#### 1. Run 'picorv32a' design floorplan using OpenLANE flow and generate necessary outputs.

Commands to invoke the OpenLANE flow and perform floorplan

```bash
# Change directory to openlane flow directory
cd Desktop/work/tools/openlane_working_dir/openlane

# alias docker='docker run -it -v $(pwd):/openLANE_flow -v $PDK_ROOT:$PDK_ROOT -e PDK_ROOT=$PDK_ROOT -u $(id -u $USER):$(id -g $USER) efabless/openlane:v0.21'
# Since we have aliased the long command to 'docker' we can invoke the OpenLANE flow docker sub-system by just running this command
docker
```
```tcl
# Now that we have entered the OpenLANE flow contained docker sub-system we can invoke the OpenLANE flow in the Interactive mode using the following command
./flow.tcl -interactive

# Now that OpenLANE flow is open we have to input the required packages for proper functionality of the OpenLANE flow
package require openlane 0.9

# Now the OpenLANE flow is ready to run any design and initially we have to prep the design creating some necessary files and directories for running a specific design which in our case is 'picorv32a'
prep -design picorv32a

# Now that the design is prepped and ready, we can run synthesis using following command
run_synthesis

# Now we can run floorplan
run_floorplan
```

Screenshot of floorplan run

![Screenshot from 2024-03-17 18-06-19](https://github.com/Lakshay-2024/Digital_VLSI_RISC_Tapeout/blob/main/Day_2/images/run_floorplan_commands.png)

#### 2. Calculate the die area in microns from the values in floorplan def.

Screenshot of contents of floorplan def

![Screenshot from 2024-03-17 18-34-53](https://github.com/Lakshay-2024/Digital_VLSI_RISC_Tapeout/blob/main/Day_2/images/flooorplan_def_file.png)

According to floorplan def
```math
1000\ Unit\ Distance = 1\ Micron
```
```math
Die\ width\ in\ unit\ distance = 660685 - 0 = 660685
```
```math
Die\ height\ in\ unit\ distance = 671405 - 0 = 671405
```
```math
Distance\ in\ microns = \frac{Value\ in\ Unit\ Distance}{1000}
```
```math
Die\ width\ in\ microns = \frac{660685}{1000} = 660.685\ Microns
```
```math
Die\ height\ in\ microns = \frac{671405}{1000} = 671.405\ Microns
```
```math
Area\ of\ die\ in\ microns = 660.685 * 671.405 = 443587.212425\ Square\ Microns
```

#### 3. Load generated floorplan def in magic tool and explore the floorplan.

Commands to load floorplan def in magic in another terminal

```bash
# Change directory to path containing generated floorplan def
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/17-03_12-06/results/floorplan/

# Command to load the floorplan def in magic tool
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &
```

Screenshots of floorplan def in magic

![Screenshot from 2024-03-17 18-05-19](https://github.com/Lakshay-2024/Digital_VLSI_RISC_Tapeout/blob/main/Day_2/images/floorplan_magic.png)

Equidistant placement of ports

![Screenshot from 2024-03-17 18-14-28](https://github.com/Lakshay-2024/Digital_VLSI_RISC_Tapeout/blob/main/Day_2/images/pin_placement.png)

Port layer as set through config.tcl

![Screenshot from 2024-03-17 18-17-46](https://github.com/Lakshay-2024/Digital_VLSI_RISC_Tapeout/blob/main/Day_2/images/port_layer.png)
![Screenshot from 2024-03-17 18-19-50](https://github.com/Lakshay-2024/Digital_VLSI_RISC_Tapeout/blob/main/Day_2/images/port_layer2.png)

Decap Cells and Tap Cells

![Screenshot from 2024-03-17 18-22-57](https://github.com/Lakshay-2024/Digital_VLSI_RISC_Tapeout/blob/main/Day_2/images/decap_cells.png)

Diogonally equidistant Tap cells

![Screenshot from 2024-03-17 18-25-28](https://github.com/Lakshay-2024/Digital_VLSI_RISC_Tapeout/blob/main/Day_2/images/tap_cells.png)

Unplaced standard cells at the origin

![Screenshot from 2024-03-17 18-31-41](https://github.com/Lakshay-2024/Digital_VLSI_RISC_Tapeout/blob/main/Day_2/images/logic_in_corner.png)

#### 4. Run 'picorv32a' design congestion aware placement using OpenLANE flow and generate necessary outputs.

Command to run placement

```tcl
# Congestion aware placement by default
run_placement
```

Screenshots of placement run

![Screenshot from 2024-03-17 22-44-17](https://github.com/Lakshay-2024/Digital_VLSI_RISC_Tapeout/blob/main/Day_2/images/run_placement_command.png)


#### 5. Load generated placement def in magic tool and explore the placement.

Commands to load placement def in magic in another terminal

```bash
# Change directory to path containing generated placement def
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/17-03_12-06/results/placement/

# Command to load the placement def in magic tool
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &
```

Screenshots of floorplan def in magic

![Screenshot from 2024-03-17 22-58-44](https://github.com/Lakshay-2024/Digital_VLSI_RISC_Tapeout/blob/main/Day_2/images/placed_design.png)

Standard cells legally placed 

![Screenshot from 2024-03-17 23-04-20](https://github.com/Lakshay-2024/Digital_VLSI_RISC_Tapeout/blob/main/Day_2/images/zoom_placement.png)

Commands to exit from current run

```tcl
# Exit from OpenLANE flow
exit

# Exit from OpenLANE flow docker sub-system
exit
```

