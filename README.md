# Openlane-Sky130-Workshop
---
## Sky130 Day1-Inception of open-source EDA, OpenLANE and Sky130 PDK
---
### How to Talk to Computers

### 1. Introduction to QFN-48 Package, Chip, Pads, Core, Die, and IPs

Most of us have encountered an Arduino board at some point (as shown below). It is a microcontroller-based development platform that simplifies the creation of electronic projects. The board integrates a programmable chip (highlighted in the encircled area) with a PCB, input/output pins, a USB interface, power regulators, and various supporting components. In this project, we will explore how to design such a microprocessor chip—starting from modeling its specifications using the C language, developing its RTL (in Verilog/VHDL), and finally generating the layout in GDSII file format, which is sent to the foundry.

<img width="709" alt="Screenshot 2025-07-08 at 12 23 49 PM" src="https://github.com/user-attachments/assets/0c40822a-32aa-4068-ac80-6e40b1532433" />

The above ARDUINO BOARD can also be described in the form of a block diagram. Showing the main processor(chip) along with various interfaces.

<img width="1359" alt="Screenshot 2025-07-08 at 1 07 58 PM" src="https://github.com/user-attachments/assets/fc51f006-eb57-48bb-a770-7d5254bd67c0" />

COMPONENTS OF CHIP:
The chip is a QFD-48 package, QFD means 'Quad-Flat No-lead', which has terminals on four side with no pins. It includes 48 contacts which are metal pads.
1. Pads-Through whcih we can send signals inside and outside the chip.
2. Core- Place where digital logic gates are fixed (eg.- MUX, AND gate, Or gate, etc.)
3. Die- It is the size of the entire chip, gets manufactured on silicon wafer.

<img width="866" alt="Screenshot 2025-07-08 at 1 31 47 PM" src="https://github.com/user-attachments/assets/1ecc9f2b-e47a-4386-b73d-cafe6fc029ce" />

A typical chip consists of RISC-V SOC, SRAM, ADCs, DACs, PLL, and SPI.

<img width="1343" alt="Screenshot 2025-07-08 at 1 32 20 PM" src="https://github.com/user-attachments/assets/656c95b9-56ae-47f0-8bd0-fcfa9d804f9d" />

## 2. Introduction to RISC-V

 RISC-V (Reduced Instruction Set Computer) is an open-source instruction set architecture (ISA) built on RISC design principles. Developed at the University of California, Berkeley, it is designed to be simple, modular, and royalty-free, making it well-suited for research, industry, and education. An ISA (Instruction Set Architecture) defines the interface between software and hardware—essentially, how to communicate with computers. It includes a set of basic instructions for performing integer operations.
The process begins with writing a program in C, which is then translated into Assembly Language → converted into Machine Language → then into Binary format → and finally, these bits are executed in the layout.

<img width="1680" alt="Screenshot 2025-07-08 at 1 45 49 PM" src="https://github.com/user-attachments/assets/0f1bb582-8b27-4292-8848-abe3d7e18c1e" />

## 3. From Software Applications to Hardware
   Applications we use on a system ultimately run on the underlying hardware—but how does this happen? Here, we aim to understand this process.
   The application software first passes through a block called "System Software," which converts it into binary language.
   System Software consists of three main components: the Operating System, Compiler, and Assembler.

* **Operating System** – The OS manages all system operations, allocates memory, and handles application execution. One of its roles is to take the application and convert it into an Assembly Language program, which is then transformed into a binary program understandable by the hardware.
* **Compiler** – The OS outputs the program in a high-level language such as C++, C, or Java. The compiler then translates this into a set of specific instructions (e.g., an `.exe` file). The instruction format depends on the hardware; for instance, if the hardware uses RISC-V, the instructions will follow the RISC-V format.
* **Assembler** – After the compiler generates the instruction set, the assembler converts these instructions into binary numbers, known as the Machine Language Program.
  Finally, the binary data is sent to the hardware for execution.

<img width="1680" alt="Screenshot 2025-07-08 at 7 23 01 PM" src="https://github.com/user-attachments/assets/e35b3a0a-b44b-48b3-8d68-a70941acaed1" />

<img width="1680" alt="Screenshot 2025-07-08 at 7 25 34 PM" src="https://github.com/user-attachments/assets/6b747596-b4cc-4318-8245-3e7aeb4380c6" />

<img width="1680" alt="Screenshot 2025-07-08 at 7 26 16 PM" src="https://github.com/user-attachments/assets/920861b0-94fc-4c31-9e33-a0f8bbf96618" />

Instructions form an abstract interface between the compiler and hardware, known as the *Instruction Set Architecture (ISA)* or *Computer Architecture* (**PART-1**).
To implement these instructions, we use RTL (Register Transfer Level), which is then synthesized into logic gates (**PART-2: RTL and Synthesis of RISC-V CPU Core – picorv32**).
Finally, the design is physically implemented on hardware (**PART-3: Physical Design of picorv32**).

![Uploading Screenshot 2025-07-08 at 7.39.24 PM.png…]()

## SoC Design and OpenLANE

## 1. Introduction to All Components of Open-Source Digital ASIC Design**
   SoC (System on Chip) design using OpenLane involves creating a complete integrated chip—including processor cores, memory blocks, and I/O interfaces—through the OpenLane open-source digital ASIC design flow. OpenLane automates the entire RTL-to-GDSII process to generate silicon-ready layout files.
   Key requirements for ASIC design include: RTL IPs, EDA tools, and PDK data.

   <img width="791" alt="Screenshot 2025-07-09 at 11 08 11 AM" src="https://github.com/user-attachments/assets/4ea47002-0ba5-48cb-965c-72461df5e8cf" />

**RTL IPs, EDA Tools, and PDKs**

* **RTL IPs (Register Transfer Level Intellectual Property)** are reusable hardware design blocks described in RTL, typically using Verilog or VHDL. An example is the RISC-V CPU core like *picorv32*.
* **EDA (Electronic Design Automation) Tools** are software tools used to design, verify, and simulate electronic systems such as ASICs, SoCs, and PCBs. They automate tasks across the design flow—from RTL coding to GDSII layout generation.
* **PDK (Process Design Kit)** is a foundry-provided, technology-specific toolkit that includes all necessary data to design chips using a particular manufacturing process.
  Example: *SkyWater 130nm PDK (Sky130)* is an open-source PDK widely used with tools like OpenLane. It serves as an interface between the fab and chip designers. While more advanced nodes like 5nm are expensive, Sky130 remains relevant where high performance isn't critical—for example, *Intel P4EE @ 3.46GHz (Q4’04)* used 130nm.

**How They Work Together:**
**RTL IPs** ──▶ **EDA Tools** ──▶ **GDSII Layout** ──▶ **Foundry (using PDK)**

## 2.Simplified RTL2GDS flow

<img width="1059" alt="Screenshot 2025-07-09 at 11 13 36 AM" src="https://github.com/user-attachments/assets/388012ae-2f69-4ec7-b919-3229126721fc" />

**PDK Flow: RTL to GDSII**

**STEP 1: SYNTHESIS**
Synthesis converts the RTL design into a gate-level circuit using components from the Standard Cell Library (SCL). The output is a gate-level netlist written in HDL, which is functionally equivalent to the original RTL. Standard cells have regular layouts with variable or discrete widths and come with different views/models, such as:

**STEP 2: FLOOR AND POWER PLANNING**
Floorplanning and power planning are essential steps in the early phase of physical design. They define the chip’s physical structure and ensure power integrity before placement and routing begin.

* If implementing a single component, it is termed **Macro** planning. If the entire chip is designed, it is **Chip** planning.
* **Chip Floorplanning** involves partitioning the chip die into various system blocks and placing I/O pads.
* **Macro Floorplanning** sets the macro block dimensions, pin locations, and defines standard cell rows.
  The goal is to efficiently plan silicon area and establish a robust power distribution network.

  Macro Floor planning- Defines the macro dimensions, pin locations and rows.

  <img width="1051" alt="Screenshot 2025-07-09 at 11 17 02 AM" src="https://github.com/user-attachments/assets/909b8326-a4aa-4f42-805f-97fdd1d54e20" />

Power planning- The power netweork is constructed, typically a chip is powered by multiple VDD and GND power pins. The power pins are connected to all components through power rings and horizontal and vertical power straps. Such parallel structures are meant to reduce resistance and also addresses the problem of electromigration.

<img width="833" alt="Screenshot 2025-07-09 at 11 17 31 AM" src="https://github.com/user-attachments/assets/20c8ba7c-5eff-44b4-b9ad-6d68a3ad8573" />

STEP3: PLACEMENT- We place the gate level netlist cells on the floorplan rows, aligned with the sites to reduce interconnected delays and enable successful routing. Done in Two steps; Global followed by Detailed Placement

<img width="1008" alt="Screenshot 2025-07-09 at 11 18 13 AM" src="https://github.com/user-attachments/assets/f6c02eac-ffd8-4714-afa6-7d04745e45c3" />

Global Placement- Global placement tries to find the optimum positions for all cells, not necessarily legal. The main purpose is to find the approximate locations for all cells to minimize wirelength and congestion. Cells may overlap or may go off rows.
Detailed Placement- Adjusts cell positions to legal locations on standard cell rows and ensures there are no overlaps. The placement obtained from global placement are altered to make it legal.

<img width="909" alt="Screenshot 2025-07-09 at 11 19 02 AM" src="https://github.com/user-attachments/assets/00981066-f87c-4254-a2f6-9f1ee8ff298f" />

**STEP 4: CLOCK TREE SYNTHESIS (CTS)**
After placement, but before routing signals, the **clock** must be routed. Clock Tree Synthesis ensures that the clock signal reaches all sequential elements (e.g., flip-flops) with minimal skew. Uneven clock arrival times at different registers—called **clock skew**—can cause timing violations and functional errors. To prevent this, CTS balances the clock distribution, often using structured shapes like **H-tree** or **X-tree**.

**STEP 5: ROUTING**
Once the clock is routed, signal routing begins. Routing creates the physical metal connections between placed cells, macros, and I/O pins, as defined by the synthesized netlist.
The goal is to connect all logic elements (standard cells, flip-flops, buffers) while adhering to design rules.
Given the placements and limited metal layers, the router finds valid horizontal and vertical wiring patterns using metal tracks defined in the **PDK**.
For each metal layer, the PDK specifies:

Routing finalizes the layout and is a crucial step before **tape-out**.

<img width="1035" alt="Screenshot 2025-07-09 at 11 20 14 AM" src="https://github.com/user-attachments/assets/d2b90a04-829d-4e83-a1bb-1573b31058ce" />

The **Sky130 PDK** defines six routing layers. The lowest among them is the *Local Interconnect Layer*, which includes a **TiN (Titanium Nitride)** layer. The next five layers are **Aluminum** metal layers used for signal routing.

Routers used in ASIC design are typically **Grid Routers**, which build a routing grid based on the tracks of the metal layers. Since the routing grid is large and complex, a **“Divide and Conquer”** approach is used to manage it efficiently.

There are two main types of routing. **Global Routing** provides an estimate of routing paths by reserving resources without placing the actual wires. It serves as a rough guide for the final routing. **Detailed Routing** then uses these guides to place the actual wires while ensuring that the design follows all **Design Rule Checks (DRC)** as defined by the PDK, such as spacing, width, and via requirements.

**STEP 6: SIGN-OFF**
After routing is completed, the design enters the **Sign-Off** stage, where final checks are performed to ensure correctness and manufacturability. This includes both **physical** and **timing** verification.

**Physical verification** involves **Design Rule Checking (DRC)** and **Layout vs Schematic (LVS)**. DRC ensures that the layout follows all manufacturing rules defined in the PDK, while LVS checks that the layout matches the gate-level netlist, confirming the design’s functional correctness.

**Timing verification** is done using **Static Timing Analysis (STA)**, which verifies that all timing constraints are met across all possible paths, ensuring the chip will operate reliably at the intended clock frequency.

**3. Introduction to OpenLANE and Strive Chipsets**
To achieve a fully open-source ASIC design flow, we use **OpenLANE**, an open-source, automated RTL-to-GDSII flow for digital ASIC development. It is part of the **OpenROAD** and **SkyWater PDK** ecosystem and allows users to go from RTL (e.g., Verilog) to a tapeout-ready **GDSII** layout using only open-source tools. OpenLANE integrates tools for every step of the ASIC design flow and was developed to support true open-source tape-out experiments.

At **Fabless**, there is a family of open-source SoCs called **Strive**, which represents the *"Open Everything"* philosophy—combining open RTL, open EDA tools, and open PDKs.

<img width="1680" alt="Screenshot 2025-07-09 at 12 28 59 PM" src="https://github.com/user-attachments/assets/32431aef-5284-4edd-a257-445c100bee46" />

The main goal of OpenLANE is to produce clean GDSII with no human intervention(no-human-in-the-loop)
Clean means:
No LVS violations
No DRC violations
No Timing violations

It is tuned for skyWater130 nm open PDK. Also supports XFAB180 and GF130. It can be used to harden(implement) Macrso and Chips.
It has two modes of operation: Autonomous and Interactive.
OpenLANE comes with large number of design examples , currently there are 43 designs with their best configurations.

**4. Introduction to OpenLANE detailed ASIC design flow**

The **OpenLANE ASIC flow** consists of multiple steps, beginning with the **RTL design** and ending with the final layout in **GDSII** format. To function, it relies on a **PDK**. OpenLANE is built upon several open-source projects, including **OpenROAD**, **Yosys**, **ABC**, **Qflow**, and others.

The flow starts with **RTL synthesis**, where the RTL code is passed to **Yosys** along with design constraints. Yosys converts the RTL into logic circuits, which can then be optimized using the **ABC** tool. ABC requires guidance during optimization, provided through an **ABC script**. Since different designs have different requirements, various strategies can be applied—this is supported by the **Synthesis Exploration** utility, which helps generate optimization reports.

OpenLANE also includes a **Design Exploration** utility, used to sweep through various design configurations. Additionally, it supports **Regression Testing (CI)**, where OpenLANE is run on around **70 designs**, and the results are compared to identify the best-performing configurations.

<img width="1680" alt="Screenshot 2025-07-09 at 12 35 11 PM" src="https://github.com/user-attachments/assets/e3809e27-f034-45c2-89c2-16c26e639d1c" />

<img width="1680" alt="Screenshot 2025-07-09 at 12 41 36 PM" src="https://github.com/user-attachments/assets/abe7a0fc-379a-431e-bdbf-106981af99d8" />

Next step is testing or DFT(Design For Testing) which uses the open-source tool 'Fault' to perform: Scan Insertion, Automatic test pattern Generation(ATPG), Test pattern compaction, Fault Coverage and fault Simulation.

<img width="619" alt="Screenshot 2025-07-09 at 12 42 20 PM" src="https://github.com/user-attachments/assets/3fa9813d-f00d-4646-a584-b836e8d5dde6" />

**Physical Implementation in OpenLANE**
The physical implementation stage uses the **OpenROAD app** to perform several critical tasks, including:

* **Floor and Power Planning**
* **End Decoupling Capacitors and Tap Cells Insertion**
* **Placement** (both global and detailed)
* **Post-Placement Optimization**
* **Clock Tree Synthesis (CTS)**
* **Routing** (global and detailed)

Each time the **netlist** is modified—such as after CTS or post-placement optimization—it must be verified to ensure functional correctness. This is done using **LEC (Logic Equivalence Checking)**, which formally verifies that the functionality remains unchanged after modifications.

One issue that can arise during physical implementation is **Antenna Rule Violation**. This occurs when a segment of metal wire acts as an antenna, unintentionally accumulating charge during fabrication, potentially damaging the gate of a connected MOSFET. To prevent this, wire length profiles must be carefully managed and corrected as needed.

<img width="572" alt="Screenshot 2025-07-09 at 12 45 00 PM" src="https://github.com/user-attachments/assets/b99390e6-ea46-4c9a-a640-191e8e1d98c7" />

To avoid this, there are two solutions:
Bridging: Bridging attaches a higher layer intermidiary.
Add Antenna diode cell to leak away charges, antenna diodes are provides by SCL(Standard cell library).

<img width="993" alt="Screenshot 2025-07-09 at 12 45 36 PM" src="https://github.com/user-attachments/assets/8011854a-a83e-4c08-a041-7a56c78ecd4b" />

We can also take Preventive Approach:
Add a Fake Antenna diode next to every cell input after the placement. Run the Antenna checker (Magic) on the routed layout. If the checker reports the violation on the cell input pin, replace the fake diode with a real one.

<img width="418" alt="Screenshot 2025-07-09 at 12 46 00 PM" src="https://github.com/user-attachments/assets/83089dd0-629f-4905-b69f-899350389b45" />

**Sign-Off in OpenLANE**
The final step includes **STA**, **DRC**, and **LVS** checks.

**STA (Static Timing Analysis)** uses RC extraction (via **DEF2SPEF**) from the routed layout and runs on **OpenSTA** to detect timing violations.

**DRC** is performed using **Magic** to check layout rule compliance, while **LVS** ensures the layout matches the schematic using **Magic** (for SPICE extraction) and **Netgen**.

These steps ensure the design is timing-correct, rule-compliant, and functionally accurate.
