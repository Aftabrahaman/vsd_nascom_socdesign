# VSD-PHYSICAL-DESIGN-SOC-DESIGN-PROGRAM

## Table of Contents
- [Day - 1 Introduction of Open-Source EDA, OpenLane and Sky130 PDK](#day---1-Introduction-of-Open-Source-EDA-OpenLane-and-Sky130-PDK)
- [Day - 2 Good floorplaning vs Bad floorplaning and introduction to library cells](#day---2-Good-floorplaning-vs-Bad-floorplaning-and-introduction-to-library-cells)
-  [Day -3 Design Library Cell using magic layout and ngspice charcterization](#Day--3-Design-Library-Cell-using-magic-layout-and-ngspice-charcterization)
- [DAY -4 Pre Lay-out Timing Analysis and Importance of Good clock Tree](#Day--4-Pre-Lay-out-Timing-Analysis-and-Importance-of-Good-clock-Tree)
- [DAY -5 Final Steps for RTL2GDS Using TritonROUTE and openSTA](#Day--5-Final-Steps-for-RTL2GDS-Using-TritonROUTE-and-openSTA)

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
 ```

# Day -3 Design Library Cell using magic layout and ngspice charcterization

## CONTENTS

- [VTC Spice Simulation](#vtc-spice-simulation)
- [Concept on Switching Threshold](#concept-on-switching-threshold)
- [Static And Dynamic Simulation of CMOS Inverter](#static-and-dynamic-simulation-of-cmos-inverter)
- [Layout and CMOS Fabrication Process](#layout-and-cmos-fabrication-process)
- [LABs Exercise](#labs-exercise)


## VTC Spice Simulation

Voltage Transfer Characteristic (VTC) is a key concept in electronic circuit analysis and design, especially in analog and mixed-signal integrated circuits. A VTC SPICE simulation refers to the process of using a SPICE (Simulation Program with Integrated Circuit Emphasis) simulator to analyze and plot the VTC curve of a circuit.

In a VTC simulation, the input voltage of the circuit is gradually swept over a specified range, while the corresponding output voltage is measured. The plot of output voltage versus input voltage generated from this data provides valuable insights into the circuit’s linearity, gain, and various operating regions. This is especially useful for evaluating the performance of amplifiers, logic gates, and signal processing circuits.

Here’s a step-by-step guide for performing a VTC SPICE simulation:

- Define the circuit in SPICE using its netlist.
- Set up a DC sweep simulation for the input voltage.
- Specify the range over which the input voltage will vary.
- Run the simulation to record the output voltage for each input value.
- Plot the VTC curve (output voltage vs. input voltage).
- Analyze the plot to evaluate the circuit’s behavior, including linearity, gain, and transition regions.

**Circuit Design:** The circuit to be analyzed is designed in a schematic capture tool that is compatible with the SPICE simulator being used.

**Simulation Setup:** The SPICE simulation is set up with the appropriate analysis type, which could be a DC sweep analysis if the goal is to plot the VTC over a range of DC input voltages.

**nput Signal Sweep:** The input voltage source is configured to sweep across the desired range of voltages. This could be from the negative supply rail to the positive supply rail, or any other relevant range.

**Output Measurement:**  The output node of the circuit is specified, and the simulator is instructed to record the voltage at this node for each input voltage step.

**Simulation Run:** The simulation is run, and the SPICE engine calculates the circuit's response to each input voltage.

**Data Analysis:**  The resulting data is plotted as a graph with input voltage on the x-axis and output voltage on the y-axis. This plot is the VTC of the circuit.

**Interpretation:**  The VTC is analyzed to determine the circuit's performance characteristics, such as gain, input offset voltage, output swing, and the presence of any non-linearities or distortion.

![Screenshot 2024-09-27 024757](https://github.com/user-attachments/assets/4d8b6f33-1e65-43e2-9ee1-8a9a564a556f)

![Screenshot 2024-09-27 025512](https://github.com/user-attachments/assets/49065600-8fa8-4f37-8915-c2f25a705e55)




### Concept on Switching Threshold

The concept of the switching threshold is crucial in the context of digital circuits, particularly in logic gates and transistors. The switching threshold refers to the input voltage level at which the circuit transitions from one state to another, typically from a low (logic 0) to a high (logic 1) state or vice versa.

In a digital circuit, the switching threshold is designed to be at a specific voltage level to ensure reliable operation and to minimize the effects of noise and signal degradation. The exact value of the switching threshold can vary depending on the technology and design of the circuit, but it is typically set near the midpoint of the supply voltage range.

For example, in a CMOS (Complementary Metal-Oxide-Semiconductor) inverter, which is a fundamental building block of digital logic, the switching threshold is ideally at the midpoint of the supply voltage (Vdd/2). This ensures that the inverter has equal noise margins for both high and low input levels.

The switching threshold is influenced by several factors, including:

   **1. Device Characteristics:** The physical properties of the transistors used in the circuit, such as their threshold voltage (Vth), affect the overall switching threshold of the circuit.

  **2. Supply Voltage:**  The operating voltage of the circuit can impact the switching threshold. As the supply voltage changes, the switching threshold may also shift.

   **3. Temperature:** Temperature variations can cause changes in the electrical characteristics of the transistors, which can in turn affect the switching threshold.

   **4. Process Variations:** Manufacturing variations can lead to differences in the electrical properties of transistors, which can influence the switching threshold of the circuit.

   **5. Load Capacitance:** The capacitive load connected to the output of the circuit can affect the switching threshold due to the dynamic behavior of the circuit during switching.

![image](https://github.com/user-attachments/assets/31e3b8d8-189c-40b3-bfdb-f3ec66e79c79)





## Static And Dynamic Simulation of CMOS Inverter

### Static Simulation
Static simulation, also known as DC analysis, involves analyzing the circuit's behavior at DC (Direct Current) steady-state conditions. This type of simulation is used to determine the following characteristics:

   Voltage Transfer Characteristic (VTC): The VTC plot shows the relationship between the input voltage (Vin) and the output voltage (Vout) of the inverter. It helps in understanding the transition between the logic levels (0 and 1) and the noise margins.

**Switching Threshold (V_th):** The input voltage at which the inverter switches from one logic state to another. Ideally, for a symmetrical VTC, the switching threshold is at Vdd/2, where Vdd is the supply voltage.

**Noise Margins:** The maximum noise voltage that can be tolerated at the input while the output remains in the correct logic state.

**Static Power Dissipation:** The power consumed by the inverter when it is in a stable state (not switching). This is typically very low in CMOS circuits due to the low leakage currents.

### Dynamic Simulation
Dynamic simulation, also known as transient analysis, involves analyzing the circuit's behavior over time as it responds to time-varying input signals. This type of simulation is used to determine the following characteristics:

**Propagation Delay (t_p):** The time it takes for the output to change in response to a change in the input. It is typically measured as the time from the 50% point of the input transition to the 50% point of the output transition.

**Rise Time (t_r) and Fall Time (t_f):** The times it takes for the output to transition from 10% to 90% (rise time) and from 90% to 10% (fall time) of the output voltage swing.

**Power Consumption:** The dynamic power consumed by the inverter during switching, which is a function of the switching frequency, load capacitance, and supply voltage.

**Transient Response:**  The overall response of the inverter to input signals, including overshoot, undershoot, and ringing.
    
![maxresdefaul4t](https://github.com/user-attachments/assets/5a72fdab-6149-4377-a1b9-b53323efa131)



## Layout and CMOS Fabrication Process

The CMOS (Complementary Metal-Oxide-Semiconductor) fabrication process is a complex series of steps used to manufacture integrated circuits. CMOS technology is widely used in modern electronics due to its low power consumption and high-density integration capabilities. The fabrication process involves several key steps, including substrate preparation, doping, oxidation, photolithography, etching, and metallization. Here is an overview of the CMOS fabrication process:
**1. Substrate Preparation**

    Starting Material: The process begins with a pure silicon wafer, which serves as the substrate.
    Cleaning: The wafer is thoroughly cleaned to remove any impurities.

**2. Oxidation**

    Thermal Oxidation: A layer of silicon dioxide (SiO2) is grown on the surface of the silicon wafer. This oxide layer acts as an insulator and also serves as a mask for subsequent doping processes.

**3. Photolithography**

    Photoresist Application: A light-sensitive polymer called photoresist is applied to the wafer surface.
    Exposure and Development: The wafer is exposed to ultraviolet (UV) light through a mask that contains the desired pattern. The exposed photoresist is then developed, leaving a patterned layer on the wafer.

**4. Etching**

    Wet or Dry Etching: The areas of the wafer not protected by the photoresist are etched away using either wet chemicals or dry plasma etching.

**5. Doping**

    Ion Implantation: Selected areas of the wafer are doped with impurities (dopants) such as boron or phosphorus to create n-type or p-type regions. This process alters the electrical properties of the silicon, creating the necessary semiconductor characteristics.

**6. Isolation**

    Local Oxidation of Silicon (LOCOS): An oxide layer is grown in selected areas to isolate different components of the circuit.
    Trench Isolation: Alternatively, trenches can be etched and filled with oxide to achieve isolation.

**7. Transistor Formation**

    - Gate Oxide Growth:  A thin layer of oxide is grown on the surface where the transistors will be formed.

    - Polysilicon Deposition: A layer of polysilicon is deposited and patterned to form the gates of the transistors.

    - Spacer Formation: Spacers are created on the sides of the gate to control the subsequent doping process.

    - Source/Drain Implantation: Dopants are implanted to form the source and drain regions of the transistors.

**8. Silicidation**

    Silicide Formation: A silicide layer (e.g., tungsten silicide) is formed on top of the polysilicon gate and the source/drain regions to reduce resistance.

**9. Interlayer Dielectric and Planarization**

    Dielectric Deposition: A layer of insulating material (e.g., silicon dioxide) is deposited over the entire wafer.
    Chemical Mechanical Polishing (CMP): The wafer surface is planarized using CMP to ensure a smooth surface for subsequent metallization.

**10. Metallization**

    Metal Layers: Multiple layers of metal (e.g., aluminum or copper) are deposited and patterned to form the interconnections between transistors and other components.
    Contact and Via Formation: Contacts and vias are etched and filled with metal to connect the transistors to the metal layers.

**11. Passivation**

    Passivation Layer: A final layer of insulating material is deposited to protect the circuit from external contaminants.

**12. Testing and Packaging**

    Wafer Probing: The wafer is tested to identify any defective chips.

    Dicing: The wafer is cut into individual chips (dice).

    Packaging: The chips are mounted in packages, which are then sealed to protect the delicate circuitry.

    Final Testing: The packaged chips are tested again to ensure they meet the required specifications.



### LABs Exercise

#### 1. IoPlacer revision & adding the "set::env" command

![Screenshot 2024-09-25 180429](https://github.com/user-attachments/assets/895684f4-566c-429a-822d-0058b16d694b)


#### 2. Inverter layout in Magic tool

![image](https://github.com/user-attachments/assets/760da63c-b374-4029-ada7-90b99297d174)

![image](https://github.com/user-attachments/assets/4fe2e412-8fc6-4cf5-828b-24583e633cc0)

![image](https://github.com/user-attachments/assets/30a90bb4-740a-483d-b0c9-9398be004bda)





#### 3. Extraction to spice format

![image](https://github.com/user-attachments/assets/c87a83fa-e97e-449a-ba95-7bea1cdbe00d)

![image](https://github.com/user-attachments/assets/cfc868ff-d496-4114-a1fe-3ab05e9b5e37)



#### 4. spice tecgnology file, simulation and output graph

![image](https://github.com/user-attachments/assets/68c0d3eb-ea99-4db3-8cd8-923bccfff58f)

![image](https://github.com/user-attachments/assets/0693d6ef-1fca-481d-9c5b-3aa95be224be)



#### 5. Parameters Calculation
##### 5-a rise transition of of output: 20% of VDD to 80% of VDD
rise_transition = (80% of 3.3v) - (20% of 3.3v)
                = (x-coordinate pos. of 2.64v - x-coordinate pos. of 0.66v)
                = (6.24247*e-19) - (6.61804*e-19)
                = 0.06163 ns

##### 5-b fall transition of of output: 80% of VDD to 20% of VDD
fall_transition = (20% of 3.3v) - (80% of 3.3v)
                = (x-coordinate pos. of 0.66v - x-coordinate pos. of 2.64v)
                = (8.09409*e-19) - (8.05156*e-19)
                = 0.04253 ns
                

                
 ##### 5-c cell rise delay: (50% of o/p rise) - (50% of i/p fall)
 cell_rise_delay= (x-coordinate pos. of 1.65v - x-coordinate pos. of 1.65v)
                = (6.2089*e-19) - (6.15*e-19)
                = 0.0589 ns
                
   

 ##### 5-d cell fall delay: (50% of o/p fall) - (50% of i/p rise)
 cell_fall_delay= (x-coordinate pos. of 1.65v - x-coordinate pos. of 1.65v)
                = (8.07657*e-19) - (8.05*e-19)
                = 0.02657 ns
                
  


   ##### 6. Introduction to Magic tool and DRC




   ## DAY -4 Pre Lay-out Timing Analysis and Importance of Good clock Tree

### CONTENTS

- [1-Delay Table](#1-delay-table)
- [2-Setup time and Hold time of Flop](#2-setup-time-and-hold-time-of-flop)
- [3-Clock tree routing and buffering](#3-clock-tree-routing-and-buffering)
- [4-LABs Steps](#4-labs-steps)




### 1-Delay Table

In physical design, a delay table is a data structure used to model the delay characteristics of standard cells or interconnects in a digital circuit.

#### Purpose
**Estimate Delays:**  Delay tables help in estimating the propagation delay through gates or along wires, which is crucial for timing analysis.

#### Delay Values  
The actual propagation delays, typically provided for different combinations of input slew and output load.

#### Usage
**Static Timing Analysis (STA):** Delay tables feed information into STA tools to evaluate the timing performance of a design.

**Cell Libraries:** Standard cell libraries include delay tables for each cell type, allowing EDA tools to accurately model and simulate circuit behavior.
#### Types
**Linear Delay Models:** Simplified models that use linear equations.

**Non-linear Delay Models:** More complex and accurate, capturing variations due to non-linear effects.
#### Importance
Accuracy: Provides accurate timing information, crucial for ensuring that the design meets its timing constraints.

Optimization: Helps in identifying critical paths and optimizing them to improve performance.

![Screenshot from 2024-09-03 19-46-27](https://github.com/user-attachments/assets/3accdf7b-aa36-4e30-91b4-a7f8ad3b2006)



### 2-Setup time and Hold time of Flop

#### Set Up Time Analysis
Setup time is the minimum time period before the clock edge during which the data input must be stable.

#### Purpose:
Ensures that the data is correctly sampled by the flip-flop on the active clock edge.

#### Analysis Steps:
Identify Critical Paths: Trace the longest path from one flip-flop to the next, including combinational logic.

**Calculate Data Path Delay**: Sum up the delays of all elements (gates, interconnects) in the data path.

**Compare with Setup Time:** Ensure that the data path delay plus setup time is less than the clock period.

Data Path Delay + Setup Time < Clock Period

Adjust if Necessary: If the condition isn’t met, optimize the design by reducing delays, increasing clock period, or modifying the path.

#### Hold Time Analysis
Hold time is the minimum time period after the clock edge during which the data input must remain stable.

#### Purpose:
Prevents the new data from being captured too early, ensuring the current data is held long enough.

#### Analysis Steps:
**Identify Critical Paths:** Examine paths where new data might overwrite current data too soon.

**Calculate Data Path Delay:** Consider the minimum delay from the clock edge to the data input.

**Compare with Hold Time:** Ensure that the data path delay is greater than the hold time.

Data Path Delay>Hold Time

**Adjust if Necessary:** If the condition isn’t met, add buffers or delay elements to increase path delay.

#### Importance
**Setup Time Violations:** Can lead to incorrect data being captured, causing functional errors.

**Hold Time Violations:** Can result in data corruption as new data overwrites old data prematurely.

![Screenshot from 2024-09-03 21-43-49](https://github.com/user-attachments/assets/f0228524-7072-4822-87b6-0aaff1b40980)




### 3-Clock tree routing and buffering

Clock tree routing and buffering are crucial steps in the physical design phase of integrated circuit (IC) design. These steps ensure that the clock signal is distributed efficiently and uniformly across the entire chip to all sequential elements (like flip-flops) with minimal skew and latency.

#### a. Clock Tree Synthesis (CTS):
**Purpose:** The goal of clock tree synthesis is to distribute the clock signal from a single clock source (usually a Phase-Locked Loop (PLL) or a clock generator) to all the sequential elements in the design (like flip-flops) with minimal skew and balanced delays.

#### Challenges:
**Clock Skew:** The difference in arrival times of the clock signal at different sequential elements. Skew can lead to timing violations, so minimizing it is a primary goal.

**Clock Latency:** The delay from the clock source to a flip-flop or other sequential element. Latency needs to be controlled to meet timing requirements.

#### Power Consumption: 

Clock networks are often the most power-consuming part of the chip. Optimizing for lower power while maintaining performance is crucial.

#### b. Clock Tree Routing:

**Tree Structure:** The clock distribution network is usually constructed as a tree (hence the name "clock tree"). This tree structure helps in balancing the delays and skew.

#### Clock Tree Topologies:
- H-Tree: A hierarchical tree structure that is symmetric and balanced, often used in regular grid layouts.

- X-Tree, Y-Tree: Variations of the H-tree, used based on specific design needs.

- Spine: A clock distribution method where the clock is routed along a central "spine" and branches out to different regions. This is common in large, hierarchical designs.

**Routing Techniques:**
- Minimal Skew Routing: Ensuring that all paths from the clock source to the clock sinks (sequential elements) are balanced to minimize skew.
- Buffered Clock Tree: Inserting buffers along the clock tree to manage delay and drive strength, ensuring that the clock signal reaches all parts of the circuit with sufficient strength and minimal degradation.

#### c. Clock Tree Buffering:
**Why Buffers are Needed:**
- Load Management: The clock signal needs to drive a large number of flip-flops and other clocked elements. Buffers are used to amplify the clock signal and drive these loads effectively.
- Delay Control: Buffers help in balancing the delay in different paths of the clock tree, which is crucial for minimizing skew.
- Noise Reduction: By buffering the clock signal, noise introduced by long interconnects can be reduced.

**Types of Buffers:**
- Inverters: Simple buffers that invert the signal. Sometimes used in pairs to maintain the same logic level while buffering.
- Dedicated Clock Buffers: Specially designed buffers that are optimized for driving the clock signal with high fan-out and minimal jitter.
- Buffer Insertion: Buffers are strategically inserted at points in the clock tree where the clock signal needs to be amplified or where delay needs to be controlled.
The placement of these buffers is determined during the Clock Tree Synthesis process using EDA tools, which optimize for delay, skew, and power consumption.

#### d. Skew and Latency Management:
- Zero-Skew Clock Tree: An ideal clock tree would have zero skew, meaning all flip-flops receive the clock signal at the exact same time. While practically impossible, the goal is to minimize skew as much as possible.
- Useful Skew: Sometimes, intentional skew is introduced to improve timing in certain paths. This is known as useful skew and can help in meeting setup and hold time constraints.
Clock Latency: Clock tree buffering and routing also ensure that the clock signal arrives at the flip-flops with the desired latency, which is crucial for timing closure.

#### e. Post-CTS Optimization:
After the clock tree is synthesized, further optimization steps like Clock Tree Optimization (CTO) and Post-CTS Optimization might be performed to fine-tune the clock network, ensuring that all timing requirements are met.
- Timing Analysis: Tools perform static timing analysis (STA) to verify that the clock tree meets the required timing constraints, and any violations are corrected by adjusting the clock tree design.

![Screenshot from 2024-09-03 22-05-22](https://github.com/user-attachments/assets/3592f0f9-aa1b-4356-878a-5bb2c7cdaa2f)

![Screenshot from 2024-09-03 22-05-45](https://github.com/user-attachments/assets/685395e4-8914-46e9-b331-f2ab999e09d5)


### 4-LABs Steps:

tracks.info 
![Screenshot from 2024-09-01 15-25-09](https://github.com/user-attachments/assets/3a412cc9-1e37-40b5-bec7-4e8fed3a55b3)

file:///home/vsduser/Pictures/Screenshot%20from%202024-10-01%2003-32-05.png![image](https://github.com/user-attachments/assets/fb2fabb5-99dc-49e1-b091-f3fd1b2a5119)




Inverter_mag
file:///home/vsduser/Pictures/Screenshot%20from%202024-10-01%2003-35-25.png![image](https://github.com/user-attachments/assets/3d8e8e85-3f5e-4e94-9ba2-322a35046a32)
file:///home/vsduser/Pictures/Screenshot%20from%202024-10-01%2003-39-04.png![image](https://github.com/user-attachments/assets/1e9c3ea6-e3b3-453f-a49e-8fe30b62ec70)
file:///home/vsduser/Pictures/Screenshot%20from%202024-10-01%2003-40-12.png![image](https://github.com/user-attachments/assets/68fb0c94-a3a1-40a1-a50d-b0ddf22add69)
file:///home/vsduser/Pictures/Screenshot%20from%202024-10-01%2003-43-10.png![image](https://github.com/user-attachments/assets/a8f36898-5c33-4e5d-8fac-86f5660458de)





copying the inverter lef to my design/src
file:///home/vsduser/Pictures/Screenshot%20from%202024-10-01%2003-51-15.png![image](https://github.com/user-attachments/assets/84c9703b-4030-4ac1-bd6e-a204fb397c72)



**lib file**


**config.tcl**


**run_synthesis**


file:///home/vsduser/Pictures/Screenshot%20from%202024-10-02%2019-54-48.png![image](https://github.com/user-attachments/assets/a228657f-86d8-499f-b3c9-09d7a2323335)


**run_floorplan**
```
run init_floorplan

run place_io

tap_decap_or
```
file:///home/vsduser/Pictures/Screenshot%20from%202024-10-02%2020-09-16.png![image](https://github.com/user-attachments/assets/f17891cd-645a-4cde-9d88-e9eb51b3186a)

!



**run_placement**
file:///home/vsduser/Pictures/Screenshot%20from%202024-10-02%2020-13-17.png![image](https://github.com/user-attachments/assets/fcd89403-a734-4665-986f-500519982bc6)



**lay-out**
file:///home/vsduser/Pictures/Screenshot%20from%202024-10-03%2001-05-14.png![image](https://github.com/user-attachments/assets/b6213cd6-1a0a-448b-b70c-6b3dc6e79473)
file:///home/vsduser/Pictures/Screenshot%20from%202024-10-03%2001-11-39.png![image](https://github.com/user-attachments/assets/391f0c4e-0768-4c21-9284-6d427041f379)
file:///home/vsduser/Pictures/Screenshot%20from%202024-10-03%2001-12-18.png![image](https://github.com/user-attachments/assets/fe48f177-67d2-428b-9a1c-193b3e6e6567)


**my_base.sdc**
file:///home/vsduser/Pictures/Screenshot%20from%202024-10-03%2006-15-41.png![image](https://github.com/user-attachments/assets/e2f30bfd-194f-4fad-a21f-922a3dc0fb42)


**sta.conf**
file:///home/vsduser/Pictures/Screenshot%20from%202024-10-03%2006-18-25.png![image](https://github.com/user-attachments/assets/36157603-c8a2-437c-8042-98a2e9d8ea43)


**openroad**


**CTS run**






## DAY -5 Final Steps for RTL2GDS Using TritonROUTE and openSTA

### CONTENTS

- [1-Routing and DRC](#1-routing-and-drc)
- [2-Global routing and Detailed routing](#2-global-routing-and-detailed-routing)
- [3-Maze routing and Steiner tree algorithm](#3-maze-routing-and-steiner-tree-algorithm)
- [4-Power distribution and network routing](#4-power-distribution-and-network-routing)
- [5-TritonRoute features](#5-tritonroute-features)
- [6-Labs practise](#6-labs-practise)



### 1-Routing and DRC
Routing and Design Rule Check (DRC) are key steps in the physical design process of integrated circuits (ICs). They play crucial roles in ensuring that the design is manufacturable and meets all the required electrical and physical constraints.

#### a. Routing:
Routing is the process of connecting the various components (standard cells, macros, IOs, etc.) in an IC design with metal interconnects according to the netlist generated during the synthesis stage.

#### Key Aspects of Routing:
**Global Routing:**

- Purpose: Provides an abstract, coarse-grained plan of how the connections will be made across different regions of the chip. It divides the chip area into a grid and assigns paths to nets without detailed geometry.
- Outcome: Guides the detailed routing stage by giving an overall connectivity layout.

  
**Detailed Routing:**

- Purpose: Converts the global routing plan into actual geometrical paths on the metal layers of the chip.
- Metal Layers: The routing is done using different metal layers, with lower layers typically used for local connections and higher layers for long-distance or global connections.

#### Routing Algorithms:

Maze Routing: Finds the shortest path between two points, avoiding obstacles.
Channel Routing: Manages routing in channels between rows of cells.
Grid-Based Routing: Uses a grid to guide the routing paths and ensure that they follow design rules.

**Routing Challenges:**

Congestion: Too many wires trying to pass through the same area can lead to congestion, which can cause timing issues or even make routing impossible.
Crosstalk: Signals on adjacent wires can interfere with each other, leading to noise and potential errors.
Delay: Longer or more resistive paths can introduce delays that affect the timing of the circuit.

**Routing Constraints:**

Timing Constraints: Ensuring that the routed paths meet the required timing (setup and hold times).

**Power Constraints:** Minimizing power consumption and ensuring that power distribution is balanced.

**Signal Integrity:** Maintaining signal quality by minimizing noise, crosstalk, and other electrical issues.

#### b. Design Rule Check (DRC):
Design Rule Check (DRC) is a verification step in the physical design process that ensures the layout of the chip adheres to a set of predefined rules provided by the semiconductor foundry. These rules are necessary to guarantee that the design can be manufactured reliably.

#### Key Aspects of DRC:

 **Design Rules:**

 - Spacing Rules: Minimum distances between different metal layers, vias, or other features to avoid shorts and ensure manufacturability.
 - Width Rules: Minimum and maximum width of wires to ensure that they can be manufactured correctly and carry the required current without issues.
 - Enclosure Rules: Requirements for how different layers must overlap or enclose each other (e.g., metal layers and vias).
 - Alignment Rules: Ensure that different layers align correctly to avoid misalignment during manufacturing.

**DRC Tools:**

Tools like Calibre, Mentor Graphics, or Cadence's Assura are commonly used to run DRC checks. These tools take the physical layout as input and verify it against the foundry's design rules.

**Common DRC Violations:**

- Shorts: When two wires or components that should not be connected are too close or overlap.
  
- Opens: Missing connections due to routing errors or insufficient overlap of layers.
  
- Minimum Width Violations: Wires or features that are too narrow and might not be reliably manufactured.
  
- Spacing Violations: Features that are too close together, which can cause shorts or manufacturing issues.
 

**DRC Correction:**

After running a DRC, any violations are flagged, and the design must be corrected. This may involve adjusting the layout, rerouting wires, or modifying cell placement.

![Screenshot from 2024-09-04 00-25-49](https://github.com/user-attachments/assets/de745192-63fb-4d8e-81cc-4091f6333ff6)
![Screenshot from 2024-09-04 00-26-08](https://github.com/user-attachments/assets/9b3dd792-b4b0-4183-943d-ea7bd653bcb2)



### 2-Global routing and Detailed routing

#### a. Global Routing:

**Purpose:**  Provides a high-level, coarse plan for connecting different regions of the chip. It divides the chip into grids and assigns general paths for connections without specifying exact wire routes.

**Outcome:** Guides the detailed routing stage by outlining broad paths for signals to follow, helping to manage congestion and ensure that connections can be made.

#### b. Detailed Routing:

**Purpose:** Converts the global routing plan into precise, geometric wire routes on specific metal layers, connecting all components according to the design's netlist.

**Outcome:**  Finalizes the exact paths for each wire, ensuring they meet design rules (like spacing and width), and resolves any conflicts or congestion identified during global routing.

![Screenshot from 2024-09-04 01-01-55](https://github.com/user-attachments/assets/60b5b7d6-ed70-4ab7-8ffc-c50d98ec4ba1)


### 3-Maze routing and Steiner tree algorithm

#### a. Maze Routing:

- Purpose: Maze routing is an algorithm used to find a path between two points (e.g., from a source to a destination pin) in a grid while avoiding obstacles.

**How It Works:**

The grid is treated as a matrix, where each cell represents a possible location for the wire. Obstacles like other wires, cells, or blocked areas are marked as unavailable.
The algorithm explores all possible paths from the source to the destination, usually using a breadth-first search (BFS) approach.
It expands from the source node by checking neighboring cells until it reaches the destination.
The shortest path found through this exploration is chosen as the route.
- Pros:

Optimal Path: Guarantees finding the shortest path if one exists.
Flexibility: Can handle complex obstacles and multiple routing layers.
- Cons:

Computationally Expensive: Can be slow and resource-intensive, especially for large designs.
Not Always Practical for Large Grids: The algorithm can become infeasible for very large or densely populated grids due to its exhaustive nature.

#### b. Steiner Tree Algorithm:

**Purpose:** The Steiner tree algorithm is used to connect multiple points (e.g., multiple pins that need to be connected by a single net) with the shortest possible interconnect length, minimizing the total wire length.

**How It Works:**

The algorithm starts by creating a minimal spanning tree (MST) that connects all the target points (e.g., pins).
Steiner points, which are additional points not originally in the set of target points, are introduced to reduce the overall wire length. These points act as intermediate nodes that help in minimizing the total connection length.
The resulting structure is a Steiner tree, which is a tree that connects all target points (and possibly additional Steiner points) with the minimal total interconnect length.

- Pros:

Minimized Wire Length: Produces a routing solution with minimal total wire length, which is beneficial for reducing delay and power consumption.
Efficient for Multi-Terminal Nets: Particularly useful for nets that need to connect more than two pins.

- Cons:
  
Complexity: Finding the exact Steiner tree is an NP-hard problem, meaning it can be computationally expensive for large nets.
Approximation: Often, heuristic or approximate methods are used to find a near-optimal Steiner tree, which might not be the absolute minimum.

![Screenshot from 2024-09-04 00-45-15](https://github.com/user-attachments/assets/f10eb3e1-abcd-49d3-8ef7-f201b5c945ba)
![Screenshot from 2024-09-04 00-45-34](https://github.com/user-attachments/assets/e14c7003-6fda-431b-8e08-100263c39ef2)
![Screenshot from 2024-09-04 00-45-49](https://github.com/user-attachments/assets/e8755cd7-549d-4899-9d29-79c55d5f5031)



### 4-Power distribution and network routing

**Power Distribution:**
Power distribution in IC design refers to the process of delivering power from the external power sources (like power pads or pins) to every component within the chip, including logic gates, flip-flops, memory cells, and other circuits.

##### Key Concepts:

**Power and Ground Rails:**
These are the main conductors that distribute power (VDD) and ground (VSS) across the chip. They are typically implemented as wide metal lines running across the chip to minimize resistance and voltage drop.

##### Power Grids:
A power grid is a network of horizontal and vertical metal lines that form a mesh-like structure across the chip. This grid ensures that power is distributed uniformly to all areas of the chip.
- VDD Grid: Carries the supply voltage.
  
- VSS Grid: Carries the ground potential.

##### Power Rings:
Rings of metal lines surrounding certain regions or blocks on the chip, providing a stable supply of power and grounding. They help in minimizing noise and ensuring a reliable power supply.

##### Power Straps:
Wider metal lines or groups of parallel lines that provide robust connections between the power grid and the power sources. These straps help reduce resistance and voltage drop.

##### Decoupling Capacitors:
Capacitors placed close to the power pins of circuits to stabilize the power supply by filtering out noise and compensating for sudden changes in current demand.

#### b. Power Network Routing:
Power network routing involves the detailed placement and routing of the power distribution network (PDN) on the chip, ensuring that all blocks and standard cells receive adequate power.

##### Steps in Power Network Routing:

**Placement of Power Pads:**
Power pads (VDD, VSS) are placed around the periphery of the chip or in specific locations within the chip. These pads connect the internal power network to the external power supply.

**Creation of Power Grid:**
A grid of metal layers is laid out across the chip to distribute the power. The grid usually spans multiple metal layers, with the lower layers used for local distribution and the upper layers for global distribution.

**Power Rings and Straps:**

Power rings are placed around the periphery of major blocks (e.g., memory blocks, large logic blocks) to ensure a stable power supply.
Power straps are used to connect the power rings and the power grid, ensuring that current can flow efficiently from the power pads to all parts of the chip.

**Via Placement:**

Vias are used to connect different metal layers within the power grid. Multiple vias are often placed in parallel to reduce resistance and ensure reliable current flow.

**IR Drop Analysis:**

After routing the power network, an IR drop analysis is performed to ensure that the voltage drop across the power grid is within acceptable limits. Excessive IR drop can cause circuits to malfunction due to insufficient voltage levels.

**Electromigration Check:**

Electromigration refers to the gradual movement of metal atoms in the power grid due to high current densities, which can lead to failures. The power network is checked to ensure that the current density in all metal lines is below the safe limits to avoid electromigration.

**Noise and Decoupling:**

The power network is designed to minimize noise (e.g., ground bounce) by carefully placing decoupling capacitors and ensuring that the grid structure is robust.

![Screenshot from 2024-09-04 00-59-03](https://github.com/user-attachments/assets/cdbff524-e23c-4c85-9162-34b2e208ce8d)




### 5-TritonRoute features

TritonRoute is an open-source detailed routing tool used in the physical design of integrated circuits (ICs). It is a critical component of the Triton suite of EDA (Electronic Design Automation) tools. TritonRoute is specifically designed for the detailed routing stage of VLSI (Very Large Scale Integration) design.

#### Key Features of TritonRoute:

**Detailed Routing:**

TritonRoute performs the detailed routing step in the IC design flow, where it converts global routing paths into exact geometrical wire routes on the metal layers of the chip.
It handles the connection of signal nets, power nets, and clock nets, ensuring that all routing adheres to design rules.

**Design Rule Checking (DRC):**

TritonRoute incorporates design rule checking within its routing algorithms to ensure that all routed wires comply with the foundry's design rules, such as spacing, width, and via requirements.
The tool is capable of resolving DRC violations that may arise during routing.

**Open-Source and Integration:**

TritonRoute is open-source, which means it can be freely used, modified, and integrated into custom EDA flows.
It is part of the broader OpenROAD project, which aims to provide a fully autonomous, open-source RTL-to-GDSII flow.

**High-Quality Routing:**

The tool focuses on generating high-quality routing solutions that minimize wire length, reduce congestion, and respect timing and power constraints.
It can handle complex designs with a large number of nets and multiple metal layers.
Scalability:

TritonRoute is designed to be scalable, handling both small and large designs efficiently. It leverages modern algorithms to manage the complexity of routing in advanced technology nodes.

**Customization and Flexibility:**

Being open-source, TritonRoute allows for extensive customization. Users can modify the tool to suit specific design requirements or integrate it with other tools in their design flow.
The tool can be adapted for different technology nodes and design styles.

**Part of the Triton Suite:**

TritonRoute works in conjunction with other tools in the Triton suite, such as TritonCTS (Clock Tree Synthesis) and TritonPlace (Placement), providing a comprehensive solution for physical design.

**Community and Support:**

As an open-source project, TritonRoute benefits from community contributions and ongoing development. It is actively maintained by researchers and contributors in the EDA community.

![Screenshot from 2024-09-04 01-07-12](https://github.com/user-attachments/assets/c6d657d9-fb20-47cd-987f-bf5396563fe1)
![Screenshot from 2024-09-04 01-07-40](https://github.com/user-attachments/assets/ae3d8a68-aa5c-49ec-bbee-c1bb00420a06)
![Screenshot from 2024-09-04 01-08-24](https://github.com/user-attachments/assets/3bb0926d-fd52-4ebe-b417-95f9ccbbc4a1)

