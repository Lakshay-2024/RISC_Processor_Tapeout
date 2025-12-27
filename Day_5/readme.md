# Final steps for RTL2GDS using tritonRoute and openSTA 

## Theory

### Implementation

* Section 5 tasks:-
1. Perform generation of Power Distribution Network (PDN) and explore the PDN layout.
2. Perfrom detailed routing using TritonRoute.
3. Post-Route parasitic extraction using SPEF extractor.
4. Post-Route OpenSTA timing analysis with the extracted parasitics of the route.


#### 1. Perform generation of Power Distribution Network (PDN) and explore the PDN layout.

Commands to perform all necessary stages up until now

```bash
# Change directory to openlane flow directory
cd Desktop/Openlane
make mount

```
```tcl
# Now that we have entered the OpenLANE flow contained docker sub-system we can invoke the OpenLANE flow in the Interactive mode using the following command
./flow.tcl -interactive

# Now that OpenLANE flow is open we have to input the required packages for proper functionality of the OpenLANE flow
package require openlane 

# Now the OpenLANE flow is ready to run any design and initially we have to prep the design creating some necessary files and directories for running a specific design which in our case is 'picorv32a'
prep -design picorv32a

# Addiitional commands to include newly added lef to openlane flow merged.lef
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs

# Command to set new value for SYNTH_STRATEGY
set ::env(SYNTH_STRATEGY) "DELAY 3"

# Command to set new value for SYNTH_SIZING
set ::env(SYNTH_SIZING) 1

# Now that the design is prepped and ready, we can run synthesis using following command
run_synthesis

# Following commands are alltogather sourced in "run_floorplan" command
init_floorplan
place_io
tap_decap_or

# Now we are ready to run placement
run_placement

# Incase getting error
unset ::env(LIB_CTS)

# With placement done we are now ready to run CTS
run_cts

# Now that CTS is done we can do power distribution network
gen_pdn 
```

Screenshots of PDN def

![Screenshot from 2024-03-26 14-30-52](https://github.com/Lakshay-2024/Digital_VLSI_RISC_Tapeout/blob/main/Day_2/images/placed_design.png)
![Screenshot from 2024-03-26 14-32-24](https://github.com/Lakshay-2024/Digital_VLSI_RISC_Tapeout/blob/main/Day_2/images/zoom_placement.png)

#### 2. Perfrom detailed routing using TritonRoute and explore the routed layout.

Command to perform routing

```tcl
# Check value of 'CURRENT_DEF'
echo $::env(CURRENT_DEF)

# Check value of 'ROUTING_STRATEGY'
echo $::env(ROUTING_STRATEGY)

# Command for detailed route using TritonRoute
run_routing
```

Commands to load routed def in magic in another terminal

```bash
# Change directory to path containing routed def
cd /OpenLane/designs/picorv32a/runs/RUN_2025.12.26_13.02.52/results/routing/

# Command to load the routed def in magic tool
magic -T /home/vscode/.ciel/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.def &
```

Screenshots of routed def

![Screenshot from 2024-03-26 15-33-12](https://github.com/Lakshay-2024/Digital_VLSI_RISC_Tapeout/blob/main/Day_5/images/routed_design1.png)
![Screenshot from 2024-03-26 15-30-36](https://github.com/Lakshay-2024/Digital_VLSI_RISC_Tapeout/blob/main/Day_5/images/routed_design2.png)
![Screenshot from 2024-03-26 15-31-29](https://github.com/Lakshay-2024/Digital_VLSI_RISC_Tapeout/blob/main/Day_5/images/routed_design3.png)

Screenshot of fast route guide present in `/home/lakshay/vsdflow/OpenLane/designs/picorv32a/runs/RUN_2025.12.26_13.02.52/tmp/routing` directory

![Screenshot from 2024-03-26 15-41-18](https://github.com/Lakshay-2024/Digital_VLSI_RISC_Tapeout/blob/main/Day_5/images/fast_route_file.png)

#### 3. Post-Route parasitic extraction using SPEF extractor.

Commands for SPEF extraction using external tool

```bash
# Change directory
cd Desktop/work/tools/SPEF_EXTRACTOR

# Command extract spef
python3 main.py /home/lakshay/vsdflow/OpenLane/designs/picorv32a/runs/RUN_2025.12.26_13.02.52/tmp/merged.nom.lef /home/lakshay/vsdflow/OpenLane/designs/picorv32a/runs/RUN_2025.12.26_13.02.52/results/routing/picorv32a.def
```

#### 4. Post-Route OpenSTA timing analysis with the extracted parasitics of the route.

Commands to be run in OpenLANE flow to do OpenROAD timing analysis with integrated OpenSTA in OpenROAD

```tcl
# Command to run OpenROAD tool
openroad

# Reading lef file
read_lef /OpenLane/designs/picorv32a/runs/RUN_2025.12.26_13.02.52/tmp/merged.nom.lef

# Reading def file
read_def /OpenLane/designs/picorv32a/runs/RUN_2025.12.26_13.02.52/results/routing/picorv32a.def

# Creating an OpenROAD database to work with
write_db pico_route.db

# Loading the created database in OpenROAD
read_db pico_route.db

# Read netlist post CTS
read_verilog /OpenLane/designs/picorv32a/runs/RUN_2025.12.26_13.02.52/results/synthesis/picorv32a.v

# Read library for design
read_liberty $::env(LIB_SYNTH_COMPLETE)

# Link design and library
link_design picorv32a

# Read in the custom sdc we created
read_sdc /OpenLane/designs/picorv32a/src/my_base.sdc

# Setting all cloks as propagated clocks
set_propagated_clock [all_clocks]

# Read SPEF
read_spef /OpenLane/designs/picorv32a/runs/RUN_2025.12.26_13.02.52/results/routing/picorv32a.spef

# Generating custom timing report
report_checks -path_delay min_max -fields {slew trans net cap input_pins} -format full_clock_expanded -digits 4

# Exit to OpenLANE flow
exit
```

Screenshots Timing report generated

![Screenshot from 2024-03-26 23-16-16](https://github.com/Lakshay-2024/Digital_VLSI_RISC_Tapeout/blob/main/Day_5/images/p14.png)
![Screenshot from 2024-03-26 23-17-09](https://github.com/Lakshay-2024/Digital_VLSI_RISC_Tapeout/blob/main/Day_5/images/p13.png)

