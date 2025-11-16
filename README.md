# VSD-coursework
# Digital SoC Design using open-source EDA tools

This repository documents digital SoC design using OpenLane, an open-source RTL-to-GDSII flow built around the SkyWater 130nm (Sky130) PDK.

## Section 1: Inspection of Open-Source EDA, OpenLane, and Sky130 PDK

This section provides a quick overview of ASIC design pipeline and essential components involved in open-source ASIC chip design.

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

> *Note that OpenROAD is a customisable digital place-and-route framework, while OpenLane is a user-friendly, automated RTL-to-GDSII flow built on top of OpenROAD, optimized for power, performance, and area (PPA) trade-offs.*

### OpenLane
OpenLane is a streamlined, automated digital ASIC flow built on top of multiple open-source tools such as Yosys, OpenROAD, Magic, Netgen, KLayout, and others. It supports process such as RTL synthesis, Floorplanning, Placement & routing, Timing and DRC checks, and GDSII generation. 

OpenLane is commonly used with Sky130 for hands-on VLSI learning and tapeout preparation. The process flow of OpenLane is depicted in the image below, where required design is provided as input to the OpenLane Flow and it generates GDSII files incorporating all design rules specific to the assigned PDK.   

<img width="1100" height="779" alt="Image" src="https://github.com/user-attachments/assets/8f0bc1a0-d108-45ea-a196-c3e90d2c2a84" />


> *LibreLane, by FOSSi Foundation, is the modern successor to the original OpenLane RTL-to-GDSII flow. While OpenLane has monolithic architecture, LibreLane has a fully modular architecture allows easy plug-in, replace or configure individual tools, better performance and maintainability.*
https://github.com/librelane/librelane.git

### Sky130 PDK

The SkyWater 130nm PDK provides all the design rules, device models, libraries, and data required to fabricate a chip at 130nm. As an open-source PDK, it enables anyone to design and test ASICs freely.
Some key resources for more documentation are listed in the followiing table.

| **Resource**                           | **Link**                                                                                             | **Description**                                                                                       |
| -------------------------------------- | ---------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------- |
| **Sky130 PDK Documentation**           | [https://skywater-pdk.readthedocs.io](https://skywater-pdk.readthedocs.io)                           | Official documentation covering PDK structure, design rules, device information, and library details. |
| **Sky130 PDK GitHub Repository**       | [https://github.com/google/skywater-pdk](https://github.com/google/skywater-pdk)                     | Source files for the PDK, including SPICE models, DRC/LVS rules, and standard cell libraries.         |
| **OpenPDKs (Sky130 Build System)**     | [https://github.com/RTimothyEdwards/open_pdks](https://github.com/RTimothyEdwards/open_pdks)         | Tools and scripts used to build and install the Sky130 PDK for Magic, KLayout, and OpenLane.          |
| **OpenLane (Sky130 Flow Integration)** | [https://github.com/The-OpenROAD-Project/OpenLane](https://github.com/The-OpenROAD-Project/OpenLane) | Demonstrates how Sky130 is used within the OpenLane RTL-to-GDSII digital design flow.                 |

There are a few other open-source PDKs available such as;
- GF180MCU PDK (GlobalFoundries 180nm) – A fully open PDK offering analog, mixed-signal, and MCU-friendly features. https://gf180mcu-pdk.readthedocs.io
- IHP Open Source PDK (SG13G2 – 130nm SiGe BiCMOS) – Open components for analog and RF SiGe processes. https://github.com/IHP-GmbH/IHP-Open-PDK

