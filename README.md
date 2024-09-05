# VSD_WORKSHOP


> 2 Week digital VLSI SoC design and planning workshop with complete RTL2GDSII flow organised by VSD in collaboration with NASSCOM.
>
## Section 1 - Inception of open-source EDA, OpenLANE and Sky130 PDK (04/09/2024 - 05/09/2024)

### Theory
<details>
  <summary>
Expand or Collapse
  </summary>

#### Components

* In every embedded board we've seen, the only component we think of as the chip is the ***PACKAGE*** of the chip, which is essentially a packet bound over a protective layer. The manufactured chip is typically located in the middle of a package, and connections from the package are fed to the chip using the ***WIRE BOUND*** method, which is just a standard wired connection.

![board pcb](https://github.com/user-attachments/assets/413f639b-0bfa-4a4d-bb38-2ad70941f969)
![image](https://github.com/user-attachments/assets/3a52344a-d24a-4e09-90c2-3dd922c9e7dc)
![image](https://github.com/user-attachments/assets/b361e263-28a1-4f4a-89fe-6d10127648ed)

#### Chip

* Looking inside the chip now, we can see that PADS is responsible for passing all signals from the outside world to the chip and vice versa. The entirety of the chip's digital logic is located in CORE, the region enclosed by the pads. The DIE, or basic integrated electronics assembly, is made up of both the core and the pads.

![image](https://github.com/user-attachments/assets/ababd03f-0356-4b19-bf41-5292dc55b670)

* The location where semiconductor chips are made is known as the "FOUNDRY***. Repeatable digital logic blocks are known as "MACROS***," but "FOUNDRY IP's*** are intellectual properties based on a particular foundry that require a certain level of intelligence to produce.

![image](https://github.com/user-attachments/assets/ada21d92-b055-47f0-8899-ae96fd6cc8d1)



#### ISA (Intruction Set Architecture)

* The C program is compiled into assembly language using the RISC-V ISA, which is a human-readable intermediate step between high-level code and machine language.
* The assembly code is converted into machine language (binary 0s and 1s) that is directly understood by the hardware.
* The RISC-V specification is implemented using RTL (Hardware Description Language like Verilog or VHDL), defining how the processor handles data and executes instructions.
* The RTL design is synthesized into a physical layout using Place and Route (PnR), ultimately generating the GDSII file for chip manufacturing.

![image](https://github.com/user-attachments/assets/267f3a4c-b619-46af-8f1c-8b2db2bbfd82)


* When an application is executed, it goes through the system software, which converts the application program into binary code that hardware understands. The major components of system software include the Operating System (OS), Compiler, and Assembler.
* The Operating System generates small functions or routines in high-level languages such as C, C++, VB, or Java, based on the application’s needs and its interaction with the hardware.
* The compiler takes the high-level language code and converts it into hardware-specific instructions. The syntax of these instructions varies based on the underlying architecture (e.g., x86, ARM, RISC-V).
* The assembler then translates the hardware-specific instructions into machine language (binary code). This binary code is then passed to the hardware, which executes the instructions to perform the required tasks.

![image](https://github.com/user-attachments/assets/2e380647-081c-4873-9602-e95cc56177ec)


* For example, a stopwatch app on a RISC-V core, the OS outputs a small C function, which the compiler converts into RISC-V instructions; the assembler then translates these instructions into binary code that is executed by the hardware on the chip.

![image](https://github.com/user-attachments/assets/6275ecd5-410c-47ea-bd25-3114c87978d9)


* For the above stopwatch the following are the input and output of the compiler and assembler.

![image](https://github.com/user-attachments/assets/3182aa7c-19f0-4246-974e-415057b00a0f)


*In this process, the compiler outputs instructions, and the assembler converts these instructions into binary patterns. The next step involves using RTL (a Hardware Description Language like Verilog or VHDL) to design hardware that understands and implements these instructions. This RTL is then synthesized into a netlist (a representation of logic gates and their connections), which is further processed through physical design implementation to fabricate the chip.

![image](https://github.com/user-attachments/assets/5f87f002-7c00-4eb9-b352-70edd034e3d1)


* There are mainly 3 different parts in this course. They are:
1. RISC-V ISA
2. RTL and synthesis of RISC-V based CPU core - picorv32
3. Physical design implementation of picorv32

![image](https://github.com/user-attachments/assets/a00c78ff-2652-4f32-893e-741b6ef6966c)


#### Open-source Implementation

* To implement open-source ASIC designs, three key enablers are required in open-source form:
1. RTL Designs
2. EDA Tools
3. PDK Data

* Initially, integrated circuit (IC) design and fabrication were tightly coupled and controlled by a few companies, such as TI and Intel.
* Lynn Conway and Carver Mead introduced structured design methodologies in 1979, separating IC design from fabrication. This innovation, based on λ-based design rules, was published in the landmark VLSI book "Introduction to VLSI Systems," initiating VLSI education.
* This methodology led to the rise of Fabless Companies (design-only) and Pure Play Fabs (fabrication-only). The interface between designers and fabs became the Process Design Kit (PDK), which includes essential information like device models, design rules, and standard cell libraries.
* Traditionally, PDKs were distributed under Non-Disclosure Agreements (NDAs), making them inaccessible to the public. However, in June 2020, Google and SkyWater Technology released the first-ever open-source PDK for SkyWater's 130nm process, marking a significant step toward open-source ASIC development.

![image](https://github.com/user-attachments/assets/446f0481-f25e-45c8-8ebb-f7df53bdaeba)



* ASIC design involves a complex flow that integrates various methodologies and EDA tools into a unified software framework to efficiently carry out the entire design and implementation process.

![image](https://github.com/user-attachments/assets/d4dc30a3-e16c-4372-aa80-127fd2102ee1)



#### OpenLANE Open-source ASIC Design Implementation Flow

* The main objective of the ASIC Design Flow is to take the design from the RTL (Register Transfer Level) through various stages of synthesis, verification, and physical design, culminating in the generation of the GDSII file for final chip fabrication.

![image](https://github.com/user-attachments/assets/5555cad2-6edb-4c2d-90f9-bb89deb08513)


* Synthesis is the process of converting RTL (Register Transfer Level) design into circuits made from Standard Cell Libraries (SCL), resulting in a description in HDL known as the Gate-Level Netlist.
* Gate-Level Netlist is functionally equivalent to the RTL.

![image](https://github.com/user-attachments/assets/5b748b1b-0d96-4608-b5c2-3048aada9fd1)



* The fundemental building blocks which are the standard cells have regular layouts.
* Each cell has different views/models which are utilised by different EDA tools like liberty view with electrical models of the cells, HDL behavioral models, SPICE or CDL views of the cells, Layout view which include GDSII view which is the detailed view and LEF view which is the abstract view.

![image](https://github.com/user-attachments/assets/c365e7dd-c8a5-4b8d-b791-2dfdbabb9b60)



* Chip Floor Planning

![image](https://github.com/user-attachments/assets/293d4181-8965-448d-a5f4-80bebdc8e98f)



* Macro Floor Planning

![image](https://github.com/user-attachments/assets/88ddfb8e-49f6-4fe8-84a2-3c763d8d32c3)



* Power Planning typically uses upper metal layers for power distribution since thay are thicker than lower metal layers and so have lower resistance and PP is done to avoid electron migration and IR drops.

![image](https://github.com/user-attachments/assets/551649f8-09e1-471c-960a-64c1bce98266)


* Placement

![image](https://github.com/user-attachments/assets/0e6ddad2-0007-4602-8dd0-8b206cf64bdd)



* Global placement provide approximate locations for all cells based on connectivity but in this stage the cells may be overlapped on each other and in detailed placement the positions obtained from global placements are minimally altered to make it legal (non-overlapping and in site-rows)

![image](https://github.com/user-attachments/assets/fb6ec6f1-f0f4-4cec-aaf3-48305422a7a8)



* Clock Tree Synthesis

![image](https://github.com/user-attachments/assets/4d9315b8-6b0f-4dac-a3dd-9d835f1d1770)



* Clock skew is the time difference in arrival of clock at different components.
* Routing

![image](https://github.com/user-attachments/assets/71b19c91-897b-4322-a1f9-012387ff3664)



* skywater PDK has 6 routing layers in which the lowest layer is called the local interconnect layer which is a Titanium Nitride layer the following 5 layers are all Aluminium layers.

![image](https://github.com/user-attachments/assets/4de699b1-a357-423a-8546-3f23cec613e9)



* Global and Detailed Routing

![image](https://github.com/user-attachments/assets/b89cf2ea-43fe-4db8-8050-1f8313fe5d2d)



* Once done with the routing the final layout can be generated which undergoes various Sign-Off checks.
* Design Rules Checking (DRC) which verifies that the final layout honours all design fabrication rules.
* Layout Vs Schematic (LVS) which verifies that the final layout functionality matches the gate-level netlist that we started with.
* Static Timing Analysis (STA) to verify that the design runs at the designated clock frequency.

![image](https://github.com/user-attachments/assets/09205638-2bdb-450e-9397-f4f0e3e08011)


</details>


### Implementation

Section 1 tasks:- 
1. Run 'picorv32a' design synthesis using OpenLANE flow and generate necessary outputs.
2. Calculate the flop ratio.

```math
Flop\ Ratio = \frac{Number\ of\ D\ Flip\ Flops}{Total\ Number\ of\ Cells}
```
```math
Percentage\ of\ DFF's = Flop\ Ratio * 100
```

* All section 1 logs, reports and results can be found in following run folder:











  



#### 1. Run 'picorv32a' design synthesis using OpenLANE flow and generate necessary outputs.

Commands to invoke the OpenLANE flow and perform synthesis

```bash
# Change directory to the OpenLANE flow directory
cd Desktop/work/tools/openlane_working_dir/openlane

# Set up Docker alias (uncomment and run this command if needed)
# alias docker='docker run -it -v $(pwd):/openLANE_flow -v $PDK_ROOT:$PDK_ROOT -e PDK_ROOT=$PDK_ROOT -u $(id -u $USER):$(id -g $USER) efabless/openlane:v0.21'

# Start the Docker container
docker
```
```tcl
# Enter the OpenLANE flow interactive mode
./flow.tcl -interactive

# Load the required OpenLANE package
package require openlane 0.9

# Prepare the design 'picorv32a'
prep -design picorv32a

# Perform synthesis
run_synthesis

# Exit the OpenLANE flow
exit

```

Screenshots of running each commands
![day1 preparation complete](https://github.com/user-attachments/assets/1e7aac7a-8d80-4bba-9e66-a72410bfaf52)
![day 1 synthesis](https://github.com/user-attachments/assets/b079e940-8e0a-4421-94f3-f5c791d9a4a5)

#### 2. Calculate the flop ratio.

Screenshots of synthesis statistics report file with required values highlighted

![d1s](https://github.com/user-attachments/assets/2197a467-a2ec-4aec-a87b-2759414b1fd6)


Calculation of Flop Ratio and DFF % from synthesis statistics report file

```math
Flop\ Ratio = \frac{1613}{14876} = 0.108429685
```
```math
Percentage\ of\ DFF's = 0.108429685 * 100 = 10.84296854\ \%
```



