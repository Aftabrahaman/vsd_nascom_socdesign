# vsd_nascom_socdesign
## Contents

##  Day - 1 Introduction of Open-Source EDA, OpenLane and Sky130 PDK


## Openlane
OpenLane is an open-source, fully-automated RTL-to-GDSII flow for ASIC design. It provides a comprehensive platform for converting digital circuit designs (typically described in RTL—Register Transfer Level) into a final layout that can be fabricated on silicon

OpenLane integrates various open-source tools and scripts to handle different stages of the ASIC design flow. 

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
- This step involves running DRC (Design Rule Check), LVS (Layout vs. Schematic), and generating the final layout.
  
![Screenshot from 2024-08-22 17-41-01](https://github.com/user-attachments/assets/bbdc48bc-b228-4819-9db6-d7feaa73d3b9)

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

![Screenshot from 2024-08-22 18-43-04](https://github.com/user-attachments/assets/3bb4b134-afdc-42f3-97bc-4bee082626d2)

![Screenshot from 2024-08-22 18-43-17](https://github.com/user-attachments/assets/2c3ca1da-b5b9-4837-800e-2b750ac17aa7)
###### D Flip-Flop Ratio
Now we will find the flip flop ratio
```bash
flop ratio =(total no. of d flop realised) / (total no. cells)
           = 1613/14876
           = 0.10842
```


![Screenshot from 2024-08-22 18-13-40](https://github.com/user-attachments/assets/a6be1ca5-842b-4431-a771-784ba144fc95)
  
