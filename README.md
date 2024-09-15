
# Digital VLSI SoC Design and Planning


> A comprehensive 2-week hands-on workshop covering the complete RTL to GDSII flow for VLSI SoC design, organized by VSD in collaboration with NASSCOM.
>
## Section 1 - Inception of open-source EDA, OpenLANE and Sky130 PDK 




### Implementation

Objectives: 
1. Run 'picorv32a' design synthesis using OpenLANE flow and generate necessary outputs.
2. Calculate the flop ratio based on synthesis results.


### Task 1: Running 'picorv32a' Design Synthesis using OpenLANE

Steps to perform synthesis:
  1.Change to the OpenLANE flow directory:

```bash
cd Desktop/work/tools/openlane_working_dir/openlane
```
  2.(Optional) Set up Docker alias if needed:
  ```bash
alias docker='docker run -it -v $(pwd):/openLANE_flow -v $PDK_ROOT:$PDK_ROOT -e PDK_ROOT=$PDK_ROOT -u $(id -u $USER):$(id -g $USER) efabless/openlane:v0.21'
```
3.Start the Docker container:
```bash
docker
```
4.Enter OpenLANE interactive mode:
```tcl
./flow.tcl -interactive
```
5.Load the OpenLANE package:
```tcl
package require openlane 0.9
```
6.Prepare the design 'picorv32a':
```tcl
prep -design picorv32a
```
7.Perform the synthesis:
```tcl
run_synthesis
```
8.Exit the OpenLANE flow:
```tcl
exit
```
Synthesis Output:
Below are screenshots showcasing the execution of synthesis:
* Preparation Completion:

![day1 preparation complete](https://github.com/user-attachments/assets/1e7aac7a-8d80-4bba-9e66-a72410bfaf52)

* Synthesis Running:

![day 1 synthesis](https://github.com/user-attachments/assets/b079e940-8e0a-4421-94f3-f5c791d9a4a5)

### Task 2: Calculating Flop Ratio
We calculate the Flop Ratio and the percentage of D Flip-Flops (DFF) using the synthesis statistics report.
Formulae:

```math
Flop\ Ratio = \frac{Number\ of\ D\ Flip\ Flops}{Total\ Number\ of\ Cells}
```
```math
Percentage\ of\ DFF's = Flop\ Ratio * 100
```
Synthesis Report:
Below is the synthesis statistics report with relevant values highlighted:

![d1s](https://github.com/user-attachments/assets/2197a467-a2ec-4aec-a87b-2759414b1fd6)

Values from the Report:
* Number of D Flip-Flops: 1613
* Total Number of Cells: 14876


Calculation:

```math
Flop\ Ratio = \frac{1613}{14876} = 0.108429685
```
```math
Percentage\ of\ DFF's = 0.108429685 * 100 = 10.84296854\ \%
```

## Section 2 - Good Floorplan vs Bad Floorplan and Introduction to Library Cell 

### Implementation
Objectives: 
1.Run 'picorv32a' Design Floorplan Using OpenLANE.

2.Calculate Die Area in Microns.

3.Load Floorplan DEF in Magic Tool.

4.Run Congestion-Aware Placement.

5.Load Placement DEF in Magic Tool.

### Task 1: Run 'picorv32a' Design Floorplan Using OpenLANE.

Steps to perform Floorplan:
  1.Change to the OpenLANE flow directory:

```bash
cd Desktop/work/tools/openlane_working_dir/openlane
```
  2.(Optional) Set up Docker alias if needed:
  ```bash
alias docker='docker run -it -v $(pwd):/openLANE_flow -v $PDK_ROOT:$PDK_ROOT -e PDK_ROOT=$PDK_ROOT -u $(id -u $USER):$(id -g $USER) efabless/openlane:v0.21'
```
3.Start the Docker container:
```bash
docker
```
4.Enter OpenLANE interactive mode:
```tcl
./flow.tcl -interactive
```
5.Load the OpenLANE package:
```tcl
package require openlane 0.9
```
6.Prepare the design 'picorv32a':
```tcl
prep -design picorv32a
```
7.Perform the synthesis:
```tcl
run_synthesis
```
8.Run the floorplan step:
```tcl
run_floorplan
```
9.Exit the OpenLANE flow:
```tcl
exit
```

Floorplan Output:
Below are screenshots showcasing the execution of Floorplan:

![floorplan 1](https://github.com/user-attachments/assets/9d5b70c4-5f60-4848-a802-2cd0db0ab7f5)

![floorplan](https://github.com/user-attachments/assets/147f8945-4e72-43a8-8972-699682fdcadd)

#### Task 2: Calculate Die Area in Microns.

Below are screenshots showcasing the floorplan DEF file:

![floorplandesign](https://github.com/user-attachments/assets/d7cea66e-d9b9-41f3-8bb0-a7c3712ac413)

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

### Task 3: Load Floorplan DEF in Magic Tool.

Commands to Load Floorplan DEF in Magic:
1.Change directory to path containing generated floorplan def
```bash
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/17-03_12-06/results/floorplan/
```
2.Command to load the floorplan def in magic tool
```bash
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &
```
* Floorplan Loaded in Magic:

The floorplan DEF file has been successfully loaded into Magic for visualization.

![magic open1](https://github.com/user-attachments/assets/519956f4-5e4a-4450-8bdd-9b3a02574e5e)

* Equidistant Placement of Ports:

This image showcases the equidistant placement of ports, which was configured in the config.tcl file to ensure optimal distribution.

![zoomfloorplan](https://github.com/user-attachments/assets/3f40e721-59f4-41a3-bd8a-b15b4f9c3670)

* Port Layer Configuration:

Ports are placed on the layer as defined by the config.tcl file.

![metal 3](https://github.com/user-attachments/assets/efd50323-f613-4859-8abe-bcd79ede6850)
![metal2](https://github.com/user-attachments/assets/b6a836ed-4fa0-4903-9c9d-d276b84f8d55)

* Decap Cells and Tap Cells:

This shows the placement of decap (decoupling capacitor) cells and tap cells, which are used for power distribution and ground rail management.


![decap cells](https://github.com/user-attachments/assets/396f9873-b129-4c78-b1fc-f52e1fcb4d12)



* Diagonally Equidistant Tap Cells:

The tap cells are placed in a diagonally equidistant fashion to ensure proper power and ground distribution across the chip.

![equidistence tap cell](https://github.com/user-attachments/assets/fbdc6edd-c89b-4ae9-a5d9-5827f44e201b)



* Unplaced Standard Cells at Origin:

This image shows the standard cells that are currently unplaced, located at the origin, which is typical prior to placement optimization.

![standerd cells](https://github.com/user-attachments/assets/aa16dcb8-a9be-4af8-9be8-8fbacb5696d4)

### Task 4:Run Congestion-Aware Placement.

Command to run placement
```bash
run_placement
```
* Placement Completion:


  The placement run has successfully completed, with the design placed according to congestion and timing constraints.
  
![placement2](https://github.com/user-attachments/assets/b3d78910-faa9-4311-92d7-0fed726dc86d)

### Task 5: Load Placement DEF in Magic Tool.
To load the generated placement DEF file in the Magic tool, use the following commands in a separate terminal:
Commands to Load Placement DEF in Magic
1.Navigate to the directory containing the placement DEF file:
```bash
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/17-03_12-06/results/placement/
```
2.Load the placement DEF in Magic:
```bash
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech \
lef read ../../tmp/merged.lef def read picorv32a.placement.def &
```
* Placement DEF Loaded in Magic
  
  The placement DEF file has been successfully loaded into Magic, showing the placement of standard cells and other components.

![placement](https://github.com/user-attachments/assets/a4c9133c-538f-4996-981e-3a7778b2bae6)

* Legally Placed Standard Cells
  
  This screenshot shows the standard cells placed legally according to the design rules, ensuring a functional and optimized layout.

![legally placed](https://github.com/user-attachments/assets/d9480e1d-1893-4a27-9a3c-50e492b92d4b)

Commands to exit from current run

```tcl
# Exit from OpenLANE flow
exit

# Exit from OpenLANE flow docker sub-system
exit
```

## Section 3 - Design library cell using Magic Layout and ngspice characterization 

### Implementation

Objectives: 

1.Clone the GitHub repository for the Standard Cell Design and Characterization using the OpenLANE flow: [Standard cell design](https://github.com/nickson-jose/vsdstdcelldesign).

2.Open the custom inverter layout in Magic for inspection and exploration.

3.Perform SPICE extraction of the inverter design using Magic.

4.Modify the SPICE model file for further analysis and simulation.

5.Conduct post-layout simulations using ngspice.

6.Rise and Fall Transition Time Calculations.

7.Cell Delay Calculations.

8.Correcting DRC Errors in SkyWater SKY130 Process Using Magic VLSI Layout Tool.

### Task 1: Cloning Custom Inverter Standard Cell Design from GitHub Repository
1.Navigate to the OpenLANE working directory:

```bash
cd Desktop/work/tools/openlane_working_dir/openlane
```
  2.Clone the custom inverter design repository
  ```bash
git clone https://github.com/nickson-jose/vsdstdcelldesign
```
3.Move into the repository's directory
```bash
cd vsdstdcelldesign
```
4.Copy the Magic tech file for easier access
```bash
cp ~/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech .
```
5.Verify the contents of the directory
```bash
ls
```
6.Open the custom inverter layout in Magic
```bash
magic -T sky130A.tech sky130_inv.mag &
```

Ensure that you have successfully cloned the repository and opened the custom inverter layout using Magic for further exploration. The screenshot of the commands run is also shown below.

* Screenshot:

  ![clone](https://github.com/user-attachments/assets/02aeb5f5-2dd4-4e2c-b825-4f4455d1e5f8)

### Task 2: Open the custom inverter layout in Magic for inspection and exploration.

After opening the custom inverter layout into Magic, the design exploration can be conducted by identifying critical components and connections, including NMOS, PMOS, and various signal paths.
* Steps:
  1.Load the layout in Magic:
  ```bash
  magic -T sky130A.tech sky130_inv.mag &
  ```
  2.NMOS and PMOS Identified:
  * Both NMOS and PMOS transistors were identified within the layout. Screenshots of these are provided below.

    ![nmos](https://github.com/user-attachments/assets/4729a844-f352-4b25-9b40-79412e2609ed)

    ![pmos](https://github.com/user-attachments/assets/8ede3280-0270-44a7-9073-e8906eacd4cc)

  3.Verification of Output (Y) Connectivity to PMOS and NMOS Drain:
  * The connectivity from the output Y to the PMOS and NMOS drains was checked and verified.

    ![y output](https://github.com/user-attachments/assets/a8975c27-de7a-4a42-8a24-214b5c31e9ac)
    

  4.PMOS Source Connectivity to VDD (VPWR) Verified:
  
  * The source of the PMOS was verified to be correctly connected to VDD (VPWR).

   ![vdd](https://github.com/user-attachments/assets/697efc0c-addb-4024-8fac-5cbb75b7f856)


  5.NMOS Source Connectivity to VSS (VGND) Verified:

  * The source of the NMOS was verified to be connected to VSS (VGND).
 
   ![vss](https://github.com/user-attachments/assets/a0d858c7-6cff-4bc8-a316-a5451b4fa543)


  6.Testing DRC Errors:

  * A portion of the layout was deliberately deleted to trigger a DRC (Design Rule Check) error, and the tool successfully flagged the error.
 
    ![drc error](https://github.com/user-attachments/assets/61913715-91cd-493a-86af-83400e8f28da)
    
### Task 3: Perform SPICE extraction of the inverter design using Magic.

To extract the SPICE netlist from the custom inverter layout in Magic, follow these steps in the tkcon window:

Commands for SPICE Extraction:
1.Check the current directory
```bash
pwd
```
2.Extract the layout into .ext format
```bash
extract all
```
3.Enable parasitic extraction (capacitance and resistance thresholds set to 0 for full extraction)
```bash
ext2spice cthresh 0 rthresh 0
```
4.Convert .ext file to SPICE format
```bash
ext2spice
```
Screenshots:

1.TKcon window after running extraction commands:

![extract spice](https://github.com/user-attachments/assets/5d43cd61-3f33-4392-8e5c-44c1d97831b7)

2.Generated SPICE file:

![extract spice file](https://github.com/user-attachments/assets/a3d6cf78-e031-40e1-ac32-f60bbe901a19)


### Task 4: Modify the SPICE model file for further analysis and simulation.
* Measuring Unit Distance in Layout Grid

  Before editing the SPICE model file, ensure the grid spacing and unit distance in the layout are accurate. This will help in verifying and adjusting the extracted device dimensions.

  Screenshot: Measuring unit distance in layout grid

  ![box](https://github.com/user-attachments/assets/80ee2f3f-30f6-401b-9bd0-94a9e2b4c215)


* Final Edited SPICE File for ngspice Simulation

  After extracting the SPICE file, review and edit it as needed for ngspice simulation. Ensure that the netlist includes accurate connections and the appropriate device models for the Skywater 130nm process.

* Screenshot: Edited SPICE file

![edited](https://github.com/user-attachments/assets/3dbad5c8-479f-4b1a-b2d5-d013a64f11ee)




### Task 5: Conduct post-layout simulations using ngspice.

* Running the SPICE Simulation

  To run a post-layout simulation in ngspice, use the following commands to load the SPICE file and plot the output.

  1.Load the SPICE file into ngspice
  ```bash
  ngspice sky130_inv.spice
  ```
  2.Plot output y vs time a (input)
  ```bash
  plot y vs time a
  ```


* Screenshot: Running the ngspice simulation


  
![ngspice](https://github.com/user-attachments/assets/ccdd6206-2f49-4069-a8be-29174eebdbe7)


![plot](https://github.com/user-attachments/assets/6abc4893-da8c-4f58-b3ff-3aba8245c31d)


  
* Screenshot: Generated plot

  
![graph1](https://github.com/user-attachments/assets/56d9f6d4-bf24-4601-9dd2-438c91795873)



### Task 6:  Rise and Fall Transition Time Calculations:

1. Rise Transition Time Calculation:

```math
Rise\ transition\ time = Time\ taken\ for\ output\ to\ rise\ to\ 80\% - Time\ taken\ for\ output\ to\ rise\ to\ 20\%
```
```math
20\%\ of\ output = 660\ mV
```
```math
80\%\ of\ output = 2.64\ V
```

* Screenshots: 20% Output

![rise20](https://github.com/user-attachments/assets/d2fa1c0b-ddcc-497b-b422-61307910e4c1)

![rise20terminal](https://github.com/user-attachments/assets/a4c0d720-2e34-4720-8c4e-5c0065610020)



* Screenshots: 80% Output


![rise80](https://github.com/user-attachments/assets/c9b1d67d-5d08-4c98-8afe-9323ec508834)

![rise80terminal](https://github.com/user-attachments/assets/a66d15b6-44d9-407c-9d74-3f967caaccb6)




```math
Rise\ transition\ time = 2.24592 - 2.18213 = 0.06376\ ns = 63.76\ ps
```

2. Fall Transition Time Calculation:

```math
Fall\ transition\ time = Time\ taken\ for\ output\ to\ fall\ to\ 20\% - Time\ taken\ for\ output\ to\ fall\ to\ 80\%
```
```math
20\%\ of\ output = 660\ mV
```
```math
80\%\ of\ output = 2.64\ V
```

* Screenshots: 20% Output

![fall20](https://github.com/user-attachments/assets/b0f21670-69b3-4a2b-a203-86ce7afb4c5c)

![fall20terminal](https://github.com/user-attachments/assets/c51a5ea2-7160-470c-b63a-053d70842746)



* Screenshots: 80% Output

  ![fall80](https://github.com/user-attachments/assets/f117d411-271d-4aad-8d9b-ce4d0443bd72)

  ![fall80terminal](https://github.com/user-attachments/assets/54fe2768-a796-42d5-80b8-ac23719939b9)


```math
Fall\ transition\ time = 4.09507 - 4.05269 = 0.04238\ ns = 42.38\ ps
```

### Task 7:  Cell Delay Calculations:

1.Rise Cell Delay Calculation:

```math
Rise\ Cell\ Delay = Time\ taken\ for\ output\ to\ rise\ to\ 50\% - Time\ taken\ for\ input\ to\ fall\ to\ 50\%
```
```math
50\%\ of\ 3.3\ V = 1.65\ V
```
* Screenshots: 50% Output

![rise50](https://github.com/user-attachments/assets/90eed95b-f9c6-4296-8e93-fcb8cc489ced)

![rise50terminal](https://github.com/user-attachments/assets/83612071-ac89-4a53-a43a-ddfde1f1bc6c)

![rise50terminal2](https://github.com/user-attachments/assets/c7bb50c6-fd17-4655-b179-251eadf4e55e)

   

```math
Rise\ Cell\ Delay = 2.21109 - 2.15 = 0.06109\ ns = 61.09\ ps
```

2. Fall Cell Delay Calculation:
    
```math
Fall\ Cell\ Delay = Time\ taken\ for\ output\ to\ fall\ to\ 50\% - Time\ taken\ for\ input\ to\ rise\ to\ 50\%
```
```math
50\%\ of\ 3.3\ V = 1.65\ V
```

* Screenshots: 50% Output

![fall50](https://github.com/user-attachments/assets/5a5b633c-a38d-4726-9241-39533c476307)

![fall50terminal](https://github.com/user-attachments/assets/4a83f82d-4c64-447e-874d-351c5710c390)

  
```math
Fall\ Cell\ Delay = 4.07765 - 4.05 = 0.02765\ ns = 27.65\ ps
```

### Task 8: Correcting DRC Errors in SkyWater SKY130 Process Using Magic VLSI Layout Tool

1.Download and Extract the DRC Test Files

Start by downloading the required test files and extracting them in your home directory.

  1. Change to the Home Directory:

     ```bash
     cd
     ```
  2. Download the Lab Files:

     ```bash
     wget http://opencircuitdesign.com/open_pdks/archive/drc_tests.tgz\
     ```
  3. Extract the Compressed Files:

     ```bash
     tar xfz drc_tests.tgz
     ```
  4. Navigate to the Lab Directory:

     ```bash
     cd drc_tests
     ```
  5. List the Contents of the Directory:
     ```bash
     ls -al
     ```
  
  

  * Screenshots of Commands Executed:

    ![magic_commands](https://github.com/user-attachments/assets/48eef415-2a62-4459-818a-fa5ccbeb127f)

2.View the .magicrc Configuration File

Before proceeding, it’s a good idea to inspect the .magicrc file to ensure that Magic is configured properly.

  * Viewing the .magicrc File:

    
     ```bash
     # Open the .magicrc file in a text editor
     gvim .magicrc
     ```

    ![magicfile](https://github.com/user-attachments/assets/b597ceb6-0454-4926-ac93-76c14bd5c205)

3.Open the Magic Tool with the Updated Tech File

Start Magic in a better graphics mode to view the design and identify DRC violations.

```bash
# Open Magic in XR mode
magic -d XR &
```

4.Fixing Incorrectly Implemented poly.9 Rule

The poly.9 rule defines a minimum spacing rule that was not properly enforced. We will update the DRC rule and verify the correction.

Screenshot of poly rules:


![poly 9 rule](https://github.com/user-attachments/assets/a692e665-8d3d-4d9f-8c06-030f2a905698)

Problem: No DRC violation occurred for spacing < 0.48u as required by poly.9.


![poly 9 error](https://github.com/user-attachments/assets/c653abde-0a3c-4055-ad20-37815b3a2537)

Solution: Update the Sky130 DRC tech file.

Inserting New Commands into sky130A.tech File:

open sky130A.tech file

```bash
gvim sky130A.tech
```

Screenshot after Inserting new commands into sky130A.tech file


![edited poly 9 1](https://github.com/user-attachments/assets/0b9d6289-2db0-49aa-a08e-54903e47e76d)


![edited poly 9 2](https://github.com/user-attachments/assets/97dc33f5-92e3-4d2c-b8e9-23e163678f42)

Commands to Run:

```tcl
# Load the updated tech file
tech load sky130A.tech
```
```tcl
# Run DRC check
drc check

```
```tcl
# Select region with errors and view messages
drc why
```

Post-fix Screenshot:

![poly 9 corrected](https://github.com/user-attachments/assets/df147bb8-70c9-47bd-945f-cf37685c2ea9)

5.Fixing Incorrectly Implemented difftap.2 Rule

The difftap.2 rule was improperly implemented, with no DRC violation for spacing < 0.42u.

Screenshot of difftap rules:

![difftap 2](https://github.com/user-attachments/assets/d9bde2af-03a0-46d6-9d4f-5f6669ecc7a8)

Problem: No violation occurred for difftap spacing < 0.42u.

![difftap error](https://github.com/user-attachments/assets/82625354-1267-45f5-bb10-6320d79d90ee)

Solution: Update the Sky130 DRC tech file.

Inserting New Commands into sky130A.tech File:

![edited difftap 2](https://github.com/user-attachments/assets/04840728-0ba4-4b31-9dc6-94145be686e8)

Commands to Run:

```tcl
# Load the updated tech file
tech load sky130A.tech
```
```tcl
# Run DRC check
drc check

```
```tcl
# Select region with errors and view messages
drc why
```

Post-fix Screenshot:

![difftap corrected](https://github.com/user-attachments/assets/ede865a5-c634-4b5f-8108-8af527ffc36b)

6.Fixing Incorrectly Implemented nwell.4 Rule

The nwell.4 rule, which enforces the presence of a tap within the nwell, was also improperly implemented.

Screenshot of nwell rules:

![nwell 4 rule](https://github.com/user-attachments/assets/55d3fec3-0227-44c0-a70b-73ca6ecf8131)

Problem: No DRC violation occurred even though a tap was missing from the nwell.


![nwell error](https://github.com/user-attachments/assets/ce14dc30-1fe4-4633-afa1-b433bdcd8efd)



Solution: Update the Sky130 DRC tech file.

Inserting New Commands into sky130A.tech File:

![edited nwel 1](https://github.com/user-attachments/assets/65398b05-5cf4-4e76-9a49-200181c51eab)

![edited nwel 2](https://github.com/user-attachments/assets/79c55f1a-65cd-4713-92df-df7be286a766)


Commands to Run:

```tcl
# Load the updated tech file
tech load sky130A.tech
```

```tcl
# Change drc style to drc full
drc style drc(full)
```

```tcl
# Run DRC check
drc check

```
```tcl
# Select region with errors and view messages
drc why
```

Post-fix Screenshot:

![corrected nwell](https://github.com/user-attachments/assets/40f87039-fefe-46ed-8cbe-113875dc8314)


## Section 4 - Pre-layout timing analysis and importance of good clock tree.

### Implementation

Objectives:

1.Fix minor DRC errors and verify the design for flow insertion.

2.Save the finalized layout with a custom name and open for verification.

3.Generate the LEF file from the layout.

4.Copy the LEF and required library files to the 'picorv32a/src' directory.

5.Edit config.tcl to update the library file and add the new LEF into OpenLane flow.

6.Run OpenLane synthesis with the newly inserted custom inverter cell.

7.Address any violations caused by the custom cell by adjusting design parameters.

8.Run floorplanning and placement, verifying the custom cell's integration in the PnR flow.

9.Perform post-synthesis timing analysis using the OpenSTA tool.

10.Apply timing ECO fixes to resolve any timing violations.

11.Replace the old netlist with the updated netlist and implement floorplan, placement, and CTS.

12.Conduct post-CTS timing analysis using OpenROAD.

13.Optimize post-CTS timing by removing sky130_fd_sc_hd__clkbuf_1 from CTS_CLK_BUFFER_LIST.



### Task 1: Fix minor DRC errors and verify the design for flow insertion.

Conditions to Verify:
  1. Conditiion 1:
     * Input and output ports must lie on the intersection of vertical and horizontal tracks.
  2. Condition 2:
     * The width of the standard cell should be an odd multiple of the horizontal track pitch.
  3. Condition 3:
     * The height of the standard cell should be an even multiple of the vertical track pitch.

Commands to Open the Layout in Magic

```bash
# Change directory to the vsdstdcelldesign folder
cd Desktop/work/tools/openlane_working_dir/openlane/vsdstdcelldesign
```

```bash
# Open the custom inverter layout in Magic
magic -T sky130A.tech sky130_inv.mag &
```

Track Information of sky130_fd_sc_hd

* The track information for the standard cell library sky130_fd_sc_hd is shown below. This will be used to verify the alignment and dimensions of the custom inverter cell.

![track info](https://github.com/user-attachments/assets/22a83be2-21f6-439b-856d-4142d32dc7ec)

Setting Grid for Track Alignment

* Use the following commands in the tkcon window to set the grid according to the standard cell track pitch.

```tcl
# Get syntax for the grid command
help grid
```
```tcl
# Set grid values for track alignment (locali layer)
grid 0.46um 0.34um 0.23um 0.17um
```

Screenshot of Command Execution

![grid](https://github.com/user-attachments/assets/efea418d-51e0-4f8b-aa3f-f87e92cd1cb1)

Condition 1: Port Alignment on Tracks

* Verified that the input and output ports of the custom inverter cell are aligned at the intersections of vertical and horizontal tracks.

![cond 1](https://github.com/user-attachments/assets/6901533f-ff0f-4375-bbac-0427bd499c0e)

Condition 2: Width Verification
* The width of the standard cell should be an odd multiple of the horizontal track pitch 0.46𝜇𝑚

Horizontal track pitch=0.46 umWidth of standard cell=1.38 um=0.46×3

This condition is satisfied as the width is an odd multiple.

![con 2](https://github.com/user-attachments/assets/f81e8553-3b68-4ad2-8cf7-08c061ea1b73)

Condition 3: Height Verification

* The height of the standard cell should be an even multiple of the vertical track pitch 0.34𝜇𝑚

Vertical track pitch=0.34 umHeight of standard cell=2.72 um=0.34×8

This condition is satisfied as the height is an even multiple.

![con 3](https://github.com/user-attachments/assets/6a261b54-ebd3-4af0-9496-1081579734e4)

### Task 2: Save the finalized layout with a custom name and open for verification.

* Once the layout has been verified and finalized, follow the steps below to save it with a custom name and reopen it for verification.

Command to Save the Layout with a Custom Name in Magic (tkcon Window):

```tcl
# Command to save layout as a new file
save sky130_vsdinv.mag
```

![changing  file name](https://github.com/user-attachments/assets/e46405b6-01da-41f2-b7f7-0c109efc2138)

Command to Open the Newly Saved Layout in Magic:

```bash
# Open the custom inverter layout in Magic
magic -T sky130A.tech sky130_vsdinv.mag &
```

Screenshot of the Newly Saved Layout:

![newly created  file open](https://github.com/user-attachments/assets/5b72d7dd-0771-438d-8270-4c0021b5251b)


### Task 3: Generate the LEF file from the layout.

* After finalizing the custom inverter layout, you can generate the LEF (Library Exchange Format) file, which is essential for integrating the cell into the design flow.

  Command to Write the LEF File in Magic (tkcon Window):

  ```tcl
  # Command to generate LEF
  lef write
  ```

  Screenshot of the Command Execution:

  ![lef write](https://github.com/user-attachments/assets/c6af18df-23b9-4c53-b0ae-825b43ac61b2)

  Screenshot of the Newly Created LEF File:

  ![lef file](https://github.com/user-attachments/assets/9e5906dc-d24a-4d27-8ad2-fe79a122956f)

  

### Task 4: Copy the LEF and required library files to the 'picorv32a/src' directory.

* After generating the LEF file, the next step is to copy it along with the required library files to the picorv32a design's src directory for integration.

  Commands to Copy LEF and Library Files:

  ```bash
  # Copy the LEF file to the 'picorv32a/src' directory
  cp sky130_vsdinv.lef ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/
  ```
  ```bash
  # List and check if the LEF file is copied successfully
  ls ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/
  ```
  ```bash
  # Copy the required Liberty (.lib) files
  cp libs/sky130_fd_sc_hd__* ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/
  ```
  ```bash
  # List and check if the .lib files are copied successfully
  ls ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/
  ```

  Screenshot of Command Execution:

  ![lef copy commands](https://github.com/user-attachments/assets/340e8c98-a708-44aa-aa58-d00f0e0130dd)


### Task 5: Edit config.tcl to update the library file and add the new LEF into OpenLane flow.

* To ensure that the custom inverter cell is included in the OpenLane flow, update the config.tcl file by specifying the correct library files and adding the new LEF file.

Commands to Add to config.tcl:

```tcl
# Set the paths for the new library files
set ::env(LIB_SYNTH) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__typical.lib"
set ::env(LIB_FASTEST) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__fast.lib"
set ::env(LIB_SLOWEST) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__slow.lib"
set ::env(LIB_TYPICAL) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__typical.lib"

# Include the custom LEF file
set ::env(EXTRA_LEFS) [glob $::env(OPENLANE_ROOT)/designs/$::env(DESIGN_NAME)/src/*.lef]

```
Screenshot of the Edited config.tcl:

![config file](https://github.com/user-attachments/assets/b5dbcc62-a81f-4741-a390-015ca6002a07)


### Task 6: Run OpenLane synthesis with the newly inserted custom inverter cell.

* To synthesize the design with the custom inverter cell included, follow the steps below.

Commands to Run OpenLANE Flow Synthesis:

```bash
# Change directory to the OpenLANE flow working directory
cd Desktop/work/tools/openlane_working_dir/openlane
```
```bash
# Invoke the OpenLANE Docker subsystem
docker
```
```tcl
# Enter Interactive Mode in OpenLANE flow
./flow.tcl -interactive

```
```tcl
# Load required packages
package require openlane 0.9
```
```tcl
# Prepare the 'picorv32a' design
prep -design picorv32a
```
```tcl
# Include the newly added LEF file into the OpenLANE flow
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs
```
```tcl
# Run synthesis for the design
run_synthesis
```

Screenshots of Commands Execution:

![day 4 synthesis 2](https://github.com/user-attachments/assets/ac6bdbc5-af28-4cde-bb56-a53240864032)

### Task 7: Address any violations caused by the custom cell by adjusting design parameters.

* After adding the custom inverter cell, the design may introduce timing violations. Here’s how to modify design parameters to improve timing.

  Initial Design Values Before Modifications:

  Screenshots of initial values:

  ![initial vallues 1 before](https://github.com/user-attachments/assets/7e55c198-43ff-4f19-99cd-c1fbcd6e6e39)

  ![initial vallues 2 before](https://github.com/user-attachments/assets/09a87bec-2b18-491f-8b16-8209b9fa1c8b)

  Commands to Modify Parameters and Improve Timing:

  ```tcl
  # Prep the design again to update variables
  prep -design picorv32a -tag 15-9_06-37 -overwrite
  ```
  ```tcl
  # Include newly added lef into the flow
  set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
  add_lefs -src $lefs
  ```
  ```tcl
  # Display current value of SYNTH_STRATEGY
  echo $::env(SYNTH_STRATEGY)
  ```
  ```tcl
  # Set new value for SYNTH_STRATEGY
  set ::env(SYNTH_STRATEGY) "DELAY 3"
  ```
  ```tcl
  # Check if SYNTH_BUFFERING is enabled
  echo $::env(SYNTH_BUFFERING)
  ```
  ```tcl
  # Check current value of SYNTH_SIZING
  echo $::env(SYNTH_SIZING)
  ```
  ```tcl
  # Set new value for SYNTH_SIZING
  set ::env(SYNTH_SIZING) 1

  ```
  ```tcl
  # Check the current SYNTH_DRIVING_CELL
  echo $::env(SYNTH_DRIVING_CELL)
  ```
  ```tcl
  # Run synthesis again after updating parameters
  run_synthesis
  ```

  Result of the Synthesis:

  * Custom inverter cell is now included in the design.
  * Screenshot of the merged.lef file in the tmp directory confirming the custom inverter as a macro:
 
![merged lef file](https://github.com/user-attachments/assets/96770974-03d4-4b31-b8ac-ac8aee5482c4)


Screenshots of Commands Execution:

![day 4 synthesis 1 after](https://github.com/user-attachments/assets/42e2d30b-d502-4a5f-8a63-4f679e9efef0)

![day 4 synthesis 2 after](https://github.com/user-attachments/assets/a9a4520e-881c-4bda-9d59-243b621f32f9)


Comparing Results After Modifications:

* Area: Increased slightly.
* Worst Negative Slack: Improved, now at 0.

  ![initial vallues 1 after](https://github.com/user-attachments/assets/583f2c89-4fb1-4f1c-ae57-a98d6864bca9)

  ![initial vallues 2 after](https://github.com/user-attachments/assets/f14928c9-c8e3-4ddc-b220-3c87f910c545)


### Task 8: Run floorplanning and placement, verifying the custom cell's integration in the PnR flow.

* Once the custom inverter cell has been accepted during synthesis, we proceed with floorplanning and placement in the PnR flow.

Floorplan Command
Run the run_floorplan command:
```tcl
# Command to run floorplan
run_floorplan
```
However, encountering errors while using this command led us to use the following individual commands to manually initiate the floorplan process:

```tcl
# Initialize floorplan
init_floorplan
```
```tcl
# Place I/O pins
place_io
```
```tcl
# Add tap and decap cells
tap_decap_or
```
Floorplan Command Execution Screenshots:
    * Running floorplan:
    ![day 4 floorplan error](https://github.com/user-attachments/assets/75303d3c-4e67-4f99-9462-2b041033623d)
    * Manually running the individual commands due to the error:
    ![commands to clear floorplan error in day 4](https://github.com/user-attachments/assets/69d1fe8d-cd41-4c93-8c85-0b875cd41250)

Placement Command
*  Once the floorplan is complete, we proceed with placement using the following command:

   ```tcl
   # Command to run placement
    run_placement
   ```

   Placement Command Execution Screenshots:

   * Placement process:

     ![day 4 placement done](https://github.com/user-attachments/assets/0cebb282-3b30-4e69-a9a7-821fb749f97e)


  Load Placement DEF in Magic for Visualization
  * To verify that the custom inverter cell has been accepted into the flow and placed correctly, we load the generated placement DEF into Magic.

  ```bash
  # Change directory to placement result folder
  cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/15-9_06-37/results/placement/
  ```
  ```bash
  # Load placement def in magic
  magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &
  ```
Magic Visualization Screenshots:
* Viewing the placement DEF in Magic:

  ![day 4 magic open](https://github.com/user-attachments/assets/adac3418-f39b-4f83-b30a-44e48f0381ed)

* Zooming in on the custom inverter with proper abutment:

  ![zoom to inverter](https://github.com/user-attachments/assets/59240d31-63b6-4e61-8fc9-7da232da460d)

View Internal Layers of Cells

* We use the expand command in Magic to view internal connectivity layers of the placed cells, ensuring correct abutment.

  ```tcl
  # Command to view internal connectivity layers
  expand
  ```
  Abutment Verification Screenshots:

  * Custom inverter cell properly abutting power pins with other cells from the library:

    ![expand](https://github.com/user-attachments/assets/d6c1abb5-09bc-492e-8331-927d6780b813)


### Task 9: Perform post-synthesis timing analysis using the OpenSTA tool.




    


  



    

  












































 


 


 









