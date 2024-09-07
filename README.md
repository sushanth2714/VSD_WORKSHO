# VSD_WORKSHOP


> 2 Week digital VLSI SoC design and planning workshop with complete RTL2GDSII flow organised by VSD in collaboration with NASSCOM.
>
## Section 1 - Inception of open-source EDA, OpenLANE and Sky130 PDK (04/09/2024 - 05/09/2024)




### Implementation

Objectives: 
1. Run 'picorv32a' design synthesis using OpenLANE flow and generate necessary outputs.
2. Calculate the flop ratio based on synthesis results.


#### Task 1: Running 'picorv32a' Design Synthesis using OpenLANE

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

#### Task 2: Calculating Flop Ratio
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

## Section 2 - Good Floorplan vs Bad Floorplan and Introduction to Library Cell (06/09/2024 - 07/09/2024)

### Implementation
Objectives: 
1.Run 'picorv32a' Design Floorplan Using OpenLANE.
2.Calculate Die Area in Microns.
3.Load Floorplan DEF in Magic Tool.
4.Run Congestion-Aware Placement.
5.Load Placement DEF in Magic Tool.

#### Task 1: Run 'picorv32a' Design Floorplan Using OpenLANE.

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

#### Task 3: Load Floorplan DEF in Magic Tool.

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

#### Task 4:Run Congestion-Aware Placement.

Command to run placement
```bash
run_placement
```
* Placement Completion:


  The placement run has successfully completed, with the design placed according to congestion and timing constraints.
  
![placement2](https://github.com/user-attachments/assets/b3d78910-faa9-4311-92d7-0fed726dc86d)

#### Task 5: Load Placement DEF in Magic Tool.
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












 


 


 









