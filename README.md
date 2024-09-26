# VSD-PHYSICAL-DESIGN-SOC-DESIGN-PROGRAM

## Table of Contents
- [Day - 1 Introduction of Open-Source EDA, OpenLane and Sky130 PDK](#day---1-Introduction-of-Open-Source-EDA-OpenLane-and-Sky130-PDK)
- [Day - 2 Good floorplaning vs Bad floorplaning and introduction to library cells](#day---2-Good-floorplaning-vs-Bad-floorplaning-and-introduction-to-library-cells)

### Overview Of QFN-48 Chip (PicoRV32 - A Size-Optimized RISC-V CPU)
VSD Squadron Board: The VSD Board is shown below. Our focus is on the enclosed region containing the "Microprocessor (PicoRV32A-Cpu)," which will be designed using the RTL to GDS flow, progressing from the abstract design level to the fabrication stage.

![image](https://github.com/user-attachments/assets/844ea7fc-f4f4-4353-ae5d-d75a4507d075)


## Day - 1 Introduction of Open-Source EDA, OpenLane and Sky130 PDK


### OPENLANE 


#### Key Features of OpenLane:
1. **RTL to GDSII Flow:**- OpenLane provides a flow that takes RTL as input and produces GDSII (Graphic Design System II), which is the standard file format for the final layout of the chip.

![image](https://github.com/user-attachments/assets/7a7f032d-7d7b-46f5-a4f7-2c72c91f2400)

2. **Open-Source Tools:** - It integrates several open-source tools such as Yosys (for synthesis), OpenSTA (for static timing analysis), Magic (for layout), KLayout (for layout visualization), and many others.
3. **Technology-Independent:**- OpenLane is designed to work with various technology nodes, though it is primarily associated with the SkyWater 130nm PDK (Process Design Kit), which is also open-source.
4. **Customizable Flow:**- Users can customize the flow according to their design needs, with the ability to tweak various parameters and stages of the design process.
5. **Tape-Out Ready:**- The flow is designed to produce results that are ready for tape-out, meaning the design can be sent to a foundry for manufacturing.
#### Typical Flow in OpenLane:
1. **Synthesis:**- Converting RTL code to a gate-level netlist.

 ![image](https://github.com/user-attachments/assets/2c619ac7-5117-48aa-92f1-516a3f191dc1)

2. **Floorplanning:**- Defining the layout of the chip, including the placement of blocks and the arrangement of IOs.
 ![image](https://github.com/user-attachments/assets/3581a3b6-24aa-49e8-8632-5a3e8de32f74)

3. **Placement:**- Placing the synthesized gates on the chip floorplan.
 ![image](https://github.com/user-attachments/assets/4d947db7-68e4-40ae-ad60-fabf697abd43)

4. **Clock Tree Synthesis (CTS):**- Designing a clock distribution network to meet timing requirements.

   
 ![image](https://github.com/user-attachments/assets/64904d1c-1c2e-4820-ba7c-e2d86367aa7e)

5. **Routing:**- Creating the metal connections between gates and blocks.
 ![image](https://github.com/user-attachments/assets/2dd57219-bae8-4afd-b4f8-54d695ed71f0)

6. **Signoff:**- Performing final checks, including static timing analysis, design rule checks (DRC), and layout versus schematic (LVS) checks. 
7. **GDSII Export:**-Generating the final layout file that can be used for chip fabrication.
OpenLane is significant because it democratizes access to ASIC design, allowing researchers, startups, and hobbyists to design and potentially manufacture custom silicon without relying on expensive proprietary tools.
### Process Design Kit (PDK)
A Process Design Kit (PDK) is a collection of resources provided by semiconductor foundries to help chip designers create integrated circuits (ICs) that are compatible with a specific manufacturing process. It includes detailed information about the process technology, such as design rules, standard cell libraries, simulation models, and technology files that describe how the physical layers of the chip should be constructed.
### Role Of PDK In ASIC Design
The Process Design Kit (PDK) plays a crucial role in ASIC (Application-Specific Integrated Circuit) design, serving as the bridge between the semiconductor manufacturing process and the design tools used by engineers to create custom integrated circuits. 
#### Key Roles of PDK in ASIC Design:
##### 1. Technology Representation:
- The PDK encapsulates all the technology-specific details of a semiconductor process. This includes the process nodes (e.g., 130nm, 65nm, 28nm, etc.), device models, design rules, and parameters specific to the fabrication technology.
- It allows ASIC designers to understand and apply the physical characteristics and constraints of the manufacturing process when designing their circuits.
##### 2. Standard Cell Libraries:
- PDKs include libraries of pre-designed and verified standard cells, such as logic gates, flip-flops, and other basic building blocks. These cells are optimized for the specific manufacturing process and are used extensively in the design of ASICs.
- Designers can use these cells to construct more complex circuits, ensuring that their designs will be manufacturable and meet the desired performance criteria.
##### 3. Design Rule Checking (DRC) and Layout Versus Schematic (LVS):
- The PDK provides the design rules required to check the layout for manufacturability. These rules ensure that the physical design adheres to the manufacturing constraints, preventing issues that could arise during fabrication.
- It also includes information for LVS checks, which ensure that the layout corresponds accurately to the intended circuit design. This step is critical for verifying that the design will function as expected after fabrication.
   ###### Design Rule Checking (DRC)
     - DRC is a verification process that checks whether the physical layout of an IC adheres to a set of design rules provided by the semiconductor foundry. These rules define the minimum and maximum dimensions, spacing, and alignment of various features on the chip, such as metal layers, vias, and transistors. The design rules are critical for ensuring that the chip can be reliably manufactured and will function correctly
   ###### Layout Versus Schematic (LVS)
     - LVS is a verification process that compares the netlist extracted from the physical layout with the original schematic netlist to ensure that they match. The schematic represents the intended electrical connections and behavior of the circuit, while the layout represents the actual physical implementation of that design on the chip.
##### 4. Device Models for Simulation:
- PDKs include SPICE (Simulation Program with Integrated Circuit Emphasis) models for transistors and other devices. These models are used in simulation tools to predict the electrical behavior of circuits before they are fabricated.
##### 5.Process-Specific Design Automation:
- PDKs are tightly integrated with Electronic Design Automation (EDA) tools, enabling automated processes like place-and-route, timing analysis, and power optimization. The PDK ensures that these tools are calibrated for the specific process technology.
### What is .LEF File ?
- A LEF (Library Exchange Format) file is a standard file format used in the semiconductor industry to describe the physical aspects of standard cells, macros, and other components in a design. LEF files are crucial for the place-and-route (P&R) stage of ASIC and FPGA design, where the physical layout of a chip is finalized.The information in a LEF file helps design tools understand the physical constraints and characteristics of the cells, which allows for efficient placement and routing of the components on a silicon wafer.
### What is ABC (Algorithm for Boolean Circuits) ?
- ABC is an academic software tool used for the synthesis and verification of Boolean circuits. It integrates various algorithms for logic synthesis, technology mapping, and optimization. ABC is often used in the research community for experimenting with new algorithms and techniques in logic synthesis and formal verification.
- 
### RTL2GDS OpenLANE ASIC Flow Practical Implementation

#### LAB-1
##### Steps to Use OpenLane in Interactive Mode
###### 1. Setup OpenLane Environment:
First, ensure that OpenLane is properly installed and that you have the necessary tools and libraries available.
###### 2. Start the Docker Container:
- OpenLane operates within a Docker container. You can start the container using the following command:
```bash
  make mount
```
- This command mounts your current directory into the Docker container and drops you into an interactive shell.
###### 3. Initialize the Design:
- Once inside the container, you need to prepare your design for the flow. This typically involves setting up the design directory and copying your RTL code into it.
- Use the following command to start an interactive session for your design:
```bash
./flow.tcl -interactive
```
- This command will start OpenLane in interactive mode.
###### 4. Run Individual Flow Stages:
- In interactive mode, you can manually execute each step of the ASIC design flow. Common steps include:

  
  Synthesis:
   ```bash
   run_synthesis
   ```
  Floorplanning:
   ```bash
   run_floorplan
   ```
  Placement:
    ```bash
   run_placement
    ```
  Clock Tree Synthesis (CTS):
   ```bash
   run_cts
    ```
  Routing:
    ```bash
   run_routing
    ```
  Signoff:
   ```bash
   run_signoff
   ```
###### 5. Inspect and Modify Intermediate Results:
- After each step, you can inspect the results, check logs, and adjust parameters as needed. This level of control allows you to optimize the design or troubleshoot issues before proceeding to the next step.
###### 6. Generate Final GDSII File:
- Once all steps are successfully completed, you can generate the final GDSII file, which is the format required for fabrication:
  ``` bash
  run_magic
  ````
- This step involves running DRC (Design Rule Check), LVS (Layout vs. Schematic), and generating the final layout

  
- ![image](https://github.com/user-attachments/assets/80a9cb83-590f-49d1-8735-e83baba69f6d)

  


To run in interactive mode 
 command will be 
 ```bash
bash-4.2$ ./flow.tcl -interactive
 ```
package import and check
```bash
% package require openlane
```
prepare for the design run the command 
```bash
% prep -design picorv32a
```
command for synthesis 
```bash
 % run_synthesis 
```
![image](https://github.com/user-attachments/assets/646b74cb-2b89-4326-a949-09583601e8b1)

 

![image](https://github.com/user-attachments/assets/1ec8faeb-1b46-4ae1-ae4a-f7fefec8a87c)

###### D Flip-Flop Ratio
Now we will find the flip flop ratio
```bash
flop ratio =(total no. of d flop realised) / (total no. cells)
           = 1613/14876
           = 0.10842
```

# Day - 2 Good floorplaning vs Bad floorplaning and introduction to library cells

## Utilization Factor and Aspect Ratio of a die----

**Utilization Factor**
--Utilization Factor is a quantity based on which we can say how efficiently we used a core area of chip or die by the memory components or any other logic block.
--It's the ratio between total area occupied by the netlist and total area of the core. 
````bash
*Utilization Factor* == Area occcupied by the netlist/Total area of the core
````

**Aspect Ratio**
--Aspect Ratio   of a chip or die refers to the ratio of its width to its height.
aspect ratio:
````bash
          A.r = width/height

````



The aspect ratio affects the chip's mechanical stability, ease of packaging, and manufacturability. Ideally, the aspect ratio should be close to 1 (i.e., the chip is nearly square), as this minimizes stress and simplifies the packaging process. However, depending on the application and design constraints, the aspect ratio may vary.

![Screenshot 2024-09-25 153217](https://github.com/user-attachments/assets/bf1f58dd-c22e-4265-a22e-c4c1a45bb4da)



![image](https://github.com/user-attachments/assets/0a83855f-d17a-4b7a-9480-9f1f482a8bc6)


**Run Floorplan**
````bash
run_floorplan
````
![image](https://github.com/user-attachments/assets/69c45911-8da4-4a25-9386-e119972763b0)
![image](https://github.com/user-attachments/assets/a096e81f-9ef0-4616-b453-56faa505724f)

**Picorv32a Floorplan default values**
![image](https://github.com/user-attachments/assets/ab72baf5-c3a6-495e-897a-ed64299b7e54)

![Screenshot 2024-09-25 174521](https://github.com/user-attachments/assets/afee2bfb-a8de-4979-857e-f0b7311cf317)

**magic tool tech file and the layout of design:**


![Screenshot 2024-09-25 180429](https://github.com/user-attachments/assets/3a1572ac-8ca1-4e97-8121-003fc609b933)

**Centering the Design:**

Press ``` S ``` to select the entire design.

Press ``` V ``` to vertically align it to the middle of the screen.

**Zooming In on a Specific Area:**

Left-click and drag to select the desired region.

Right-click to bring up the context menu.

Press``` Z ``` to zoom in on the selected area.


**Getting Details of a Cell:**

Move your cursor to the cell of interest.

Press ``` S ``` to select the cell.

In the tkcon window, enter the command "what" to display cell details.

## Placement in VLSI Design
Placement plays a crucial role in VLSI (Very Large Scale Integration) design. It involves determining the physical locations of standard cells or logic elements within a chip or block. Let's break it down:

- **Global Placement:**

  - Global placement assigns general locations to movable objects (cells).
  - Some overlaps between placed objects are allowed during this stage. 
  - The goal is to achieve a rough layout that satisfies area constraints.

- **Detailed Placement:**

  - Detailed placement refines the object locations obtained from global placement.
  - It enforces non-overlapping constraints and ensures that cells are placed on legal cell sites.
  - The quality of detailed placement significantly impacts subsequent routing stages.

``` magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def & ```



![Screenshot 2024-09-25 230658](https://github.com/user-attachments/assets/d5ca9ce5-6360-4f93-9544-495b7068fc0d)


## CELL DESIGN AND CHARACETRIZATION FLOWS

Library is a place where we get information about every cell. It has differents cells with different size, functionality,threshold voltages. There is a typical cell design flow steps.

    Inputs : PDKS(process design kit) : DRC & LVS, SPICE Models, library & user-defined specs.
    Design Steps :Circuit design, Layout design (Art of layout Euler's path and stick diagram), Extraction of parasitics, Characterization (timing, noise, power).
    Outputs: CDL (circuit description language), LEF, GDSII, extracted SPICE netlist (.cir), timing, noise and power .lib files
## Standard Cell Characterization Flow
A typical standard cell characterization flow that is followed in the industry includes the following steps:

- Read in the models and tech files
- Read extracted spice Netlist
- Recognise behavior of the cells
- Read the subcircuits
- Attach power sources
- Apply stimulus to characterization setup
- Provide neccesary output capacitance loads
- Provide neccesary simulation commands
Now all these 8 steps are fed in together as a configuration file to a characterization software called GUNA. This software generates timing, noise, power models. These .libs are classified as Timing 
 characterization, power characterization and noise characterization.





| Timing defintion   | value| 
|----------|----------|
| `slew_low_rise_thr`|`20% value`|
|`slew_high_rise_thr`| `80% value`   |
| `slew_low_fall_thr`| `20% value`   |
|`slew_high_fall_thr`| `80% value`   |
| `in_rise_thr`      | `50% value`   |
| `in_fall_thr`      | `50% value`   |
| `out_rise_thr`     | `50% value`   |
| `out_fall_thr`     | `50% value`   |



## Propagation Delay and Transition Time
**Propagation delay** is the time it takes for a signal to travel from the input to the output of a circuit. It's typically measured as the time difference between when the input signal reaches 50% of its final value and when the output signal reaches 50% of its final value.

If the threshold values used to measure this delay are not chosen carefully, it can result in negative delay values, which are not physically meaningful. However, even with well-chosen thresholds, the delay might still appear positive or negative due to variations in the slew rate, which is how quickly the signal transitions from one value to another.


``` bash
Propagation delay = time(out_thr) - time(in_thr)
```

**Transition Time**
Transition time is the time it takes for a signal to move between its low and high states (or vice versa). It’s typically measured between the points where the signal reaches 10% and 90% of its final value, or sometimes between 20% and 80%. This metric is crucial for understanding the speed of signal changes in a circuit.


``` bash
Rise transition time = time(slew_high_rise_thr) - time (slew_low_rise_thr)

Low transition time = time(slew_high_fall_thr) - time (slew_low_fall_thr)
