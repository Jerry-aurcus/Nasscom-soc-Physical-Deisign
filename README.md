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

# Get familiar with open-source EDA tools

## 1. OpenLANE Directory structure in detail
   Using basic Linux commands, we will work within the 'sky130\_fd\_sc\_hd' directory located inside the 'libs.ref' folder under the 'pdks' directory.
   In this context:

* `sky130` refers to the PDK name
* `fd` represents the foundry
* `sc` stands for standard cell
* `hd` denotes the high-density variant

![WhatsApp Image 2025-07-11 at 13 00 50 (3)](https://github.com/user-attachments/assets/b940775f-896e-43cd-ae24-a19726d1872b)

## 2. Design Preparation Step
   We will now run OpenLane. After navigating to the OpenLane directory, type `docker`.
   (Docker is an open-source platform that lets you build, run, and manage lightweight, portable containers for applications. It packages an application along with its dependencies and runs it reliably across different environments.)

   ![WhatsApp Image 2025-07-11 at 13 00 49](https://github.com/user-attachments/assets/2612951b-3204-49c0-9fda-d78e28ceea7f)
   
Everytime while running the openlane we need to install the package which is required, here 'package require openlane 0.9'.
OpenLane has it's own built in designs, here we will deal with 'picorv32a' design

Next, we will run `flow.tcl` using the interactive switch to execute the flow step by step.

![WhatsApp Image 2025-07-11 at 13 00 50 (1)](https://github.com/user-attachments/assets/8289ff46-9535-4a36-be37-c259d76b1a86)

In picorv32a we have 'src' file which has the RTL netlist
Also there is 'config.tcl' which bypasses any configuration that has been done in openlane. Many of the switches use the default that has been present in the openlane source.It overwrites the settings and become specific to the design

![WhatsApp Image 2025-07-11 at 15 29 03 (1)](https://github.com/user-attachments/assets/16b92b88-d55c-44ac-82a8-749c7502ff63)

![WhatsApp Image 2025-07-11 at 15 29 03](https://github.com/user-attachments/assets/bb254f25-167a-4ac7-bcbf-d9b72feada74)

Here, the RTL file, SDC file, clock period, and filename have already been set. However, when running a custom design, the `sky130_fd_sc_hd_config.tcl` file may not be present.

OpenLane follows a specific priority order for configuration values:

1. Default values (set internally in OpenLane)
2. `config.tcl`
3. `sky130_fd_sc_hd_config.tcl`

The highest priority is given to `sky130_fd_sc_hd_config.tcl`, which will override both `config.tcl` and the default values.

This completes the design configuration part.
Next, we need to set up the file system specific to the flow. This setup is fetched from a particular location in OpenLane using the command:

```bash
prep -design picorv32a
```
![WhatsApp Image 2025-07-11 at 15 33 35](https://github.com/user-attachments/assets/f905198b-9b54-4c28-ac2b-e02a70d07f67)

##3. Review files after design prep and run synthesis

After preparation is done, in picorv32a folder, runs directory is being created with today's date and time.

![WhatsApp Image 2025-07-11 at 16 16 37 (1)](https://github.com/user-attachments/assets/d2d576cb-5376-43b3-91c0-81d487e033d2)

When we enter the date-created folder, we will find all the folder structures required by OpenLane. Every folder except `tmp` will be empty.

The `tmp` folder is where all files are stored during the flow.

Inside `tmp`, use the command `less merged.lef` to view the file.
This `merged.lef` file is created during the preparation step and contains information such as wire layers, layer levels, vias, and cell-level data.
Inside the date-created folder, we will find the `results` and `reports` directories, which include subdirectories for synthesis, floorplanning, routing, and so on. Since synthesis has not yet been started, these folders will be empty.

Along with these, we will also see the `config.tcl` file, which displays all the default parameters being used for the run.

![WhatsApp Image 2025-07-11 at 16 16 37](https://github.com/user-attachments/assets/ce92d522-b999-4490-94df-94809fe0e659)

Now coming back to openlane prompt, after preparation we will go for synthesis by giving command: run_synthesis

![WhatsApp Image 2025-07-11 at 15 57 08](https://github.com/user-attachments/assets/996ad6fb-3792-4692-acf5-1ad66365d034)

Here you can see that the synthesis is completed

# 4.OpenLane project Git Link description
On google you can search for openlane efabless-->click on the github link

<img width="1680" height="1050" alt="Screenshot 2025-07-11 at 4 24 38 PM" src="https://github.com/user-attachments/assets/1ffb8e90-565a-4ea4-8498-736f6b5ce3c1" />

# 5. Steps to characterize synthesis results
After synthesis our first step would be to calculate the Flip Ratio;
Flip Ratio=no. of D flip flops/ No. of cells

![WhatsApp Image 2025-07-11 at 16 52 41 (3)](https://github.com/user-attachments/assets/85e61e3f-ee63-44b1-a45e-6994bee6adcf)

![WhatsApp Image 2025-07-11 at 16 52 42](https://github.com/user-attachments/assets/3346bf6d-b7b6-41f2-a81d-fee328b21a2c)

![WhatsApp Image 2025-07-11 at 16 52 41](https://github.com/user-attachments/assets/5854b0e5-14fc-485e-ba37-c7291ff818f4)

Here the number of D flip flop=1613
No. of cells=14876

Therefore, Flop Ratio=1613/14873=0.1084515

Flip RAtio%= 10.845%

In the results file, we can see inside synthesis, if we get the picorv32a.synthesis.v that means synthesis is complete

![WhatsApp Image 2025-07-11 at 16 52 41 (1)](https://github.com/user-attachments/assets/5970565d-1297-4c75-84e0-464b51c459d5)

![WhatsApp Image 2025-07-11 at 16 52 42 (1)](https://github.com/user-attachments/assets/42f90b50-d894-43f8-9a13-29be5c62b702)

![WhatsApp Image 2025-07-11 at 16 52 43](https://github.com/user-attachments/assets/ee852723-537b-4bd9-9491-c376ab0ba1d6)


---
# Sky130 Day 2 - Good floorplan vs bad floorplan and introduction to library cells
---

## Chip Floor planning consideration
## 1. Utilization factor and aspect ratio
In this the first step in the physical design is to DECIDE THE HEIGHT AND WIDTH OF CORE AND DIE. We will start with the basic netlist.

<img width="1680" height="1050" alt="Screenshot 2025-07-11 at 5 37 59 PM" src="https://github.com/user-attachments/assets/84bc1125-e38b-49f5-af05-cdf3430e5492" />

Considering the basic netlist, it consists of two flip-flops (launch clock and capture clock), a gate, and an OR gate. The given image represents a netlist — a *netlist* defines the connectivity between all components in the design.

We are dependent on the physical dimensions of the logic gates and flip-flops. The aim is to assign appropriate length and breadth to each of these gates in order to accurately represent them in the physical layout.

<img width="1680" height="1050" alt="Screenshot 2025-07-11 at 5 40 31 PM" src="https://github.com/user-attachments/assets/20a6b04b-ea2d-47db-8aba-78ff241908ca" />

Next, we are primarily interested in finding the dimensions of the **core** and **die**, rather than focusing on the wires at this stage. To begin with, we will determine the dimensions of the **standard cells**.

Assuming each standard cell has dimensions of **1 unit × 1 unit**, the area of a single cell is:

**Area = 1 sq. unit**

Using the netlist, we can identify the total area occupied by the standard cells on the silicon wafer. Before doing so, we remove the wires and place the standard cells closely together (as shown in the diagram).

Now, the total area occupied by the netlist becomes:

**Area = 2 units × 2 units = 4 sq. units**


<img width="1680" height="1050" alt="Screenshot 2025-07-11 at 5 42 32 PM" src="https://github.com/user-attachments/assets/ce45f423-d839-445e-971d-022f754b3f20" />

**What is a core and a die?**
On a silicon wafer, one section is called a **die**. Inside the die, there is a **core**.

A **core** is the section of the chip where the **fundamental logic of the design is placed**.

A **die**, which contains the core, is a **small piece of semiconductor material** on which the **fundamental circuit is fabricated**.

<img width="1680" height="1050" alt="Screenshot 2025-07-11 at 5 45 00 PM" src="https://github.com/user-attachments/assets/56dbd82d-f664-4884-b848-c5e9873b63ae" />

**How to arrive at the core's dimensions?**
To determine the dimensions of the core, we start by **placing all the logic cells inside the core**.

If all cells fit perfectly and occupy the entire space, this is referred to as **100% utilization** of the core.

This leads to the concept of the **Utilization Factor**, defined as:
**Utilization Factor = (Area occupied by the netlist) / (Total Area of the core)**


<img width="1680" height="1050" alt="Screenshot 2025-07-11 at 5 48 01 PM" src="https://github.com/user-attachments/assets/b2487352-4607-4710-b34a-b436c0542cbe" />

In the above example the Utilization factor=1, but practically only 50-60% utilization is possible.

Another important term is 'Aspect Ratio', which is Height/Width. So in this case the aspect ratio is 1 that means chip is square. If aspect ratio is not equal to 1 that means the chip is rectangle.In such case , the remaining place is optimized by using some other circuitry.

---
## 2. Concept of Pre-Placed Cells
---
Let’s consider an example where the **width and height of the die is 4 units × 4 units**, and the **netlist occupies an area of 2 units × 2 units**.

If we calculate the **Utilization Factor**:
**Utilization Factor = (2 × 2) / (4 × 4) = 4 / 16 = 25%**

This means **only 25%** of the core is occupied by logic cells, while the remaining **75% is empty**, which can be used for **optimization, routing, and wires**.

<img width="1680" height="1050" alt="Screenshot 2025-07-11 at 5 57 33 PM" src="https://github.com/user-attachments/assets/4bf91f54-c107-43ed-b052-63a027c26532" />

**Next step: DEFINE THE LOCATIONS OF PRE-PLACED CELLS**
Before doing that, let’s understand what **pre-placed cells** are.

Consider a **combinational circuit** (which may include components like multiplexers, demultiplexers, encoders, or decoders). Suppose the equivalent circuit consists of **100k logic gates**.

To manage such complexity, we can **divide the total number of gates into smaller groups** and convert them into **separate blocks**. These blocks can then be **implemented and placed individually**.

This approach helps **minimize congestion**, improves **area utilization**, and simplifies **placement and routing**. These blocks are known as **pre-placed cells**.

<img width="1680" height="1050" alt="Screenshot 2025-07-11 at 5 59 43 PM" src="https://github.com/user-attachments/assets/91afabbd-aee7-4d0a-a537-47f569e27dd0" />

Considering two blocks, we **separate the input and output pins** of each block. Then, we **treat these blocks as black boxes**, where the internal logic is hidden and only the I/O pins are exposed for interaction.

The I/O pins of both blocks will be handled **independently**.

The main advantage of this approach is that **we don’t need to implement the same circuit repeatedly**. The **same black box** can be **reused and shared with different users** for various applications.

This reduces the **overall number of logic gates** required and forms the basis of the concept of **Reused Models**.

<img width="1680" height="1050" alt="Screenshot 2025-07-11 at 6 00 29 PM" src="https://github.com/user-attachments/assets/a9cb80bc-8a06-4847-bf23-1ac9ca26a4fd" />

<img width="1680" height="1050" alt="Screenshot 2025-07-11 at 6 01 20 PM" src="https://github.com/user-attachments/assets/d8f800b6-9afe-43ab-8c85-f6148f18393b" />

Therefore, **preplaced cells** are specific standard cells or blocks (typically **macros** or **hard IPs**) that are **manually positioned** during the **floorplanning stage** of an ASIC or SoC design, **before** the automated placement of the remaining standard cells.

This is done to **fix the position** of critical components like **high-performance IPs** or **memory blocks** close to certain logic, in order to **minimize delay**.

Carefully placing large blocks also helps to **reduce routing congestion**.

These cells are positioned in such a way that the **placement and routing tools do not alter their location** during the later stages of the design flow.

<img width="1680" height="1050" alt="Screenshot 2025-07-11 at 6 06 16 PM" src="https://github.com/user-attachments/assets/ffe48a87-0b52-401a-976f-51cd05a32900" />

---
## 3. De-Coupling Capacitors
---

Earlier, we discussed that pre-placed cells have fixed locations and won’t change in later stages.

After placement, **preplaced cells are surrounded by decoupling capacitors**.

When a circuit (e.g., an AND gate) switches from 0 → 1, it demands current. A small capacitor charges to support this transition, with **Vdd** supplying the current.

When switching from 1 → 0, the capacitor discharges, and **Vss** handles this.

Since wires have resistance, inductance, and capacitance, **supply voltage drops** can occur—decoupling capacitors help manage this.

<img width="1680" height="1050" alt="Screenshot 2025-07-11 at 6 16 51 PM" src="https://github.com/user-attachments/assets/5d0dc598-5c44-4a72-b922-2f610fc8575e" />
<img width="1680" height="1050" alt="Screenshot 2025-07-11 at 6 25 20 PM" src="https://github.com/user-attachments/assets/c3bf3aff-ecad-4880-a767-e331ca01c106" />

So if the supply voltage is suppose 1V then due to resistances of wire due to voltage drop the voltage reached is 0.8 or 0.7V (Vdd').
The capacitor will now charge till 0.7V only. Now if the 0.7V lies between the high and low margin region, then it will be a problem as it can switch to 0 or 1 irrespective of the requirement.
This is the problem of having a large distance between the main power supply and the physical circuit.

<img width="1680" height="1050" alt="Screenshot 2025-07-11 at 6 20 25 PM" src="https://github.com/user-attachments/assets/f360fe96-9955-494d-8da2-ccc8c89ef157" />

This problem can be solved using **decoupling capacitors**.

Decoupling capacitors are **large capacitors** charged up to the applied voltage. They are placed **very close to the main circuit** so that there is minimal voltage drop.

The capacitor acts like a **shock absorber** for the chip’s power supply. It smooths out sudden voltage changes, just like a damper absorbs mechanical shocks.

As the name suggests, it **decouples** the main circuit from power supply disturbances.

<img width="1680" height="1050" alt="Screenshot 2025-07-11 at 6 27 13 PM" src="https://github.com/user-attachments/assets/9aa2f328-05ef-4f92-8c29-e19eda805e69" />

Below image shows how the main circuit blocks are surrounded by the decoupled capacitors. This ensures that there is proper switching and no cross-talk.

<img width="1680" height="1050" alt="Screenshot 2025-07-11 at 6 23 52 PM" src="https://github.com/user-attachments/assets/aef33663-4540-46d2-8353-5e63b1af98bf" />

---
## 4.Power Planning
---
Now we have taken care of local communication, but what about the global communication..?
Let us suppose there are multiple macros, and we have connected the decoupling capacitor to all the macros individually. There is a driver connected to load.

<img width="1680" height="1050" alt="Screenshot 2025-07-11 at 7 32 25 PM" src="https://github.com/user-attachments/assets/b69a3374-2ca8-4ea3-888f-05d50d981bff" />

The **macros are connected to the main power supply (Vdd)**.

As shown in the diagram, the **driver and load are connected with a red wire**. We want the logic operation from the driver to be transmitted to the load.

However, there will be a **voltage drop due to the resistance of the wire**.

Also, it is **not feasible to connect decoupling capacitors everywhere**, so they cannot be placed in all such locations.


<img width="1680" height="1050" alt="Screenshot 2025-07-11 at 7 45 03 PM" src="https://github.com/user-attachments/assets/a9778863-ab08-4bcb-a56b-bb85d4ce56bb" />
<img width="1680" height="1050" alt="Screenshot 2025-07-11 at 7 46 02 PM" src="https://github.com/user-attachments/assets/8067e5a8-ed19-4624-923b-0abfeef2544b" />

Let the **red wire represent a 16-bit bus**. Suppose we are giving a 16-bit signal, where for **logic 1**, the capacitors must be **fully charged to V**, and for **logic 0**, the capacitors must be **discharged**.

Now, if we connect an **inverter at the load**, logic 1 must turn to logic 0, meaning the **capacitor voltage must discharge to ground simultaneously**.

Since this discharge happens through a **single ground tap point**, it creates a **bump at the ground** known as **Ground Bounce**.

This bounce may fall **within the noise margin levels**, causing **disruptions in the output values**.

These problems occur because there is **only one power supply**. If there were **multiple power supplies**, such issues wouldn’t arise, as shown in the example below.

Therefore, while designing chips, we provide **multiple power supplies**. This allows any logic to **draw power from its nearest Vdd** and **dump current to its nearest ground**, reducing voltage drops and ground bounce.

<img width="1680" height="1050" alt="Screenshot 2025-07-11 at 7 48 04 PM" src="https://github.com/user-attachments/assets/a95f39b6-0731-4167-8328-26011ce8ee81" />

This is how we do Power Planning by giving horizontal and vertical lines and the interconnects are the contacts.

![Screenshot 2025-07-11 at 7 49 00 PM](https://github.com/user-attachments/assets/e6e79132-14f7-4ed7-adaa-1699f8d15240)

---
## 5. Pin Placement and Logic Cell Placement Blockage
---
Let’s consider an example design that needs to be implemented, including **input-output terminals** and **individual clocks**.

After defining the I/Os and clocks, we proceed to **connect the pre-placed blocks** to the **logic gates placed below** them in the design.

<img width="1680" height="1050" alt="Screenshot 2025-07-11 at 7 56 57 PM" src="https://github.com/user-attachments/assets/6f47a2a4-40e8-4f60-a74e-3ffbfe781245" />

Now, taking one more section of the same circuitry with two different clocks for different FFs, showcasing the concept of 'Interclocks Timing Analysis'.Also, including the pre-placed cells in between.
<img width="3360" height="2100" alt="image" src="https://github.com/user-attachments/assets/ee0006d6-9ce5-4b72-8694-e4fe3bf1a584" />

Showing below the complete design

<img width="1680" height="1050" alt="Screenshot 2025-07-11 at 7 59 56 PM" src="https://github.com/user-attachments/assets/3ba1221c-40d9-4743-991c-250af706bdd6" />

Now, let’s look at how **pin placement** is done.

The **logic circuit is placed between the gap of the core and die**. In this case, the **input port is on the left** and the **output port is on the right**, but this can vary.

A few observations:

1. **Ordering of input and output ports is random**, depending on design requirements. For example, **Block A** is connected to **D1 and D4**, so these are placed nearby. Similarly, **Block B** is connected to **Dout1 and Dout3**.

2. As the **blocks are placed in certain areas**, we must ensure that **cell placement is avoided in those areas**. This is where **hand-checking between frontend and backend teams** is important — the **frontend** team defines netlist connectivity, while the **backend** team defines pin placement.

3. The **size of the clock pins is larger** than the input/output pins. This is because the **clock drives I/O pins, flip-flops, and the full chip**. A larger size ensures a **lower resistance path**, which is necessary for proper operation.

Finally, to ensure that the **pin placement area is not used by the placement and routing tool**, we use **Logical Cell Placement Blockage**.

<img width="1680" height="1050" alt="Screenshot 2025-07-11 at 8 02 44 PM" src="https://github.com/user-attachments/assets/ea7b20a2-2be0-4d1f-86b6-31d7c118b159" />

After the logical cell placement blockage step our floorplan is done for placement and routing step.

<img width="1680" height="1050" alt="Screenshot 2025-07-11 at 8 03 28 PM" src="https://github.com/user-attachments/assets/6c42da2d-5b4a-4a57-a6a2-a996804cdd3e" />

---
## 6. Steps to run floorplan using OpenLANE
---
We will doing the floorplanning in openlane. For Floorplanning we require some switches which we will get in 'configuration' file in openlane.
Inside the configuration there is a README file--> enter into that.

<img width="1646" height="791" alt="Screenshot 2025-07-13 134453" src="https://github.com/user-attachments/assets/c73d0823-f5cb-40df-a0b6-5c01cfca8c61" />

Here, you will find the variables needed for each stage, including global variables like the design name, synthesis variables, and others.
Various switches are provided under floorplanning as shown.

<img width="1278" height="769" alt="Screenshot 2025-07-13 112507" src="https://github.com/user-attachments/assets/111697db-e662-44be-859a-ca1e2e16af12" />

Now we need to set the switches. For that, go back to the README file in the `floorplan.tcl` directory. There, you will see the default switches that are already set. For example, `(FP_IO_MODE): 1` means the I/O pins are positioned randomly but equidistant, while `0` means they are not positioned equidistant.

<img width="1274" height="772" alt="Screenshot 2025-07-13 115528" src="https://github.com/user-attachments/assets/02a70220-a4dd-4349-9e9d-fd383b017382" />
<img width="1272" height="766" alt="Screenshot 2025-07-13 115603" src="https://github.com/user-attachments/assets/7b2318cf-8bfb-45f7-9e2e-f5e48bd3589b" />

As seen earlier in OpenLANE, the lowest priority is given to the system default (`floorplan.tcl`), followed by `config.tcl`, and the highest priority is given to the PDK variant (`sky130A_sky130_fd_sc_hd_config.tcl`).

We will now run the floorplan by using the command: `run_floorplan`.

<img width="1910" height="982" alt="Screenshot 2025-07-13 133313" src="https://github.com/user-attachments/assets/da37b74d-4a8e-4add-95bb-a1885e9e9a8c" />

---
## 7.Review floorplan files and steps to view floorplan
---
As we have run the floorplan, just like we did for synthesis we will go inside picorv32a and check for the present date when the floorplan file was created. Then we will go into the floorplan, and open the directory 'ioplacer.log' and we did the placements in input output.

<img width="1642" height="795" alt="Screenshot 2025-07-13 140933" src="https://github.com/user-attachments/assets/fe04936a-5bca-426c-b0a7-79914d1c496c" />

Inside the configuration, we will see the default `floorplan.tcl` file, which shows the default settings.
To view how the floorplan looks, go to the generated folder by navigating to `floorplan → results → floorplan`. There, you will find a `.def` (Design Exchange Format) file. Open the `.def` file to see the floorplan.

<img width="1913" height="946" alt="Screenshot 2025-07-13 142332" src="https://github.com/user-attachments/assets/72bde0ca-a710-4e13-bdd4-30098a72b640" />

After opening the file, you will find various parameters, including the DIEAREA, which is given in database units.
To convert it into microns, use the conversion:
**1 micron = 1000 database units**.

Given:
`DIEAREA (0 0) (660685 671045)`

Converted:
**Width = 660.685 microns**,
**Height = 671.045 microns**.
<img width="1919" height="1079" alt="Screenshot 2025-07-13 155905" src="https://github.com/user-attachments/assets/516f41e5-22f7-4459-964b-04ab74f5e515" />

To see the actual Floorplan, let us first open Magic by writing the command magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def
We will see the layout in magic

<img width="1919" height="1024" alt="Screenshot 2025-07-13 162306" src="https://github.com/user-attachments/assets/22461ca6-cfcf-454f-84b0-e50e8f68e20a" />
<img width="1919" height="1079" alt="Screenshot 2025-07-13 162240" src="https://github.com/user-attachments/assets/795538cd-196c-4c24-9a4d-6533682ba0a6" />

---
## 8. Review floorplan layout in Magic
--- 
   In the image above, we can see that the layout is not centered. To center and fit it to the window:
   → Full-screen the window
   → Press `s`
   → Press `v`
   The layout will then fit within the window.

To zoom in:
→ First, left-click with the mouse
→ Then right-click and press `z`

To zoom out:
→ Press `Shift + z`

Since we selected `IO_MODE = 1`, the I/O pins are placed equidistant from each other.
To select any pin, hover the mouse over the element and press `s` on the keyboard.

<img width="1919" height="1079" alt="Screenshot 2025-07-13 163850" src="https://github.com/user-attachments/assets/7e040b9a-cb30-4b20-ab18-ecb142a2692c" />

After selecting any pin, there is one more window tkcon, where we can get the information of the selected pin. Just type 'what' in that window.You will see metal3 which means horizontal.

<img width="1895" height="1050" alt="Screenshot 2025-07-13 163947" src="https://github.com/user-attachments/assets/6c921fa1-883a-40a7-b6fd-b0d58fa192d6" />

Similarly, we check for vertical pins we will get metal2 as mentioned in below image.

<img width="1919" height="1079" alt="Screenshot 2025-07-13 164109" src="https://github.com/user-attachments/assets/c75e89bf-4499-4370-b32a-c9a52ca4c619" />

Along with this, we can also see the Decaps (decoupling capacitors) arranged along the border or side rows.
Then we have tap cells, which are used to avoid latch-up conditions in CMOS devices. They connect the n-well to VDD and the substrate to GND.

<img width="1919" height="1079" alt="Screenshot 2025-07-13 164624" src="https://github.com/user-attachments/assets/ddaf30fb-455e-40dc-b22c-495a2390c491" />

We also have the standard cell at the lower left corner which represents the AND, OR,etc logic gates.

<img width="1919" height="1079" alt="Screenshot 2025-07-13 164224" src="https://github.com/user-attachments/assets/44dda5a1-0d5c-4ba1-a212-b00e761ade3c" />

# Library Building and Placement

---
1. Netlist Binding and Initial Place Design
---

In placement and routing, the first step is to bind the physical netlist. In reality, logic gates do not have the exact shapes as shown in schematic diagrams; instead, they are represented as boxes with specific width and height defined during the design phase. At this stage, each component of the netlist is assigned proper physical dimensions.

<img width="1899" height="1079" alt="Screenshot 2025-07-13 165630" src="https://github.com/user-attachments/assets/fae76f36-e25d-44d6-969d-6d7ec63ccbf4" />

Now, the wires are removed and the elements are placed together. These elements are part of a shell called the **"Library."** The library contains timing and physical information. Basically, there are two types of libraries: one provides delay (timing) information, and the other provides shape and size (physical) information of the cells.

The library includes:

* Delay of a particular cell
* Width and height
* Specific operating conditions

The library also offers various size options. For example, in the second case, gates are larger in size, resulting in lower resistance paths and hence faster operation. In the third case, the cells are even larger, making the operation even faster.

<img width="1919" height="1079" alt="Screenshot 2025-07-13 165838" src="https://github.com/user-attachments/assets/a48f13fc-fd79-427d-a3a6-2c934eda324f" />

Now next comes the Placement of the desired netlist on the floorplan. So we have the floorplan, the netlist and the shape of components in netlist.

<img width="1919" height="1079" alt="Screenshot 2025-07-13 171932" src="https://github.com/user-attachments/assets/8bb5f22e-b941-484d-858b-67c533f42e3f" />

Considering the floorplan along with the preplaced cells, we begin placing the flip-flops (FFs) by referring to the netlist. In the netlist, FF1 is close to `Din1` and FF2 is near `Dout1`, so we place them accordingly. They are positioned close to each other to minimize timing delay.

In stage 2 of the logic, you can observe that all the FFs and gates are placed together.

<img width="1918" height="1079" alt="Screenshot 2025-07-13 170332" src="https://github.com/user-attachments/assets/a5e6e64e-f870-4aa9-b22e-96726e15d967" />

---
## 2. Optimize Placement Using Estimated Wire-Length and Capacitance
---
   At this stage, we estimate the wire length and capacitance, and based on that, insert repeaters. For example, from `Din2` to `Dout3`, we estimate the wire length and the corresponding equivalent capacitance. Since the distance is large, both resistance and capacitance will be high, leading to signal loss.

To prevent this, repeaters and buffers are placed at intermediate points to maintain **signal integrity**. However, this comes at the cost of additional area due to the insertion of multiple buffers and repeaters. Still, it is necessary to ensure proper signal transmission.

In stage 1, the FFs are placed close to each other, so there is no need for repeaters (as shown below).


In stage 2 the FF1 is far from Din2 so we need buffers/repeaters in between to maintain the signal integrity. So we place 2 buffers in between.(as shown below)

<img width="1680" height="1050" alt="Screenshot 2025-07-13 at 11 56 27 PM" src="https://github.com/user-attachments/assets/686c69ec-b614-46bf-b1b5-2a3ccb06c15d" />


---
## 3. Final Placement Optimization
---

In Stage 2, notice that there is no gap between the flip-flops (FFs) and the logic gates. This close placement is known as *abutment in placement optimization*. It is used for high-speed (high-frequency) circuits to minimize delay by avoiding wire routing between elements.
Similarly, in Stage 3, a buffer needs to be inserted between Logic Gate 2 and FF2 because the distance between them is relatively large, which could introduce delay.

![Uploading Screenshot 2025-07-14 at 12.02.16 AM.png…]()

Coming to the last stage i.e 4th stage, it is the trickiest one, we placed 2 buffers in between, and also there is a criss-cross with other connections in between. So we need to deal with that also further.

![Uploading Screenshot 2025-07-14 at 12.03.40 AM.png…]()

Now we will try to do the Setip Timing Ananlysis, considering the clocks to be ideal that means giving clock to all the FFs at the same time.

Here’s a clearer and more professional rewrite of both **Sections 4 and 5**, preserving all the original meaning and technical details:

---
## 4. Need for Libraries and Characterization
---

As we progress through the design flow — including Logic Synthesis, Floorplanning, Placement, and eventually Static Timing Analysis (STA) — an essential step we must address is **Clock Tree Synthesis (CTS)**.
To achieve **zero skew**, flip-flops (FFs) across the chip should receive the clock signal at the same time. This synchronization is achieved using **clock buffers**, which help deliver the signal uniformly. This is where **libraries** become crucial.

Across all design stages, one common element is the use of **logic gates or standard cells** (such as AND, OR, INVERTER, BUFFER, etc.). For Electronic Design Automation (EDA) tools to recognize and work with these gates, **library characterization** is necessary.
Library characterization involves modeling the electrical behavior, timing, and other attributes of each gate/cell. This enables the tools to understand how a specific gate functions and interact with it correctly during synthesis, placement, routing, and timing analysis.

---
## 5. Congestion-Aware Placement Using RePlAce
---

At this stage, our focus is on achieving a **congestion-free placement**. Timing analysis will be considered afterward.
As previously discussed, placement occurs in two phases:

* **Global Placement**
* **Detailed Placement**

In **Global Placement**, standard cell positions are optimized, but **legalization** (ensuring legal, manufacturable positions) is not yet enforced.
**Legalization** happens during **Detailed Placement**, where each standard cell is placed precisely within **standard cell rows**, with no overlaps and **abutment** (tight packing) between them. Timing considerations also come into play during legalization.

When running `run_placement` in OpenLane:

Global Placement** occurs first.

   * The goal here is to **minimize wire length**, using the **HPWL (Half-Perimeter Wire Length)** metric.
   * The primary objective is to **reduce congestion** and **converge the overflow**. Once the overflow is minimized, the placement is considered successful.

To **visually verify placement**, navigate to the `results/placement` folder and look for the `placement.def` file.
Open this file in **Magic** using the same technology file (`.tech`) that was used in earlier steps.

Let me know if you'd like this added to a presentation or formatted into slides.

















