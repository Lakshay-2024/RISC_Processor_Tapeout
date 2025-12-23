# Design library cell using Magic Layout and ngspice characterization

## Theory

## Implementation

* Tasks:-
1. Clone custom inverter standard cell design from github repository: [Standard cell design and characterization using OpenLANE flow](https://github.com/nickson-jose/vsdstdcelldesign).
2. Load the custom inverter layout in magic and explore.
3. Spice extraction of inverter in magic.
4. Editing the spice model file for analysis through simulation.
5. Post-layout ngspice simulations.
6. Find problem in the DRC section of the old magic tech file for the skywater process and fix them.

### 1. Clone custom inverter standard cell design from github repository

```bash
# Change directory to openlane
cd Desktop/work/tools/openlane_working_dir/openlane

# Clone the repository with custom inverter design
git clone https://github.com/nickson-jose/vsdstdcelldesign

# Change into repository directory
cd vsdstdcelldesign

# Copy magic tech file to the repo directory for easy access
cp /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech .

# Check contents whether everything is present
ls

# Command to open custom inverter layout in magic
magic -T sky130A.tech sky130_inv.mag &
```

Screenshot of commands run

![Screenshot from 2024-03-19 00-22-27](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/8f304929-a190-4aa1-9cc4-b8fefa1909e8)

### 2. Load the custom inverter layout in magic and explore.

Screenshot of custom inverter layout in magic

![Screenshot from 2024-03-19 00-22-44](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/6eae887c-ebc6-4771-8bcc-e4edaf9947d9)

NMOS and PMOS identified

![Screenshot from 2024-03-19 00-28-03](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/eada5c8b-154c-4eea-819c-d49f89495acb)
![Screenshot from 2024-03-19 00-29-14](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/dec7e465-8fd0-45d5-bac6-10de1acb8c76)

Output Y connectivity to PMOS and NMOS drain verified

![Screenshot from 2024-03-19 00-31-17](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/e16fb883-907e-437a-b8d8-1b9d1b2e67a6)

PMOS source connectivity to VDD (here VPWR) verified

![Screenshot from 2024-03-19 00-34-11](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/173efda1-d5d1-49d9-a040-771092b1e55b)

NMOS source connectivity to VSS (here VGND) verified

![Screenshot from 2024-03-19 00-36-09](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/107a8e0b-43de-4b5d-90fe-516ea83673b1)

Deleting necessary layout part to see DRC error

![Screenshot from 2024-03-19 01-10-28](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/861912e4-eef9-4226-b563-db7f49ca6632)

### 3. Spice extraction of inverter in magic.

Commands for spice extraction of the custom inverter layout to be used in tkcon window of magic

```tcl
# Check current directory
pwd

# Extraction command to extract to .ext format
extract all

# Before converting ext to spice this command enable the parasitic extraction also
ext2spice cthresh 0 rthresh 0

# Converting to ext to spice
ext2spice
```

Screenshot of tkcon window after running above commands

![Screenshot from 2024-03-19 01-24-17](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/831b0be9-3c02-4bbb-800e-6f1c3dc1ba1a)

Screenshot of created spice file

![Screenshot from 2024-03-19 01-27-07](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/2c645f55-c4d5-4007-8b9c-73ba8a8e5bcb)

### 4. Editing the spice model file for analysis through simulation.

Measuring unit distance in layout grid

![Screenshot from 2024-03-19 01-30-15](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/74335564-b7a6-4b7d-b4b7-bb251c8d790b)

Final edited spice file ready for ngspice simulation

![Screenshot from 2024-03-19 14-50-54](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/b5d20507-b65e-4b54-ba8e-576fb4d09429)
![Screenshot from 2024-03-19 14-51-16](https://github.com/fayizferosh/soc-design-and-planning-nasscom-vsd/assets/63997454/9cd17c95-de5b-48da-b8bc-2ee0c30915ef)

### 5. Post-layout ngspice simulations.

Commands for ngspice simulation

```bash
# Command to directly load spice file for simulation to ngspice
ngspice sky130_inv.spice

# Now that we have entered ngspice with the simulation spice file loaded we just have to load the plot
plot y vs time a
```

Screenshots of ngspice run

![Screenshot from 2024-03-19 14-56-42](https://github.com/Lakshay-2024/Digital_VLSI_RISC_Tapeout/blob/main/Day_3/images/ngspice_simulation_1.png)

Screenshot of generated plot

![Screenshot from 2024-03-19 14-58-55](https://github.com/Lakshay-2024/Digital_VLSI_RISC_Tapeout/blob/main/Day_3/images/ngspice_simulation_2.png)

Rise transition time calculation

```math
Rise\ transition\ time = Time\ taken\ for\ output\ to\ rise\ to\ 80\% - Time\ taken\ for\ output\ to\ rise\ to\ 20\%
```
```math
20\%\ of\ output = 660\ mV
```
```math
80\%\ of\ output = 2.64\ V
```

![Screenshot from 2024-03-19 15-15-02](https://github.com/Lakshay-2024/Digital_VLSI_RISC_Tapeout/blob/main/Day_3/images/rise_transition_time.png)


```math
Rise\ transition\ time = 6.24742 - 6.18256 = 0.06486\ ns = 64.86\ ps
```

Fall transition time calculation

```math
Fall\ transition\ time = Time\ taken\ for\ output\ to\ fall\ to\ 20\% - Time\ taken\ for\ output\ to\ fall\ to\ 80\%
```
```math
20\%\ of\ output = 660\ mV
```
```math
80\%\ of\ output = 2.64\ V
```


![Screenshot from 2024-03-19 15-34-22](https://github.com/Lakshay-2024/Digital_VLSI_RISC_Tapeout/blob/main/Day_3/images/fall_transition_time.png)

```math
Fall\ transition\ time = 6.18026 - 6.12834 = 0.05192\ ns = 51.92\ ps
```

Rise Cell Delay Calculation

```math
Rise\ Cell\ Delay = Time\ taken\ for\ output\ to\ rise\ to\ 50\% - Time\ taken\ for\ input\ to\ fall\ to\ 50\%
```
```math
50\%\ of\ 3.3\ V = 1.65\ V
```

![Screenshot from 2024-03-19 16-02-35](https://github.com/Lakshay-2024/Digital_VLSI_RISC_Tapeout/blob/main/Day_3/images/rise_cell_delay.png)


```math
Rise\ Cell\ Delay = 6.21146 - 6.15 = 0.06146\ ns = 61.46\ ps
```

Fall Cell Delay Calculation

```math
Fall\ Cell\ Delay = Time\ taken\ for\ output\ to\ fall\ to\ 50\% - Time\ taken\ for\ input\ to\ rise\ to\ 50\%
```
```math
50\%\ of\ 3.3\ V = 1.65\ V
```

![Screenshot from 2024-03-19 16-09-08](https://github.com/Lakshay-2024/Digital_VLSI_RISC_Tapeout/blob/main/Day_3/images/fall_cell_delay.png)


```math
Fall\ Cell\ Delay = 8.07844 - 8.05031 = 0.02813\ ns = 28.13\ ps
```

### 6. Find problem in the DRC section of the old magic tech file for the skywater process and fix them.

Link to Sky130 Periphery rules: [https://skywater-pdk.readthedocs.io/en/main/rules/periphery.html](https://skywater-pdk.readthedocs.io/en/main/rules/periphery.html)

Commands to download and view the corrupted skywater process magic tech file and associated files to perform drc corrections

```bash
# Change to home directory
cd

# Command to download the lab files
wget http://opencircuitdesign.com/open_pdks/archive/drc_tests.tgz

# Since lab file is compressed command to extract it
tar xfz drc_tests.tgz

# Change directory into the lab folder
cd drc_tests

# List all files and directories present in the current directory
ls -al

# Command to view .magicrc file
gvim .magicrc

# Command to open magic tool in better graphics
magic -d XR &
```

Screenshots of commands run

![Screenshot from 2024-03-21 22-33-57](https://github.com/Lakshay-2024/Digital_VLSI_RISC_Tapeout/blob/main/Day_3/images/installing_drc_lib.png)

**Incorrectly implemented poly.9 simple rule correction**

Screenshot of poly rules

![Screenshot from 2024-03-21 22-54-49](https://github.com/Lakshay-2024/Digital_VLSI_RISC_Tapeout/blob/main/Day_3/images/poly_rules.png)

Incorrectly implemented poly.9 rule no drc violation even though spacing < 0.48u

![Screenshot from 2024-03-21 22-54-19](https://github.com/Lakshay-2024/Digital_VLSI_RISC_Tapeout/blob/main/Day_3/images/poly_drc_ok.png)

New commands inserted in sky130A.tech file to update drc

![Screenshot from 2024-03-21 23-58-44](https://github.com/Lakshay-2024/Digital_VLSI_RISC_Tapeout/blob/main/Day_3/images/poly_drc_correct1.png)
![Screenshot from 2024-03-21 23-09-50](https://github.com/Lakshay-2024/Digital_VLSI_RISC_Tapeout/blob/main/Day_3/images/poly_drc_correct2.png)

Commands to run in tkcon window

```tcl
# Loading updated tech file
tech load sky130A.tech

# Must re-run drc check to see updated drc errors
drc check

# Selecting region displaying the new errors and getting the error messages 
drc why
```

Screenshot of magic window with rule implemented

![Screenshot from 2024-03-21 23-13-11](https://github.com/Lakshay-2024/Digital_VLSI_RISC_Tapeout/blob/main/Day_3/images/poly_drc_error.png)

**Incorrectly implemented difftap.2 simple rule correction**

Screenshot of difftap rules

![Screenshot from 2024-03-22 00-14-47](https://github.com/Lakshay-2024/Digital_VLSI_RISC_Tapeout/blob/main/Day_3/images/dff_rules.png)

New commands inserted in sky130A.tech file to update drc

![Screenshot from 2024-03-22 00-26-43](https://github.com/Lakshay-2024/Digital_VLSI_RISC_Tapeout/blob/main/Day_3/images/dff_tap_correction.png)

Commands to run in tkcon window

```tcl
# Loading updated tech file
tech load sky130A.tech

# Must re-run drc check to see updated drc errors
drc check

# Selecting region displaying the new errors and getting the error messages 
drc why
```

Screenshot of magic window with rule implemented

![Screenshot from 2024-03-22 00-29-22](https://github.com/Lakshay-2024/Digital_VLSI_RISC_Tapeout/blob/main/Day_3/images/dff_tap_error.png)

**Incorrectly implemented nwell.4 complex rule correction**

Screenshot of nwell rules

![Screenshot from 2024-03-22 00-51-34](https://github.com/Lakshay-2024/Digital_VLSI_RISC_Tapeout/blob/main/Day_3/images/nwell_rules.png)

Incorrectly implemented nwell.4 rule no drc violation even though no tap present in nwell

![Screenshot from 2024-03-22 00-52-51](https://github.com/Lakshay-2024/Digital_VLSI_RISC_Tapeout/blob/main/Day_3/images/nwell_correct.png)



Commands to run in tkcon window

```tcl
# Loading updated tech file
tech load sky130A.tech

# Change drc style to drc full
drc style drc(full)

# Must re-run drc check to see updated drc errors
drc check

# Selecting region displaying the new errors and getting the error messages 
drc why
```

Screenshot of magic window with rule implemented

![Screenshot from 2024-03-22 01-10-25](https://github.com/Lakshay-2024/Digital_VLSI_RISC_Tapeout/blob/main/Day_3/images/nwell_error.png)
