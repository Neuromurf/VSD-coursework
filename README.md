# Digital SoC Design using OpenLane 

This repository documents steps in execusing a complete desgin flow for a digital SoC using **OpenLane** which is an open-source RTL-to-GDSII flow built around the SkyWater 130nm (Sky130) PDK.

## Introduction to ASIC Design Flow and Open-Source EDA tools

This section provides a high-level overview of ASIC design pipeline, essential components involved in open-source ASIC chip design and open-source EDA tools specifically OpenLANE.

### High-level ASIC Design Flow 
ASIC design pipeline is a sequential series of processes, tools, and steps involved in designing, verifying, and manufacturing an ASIC.
The flow pipeline typically includes:

**1. Design:** Creation of the ASIC's architecture, writing HDL code (e.g., Verilog, VHDL), and verifying its functionality through simulation.

**2. Synthesis:** Conversion of the HDL code into a netlist, which represents the ASIC's components and connections, using tools like Synopsys Design Compiler.

**3. Place and Route:** Physical design of the ASIC, involving placement of components (e.g., transistors, wires) and routing of connections between them, using tools like Cadence Innovus.

**4. Timing Closure:** Ensuring the ASIC's design meets its timing requirements, including setup and hold times, clock domain crossing, and signal integrity, using tools like Synopsys PrimeTime.

**5. Physical Verification:** Checking the ASIC's design for physical correctness, including design rule checking (DRC), layout versus schematic (LVS), and electrical rule checking (ERC), using tools like Cadence Physical Verification System.

**6. Tapeout:** Finalizing the ASIC's design for manufacturing, including generating the mask layout, creating the manufacturing package, and delivering the design to the foundry for fabrication.

A high-level summary of these different stages in pipeline is depicted in the following illustration. 

<img width="4094" height="2432" alt="Image" src="https://github.com/user-attachments/assets/7a6d522e-a057-427e-86ef-f9b17c7ce140" />


### Open-Source EDA Tools
Open-source EDA tools enable digital design, synthesis, physical design, and verification without licensing constraints.

Following table lists some of the commonly used open-source EDA tools.


| **ASIC Flow Stage**                       | **Tool**                               | **Function**                             | **Link**                                                                                             |
| ----------------------------------------- | -------------------------------------- | ----------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------- |
| **1. RTL Simulation & Verification**      | **Verilator**                          | Simulates Verilog/SystemVerilog quickly to verify design behavior | [https://www.veripool.org/verilator/](https://www.veripool.org/verilator/)                           |
| **2. RTL Synthesis**                      | **Yosys**                              | Converts Verilog RTL into a gate-level netlist                    | [https://yosyshq.net/yosys/](https://yosyshq.net/yosys/)                                             |
| **3. Logic Optimization**                 | **ABC (inside Yosys)**                 | Optimizes logic and maps it to standard cells                     | [https://github.com/berkeley-abc/abc](https://github.com/berkeley-abc/abc)                           |
| **4. Floorplanning & Placement**          | **OpenROAD**                           | Automatically places standard cells and macro blocks              | [https://theopenroadproject.org/](https://theopenroadproject.org/)                                   |
| **5. Clock Tree Synthesis (CTS)**         | **TritonCTS** (OpenROAD)               | Builds the clock distribution network                             | [https://theopenroadproject.org/](https://theopenroadproject.org/)                                   |
| **6. Routing**                            | **TritonRoute / FastRoute** (OpenROAD) | Connects all the cells using metal routing layers                 | [https://theopenroadproject.org/](https://theopenroadproject.org/)                                   |
| **7. Static Timing Analysis**             | **OpenSTA**                            | Verifies that the chip meets timing requirements                  | [https://github.com/The-OpenROAD-Project/OpenSTA](https://github.com/The-OpenROAD-Project/OpenSTA)   |
| **8. DRC Check & Layout Editing**         | **Magic**                              | Checks for design rule violations; edits & views layouts          | [http://opencircuitdesign.com/magic/](http://opencircuitdesign.com/magic/)                           |
| **9. LVS Check (Layout vs Schematic)**    | **Netgen**                             | Compares layout vs. schematic to ensure correctness               | [http://opencircuitdesign.com/netgen/](http://opencircuitdesign.com/netgen/)                         |
| **10. GDSII Viewing / Additional DRC**    | **KLayout**                            | Visualizes GDS files and performs DRC                             | [https://www.klayout.de/](https://www.klayout.de/)                                                   |
| **11. SPICE-Level Simulation (optional)** | **ngspice**                            | Transistor-level circuit simulation                               | [http://ngspice.sourceforge.net/](http://ngspice.sourceforge.net/)                                   |
| **Complete End-to-End Flow**              | **OpenLane**                           | Automates most steps above into one unified flow                  | [https://github.com/The-OpenROAD-Project/OpenLane](https://github.com/The-OpenROAD-Project/OpenLane) |

> *Note: OpenROAD is a customisable digital place-and-route framework, while OpenLane is a user-friendly, automated RTL-to-GDSII flow built on top of OpenROAD, optimized for power, performance, and area (PPA) trade-offs.*

### OpenLane
OpenLane is a streamlined, automated digital ASIC flow built on top of multiple open-source tools such as Yosys, OpenROAD, Magic, Netgen, KLayout, and others. It supports process such as RTL synthesis, Floorplanning, Placement & routing, Timing and DRC checks, and GDSII generation. 

OpenLane is commonly used with Sky130 for hands-on VLSI learning and tapeout preparation. The process flow of OpenLane is depicted in the image below, where required design is provided as input to the OpenLane Flow and it generates GDSII files incorporating all design rules specific to the assigned PDK.   

<img width="1100" height="779" alt="Image" src="https://github.com/user-attachments/assets/8f0bc1a0-d108-45ea-a196-c3e90d2c2a84" />


> *LibreLane, by FOSSi Foundation, is the modern successor to the original OpenLane RTL-to-GDSII flow. While OpenLane has monolithic architecture, LibreLane has a fully modular architecture allows easy plug-in, replace or configure individual tools, better performance and maintainability.*
https://github.com/librelane/librelane.git

### Sky130 PDK

The SkyWater 130nm PDK provides all the design rules, device models, libraries, and data required to fabricate a chip at 130nm node. As an open-source PDK, it enables anyone to design and test ASICs without any costs.
Some key resources for documentation related to Sky130 PDK are listed in the followiing table.

| **Resource**                           | **Link**                                                                                             | **Description**                                                                                       |
| -------------------------------------- | ---------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------- |
| **Sky130 PDK Documentation**           | [https://skywater-pdk.readthedocs.io](https://skywater-pdk.readthedocs.io)                           | Official documentation covering PDK structure, design rules, device information, and library details. |
| **Sky130 PDK GitHub Repository**       | [https://github.com/google/skywater-pdk](https://github.com/google/skywater-pdk)                     | Source files for the PDK, including SPICE models, DRC/LVS rules, and standard cell libraries.         |
| **OpenPDKs (Sky130 Build System)**     | [https://github.com/RTimothyEdwards/open_pdks](https://github.com/RTimothyEdwards/open_pdks)         | Tools and scripts used to build and install the Sky130 PDK for Magic, KLayout, and OpenLane.          |
| **OpenLane (Sky130 Flow Integration)** | [https://github.com/The-OpenROAD-Project/OpenLane](https://github.com/The-OpenROAD-Project/OpenLane) | Demonstrates how Sky130 is used within the OpenLane RTL-to-GDSII digital design flow.                 |

>
> *There are a few other open-source PDKs available publically such as;*
>- *GF180MCU PDK (GlobalFoundries 180nm) – A fully open PDK offering analog, mixed-signal, and MCU-friendly features.* https://gf180mcu-pdk.readthedocs.io
>- *IHP Open Source PDK (SG13G2 – 130nm SiGe BiCMOS) – Open components for analog and RF SiGe processes.* https://github.com/IHP-GmbH/IHP-Open-PDK
> 

## Section 1: Initializing OpenLane & Synthesizing a Design

To invoke OpenLane, navigate to ```openlane/``` directory and initialize **Docker** from the current working directory.<br>
```
# Change directory to openlane flow directory
cd Desktop/work/tools/openlane_working_dir/openlane/
```

![Image](https://github.com/user-attachments/assets/aaf54ae1-8a30-4309-aff7-5d1c588b6301)


```
# Initialize docker in openlane directory
docker
```
The OpenLane flow can be invoked by executing the following command:<br>

```./flow.tcl -interactive``` <br>

This command initializes the OpenLane flow in *interactive* mode, enabling command execution and interaction with the design.

Now that the OpenLane flow is running interactively, input the require package for its function: <br>

```
# Package
package require openlane 0.9
```

Next, prepare the design for the flow. In this exercise, *picorv32a* is selected and can be prepared as following:

```
# Prepare picorv32a design for the flow 
prep -design picorv32a
```

![Image](https://github.com/user-attachments/assets/d130a938-2fc5-48a3-851f-7b257cddd49a)
<br>

> *Note: OpenLane has 42 different designs in  ```.../openlane_working_dir/openlane/designs``` directory which can be explored by users.*<br>

At the *prep* stage, OpenLane performs several tasks to prepare the design for the subsequent stages of the flow. for example, it merges the LEF (Library Exchange Format) files (i.e., technology LEF `.tlef` and cell LEF `.lef`) into a single file, which contain the physical library data and is then used in the design flow.<br>
Further, a new directory will be created in `.../openlane/designs/picorv32a/runs`with date-time of the prepration as name of the directory. This directory contains sub-directories for reports generated at each step along the flow.<br>
The merged file `merged_unpadded.lef` can be accessed `../results/synthesis/` and read by:

```
# Open mergerd LEF file
less merged_unpadded.lef
``` 

Now the design is ready for synthesis. Initilized it by executing the following command: <br>


```
# Run Synthesis of design
run_synthesis
```

During synthesis, OpenLane uses **Yosys** and **ABC** to convert the RTL Verilog design into a *gate-level netlist* mapped to Sky130 standard cells. The flow optimizes logic, removes redundancies, performs timing checks with OpenSTA, and generates reports that guide the next stages of the flow. These reports can be accessed in `../reports/synthesis` directory. <br>
<br>
For example, parameters like **Flop Ratio** and **DFF%** can be calcuated from statistics report `1-yosys_dff.stat` as shown in the following screenshot.

![Image](https://github.com/user-attachments/assets/f119a59a-68d7-422f-acb5-fb7393413970)



>*In an ASIC synthesis, *Flop Ratio* and *DFF%* help to estimate how sequential-heavy a design is. They are derived from the synthesis statistics report, which lists the total number of standard cells and the number of flip-flops (DFFs) used in the netlist.*<br>

#### Flop Ratio

Flop Ratio indicates how many flip-flops exist relative to the total number of cells.

$$
\text{Flop Ratio} = \frac{\text{Number of DFF cells}}{\text{Total number of cells}}
$$

$$
\text{Flop Ratio} = \frac{1613}{18036} = 0.089
$$

As per the above synthesis report, the **Flop Ratio** is calculated to be **0.089** in the *picorv32a* design.

#### DFF%

This expresses the flip-flop count as a percentage of the total cell count.

$$
\text{DFF\%} = \frac{\text{Number of DFF cells}}{\text{Total number of cells}} \times 100
$$

$$
\text{DFF\%} = \frac{1613}{18036} \times 100 = 8.9\%
$$

As per the above synthesis report, the **DFF%** is calculated to be **8.9%** in this design.

## Section 2: Floorplan & Placement in OpenLane Flow
### Initializing Floorplan
The Floorplan can be initialized by execuiting following command in *docker*:
```
# Run floorplan
% run_floorplan
```

Once the floorplan completes, the layout can be opened in *Magic* by executing following command from `.../results/floorplan/` directory:

```
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &
```

![Image](https://github.com/user-attachments/assets/d023a6d6-101b-4709-a5f4-b96c7602e7f1)


Review the floorplan layout in *Magic*.

![Image](https://github.com/user-attachments/assets/fa879954-2f0a-46ab-a493-15fa98dabb3d)

>*Some basic *Magic* shortkeys to navigate around.* `v` *to center/recenter view, use left mouse click + right mouse click to draw a selection box,* `z` *to zoom in at cursor locationv,* `shift + z` *to zoom out.*

`tkcon`commandline can be used to analyze the layout/geometry by using different commands, such as `% what` to get information of a geometry at cursor location and `% box` to get the coordinates and size of selection box, etc.

![Image](https://github.com/user-attachments/assets/513921a4-53fb-4569-a123-1824cd0e213c)

The die area can be found in `.../results/floorplan/picorv32a.floorplan.def` file.

![Image](https://github.com/user-attachments/assets/aa349fa9-1f9c-4c65-ac20-055732bb168c)



In the `floorplan.def` file, die area is given in the form of die coordinates in data units where *`1 micron is equal to 1000 units`*.

>*Tip: Alternatively, to get the area and coordinates of a complete layout, select the whole layout in Magic by short key* `s` *and running* `% box` *in tkcon commandline.* 

### Initilizing Placement
During floorplanning, OpenLane prepares the physical layout environment for placement and routing. It comprises of the following key stages:

1. Die area and core area definition
2. IO pin placement
3. Tapcell (ensures latch-up prevention and substrate/well ties) and Decap cell (ensures stable local power supply rails) insertion
4. Power delivery network (PDN) generation 

The Placement can be initialized by execuiting following command in *docker*:
```
# Run Placement
% run_placement
```

Once placement finishes, its `.def` file can be loaded in *Magic* for further analyses for the director `.../results/floorplan/picorv32a.placement.def` file.

```
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &
```

![Image](https://github.com/user-attachments/assets/f029f446-5226-47c2-af89-dae3f952105f)

![Image](https://github.com/user-attachments/assets/c2795218-61ad-468f-b76f-501746d779c1)


