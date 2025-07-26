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



<img width="831" height="590" alt="Screenshot 2025-07-25 at 5 34 30 PM" src="https://github.com/user-attachments/assets/f3cfa7da-8eaf-4e5d-ac37-1a790a777ee6" />


<img width="832" height="589" alt="Screenshot 2025-07-25 at 5 34 41 PM" src="https://github.com/user-attachments/assets/4a131fbc-4986-46b1-8165-e981cba6c92e" />


To view our placement, invoke the same sky130A.tech file and the same merged.lef file but this time our def file will be picorv32a.placement.def


![WhatsApp Image 2025-07-25 at 17 39 21](https://github.com/user-attachments/assets/919fbe5a-dde7-4f18-af39-967373a6272a)


<img width="832" height="433" alt="Screenshot 2025-07-25 at 5 35 17 PM" src="https://github.com/user-attachments/assets/5c361df2-d2ca-40a1-90b4-158d6d06c454" />



Zooming in we can see the placement of the standard cells in the standard cell rows


<img width="831" height="468" alt="Screenshot 2025-07-25 at 5 35 27 PM" src="https://github.com/user-attachments/assets/6293cf78-a03f-4097-9153-0d3c1455711e" />



## Cell Design and Characterization Flows

---
### 1. Inputs for Cell Design Flow
---

In a typical IC design flow, **standard cells** serve as the fundamental building blocks for constructing digital logic. These cells are **pre-designed** and **pre-characterized**, meaning their logical function, timing, power, and physical layout information are already defined and stored in libraries.

**What are Standard Cells?**
Standard cells are a set of reusable logic components used in the **physical design** of integrated circuits. They ensure **design consistency**, **reliability**, and **optimization** for area, power, and performance.

**Common Types of Standard Cells Include:**

* **Logic Gates**: AND, OR, NAND, NOR, NOT, XOR, etc.
* **Sequential Elements**: Flip-flops, Latches
* **Combinational Buffers**: Inverters, Buffers
* **Data Routing**: Multiplexers
* **Special Purpose Cells**:

  * **Tie-high/Tie-low cells**: Used to connect a net permanently to logic ‘1’ or ‘0’
  * **Filler cells**: Used to fill empty spaces in rows to maintain DRC and power rail continuity

These cells are provided as part of a **standard cell library**, which also includes files required for design automation, such as:

* **.lib** (timing and power characterization)
* **.lef** (layout abstract)
* **.gds** (full layout data)
* **.v** (Verilog model for functional simulation)

These inputs are essential for downstream processes like **logic synthesis, placement, routing, and STA**.

<img width="1680" height="1050" alt="Screenshot 2025-07-14 at 12 45 56 AM" src="https://github.com/user-attachments/assets/72d31a55-11a3-44c3-9d70-f7e98cf562da" />

These standard cells are placed in Libraries. A library has got cells with different functionality, and different sizes. Also cells with different threshold voltage(Vt).

<img width="1680" height="1050" alt="Screenshot 2025-07-14 at 12 46 45 AM" src="https://github.com/user-attachments/assets/8e8c7eb7-7968-470d-86e9-02e52886357b" />

Let's take one particular inverter-->see the cell design flow, this inverter should be understood by a particular EDA tools.It has to be represented in form of shape, size and various cell design flow.
Cell design flow is divided into 3 parts: 
a)Inputs
b)Design steps
c)Outputs

<img width="1680" height="1050" alt="Screenshot 2025-07-14 at 1 08 41 AM" src="https://github.com/user-attachments/assets/bd03190b-f714-44a1-a149-61f4d6c03bb3" />

<img width="1680" height="1050" alt="Screenshot 2025-07-14 at 12 59 07 AM" src="https://github.com/user-attachments/assets/d7979a6e-760b-4de8-a13b-aff17b1d5649" />

![Screenshot 2025-07-14 at 1 03 15 AM](https://github.com/user-attachments/assets/1f0af6f3-b17d-44ca-8409-5cd8d30b5df3)

---
## 2. Circuit Design Steps
---

Consider an example where a 'Library' is part of the inputs. The separation between the power rail and ground rail determines the cell height, and it is the responsibility of the cell library to ensure that this height is consistently maintained.

Additionally, if the drive strength of a particular cell is high, it will be capable of driving longer wires effectively.

<img width="1680" height="1050" alt="Screenshot 2025-07-14 at 1 15 54 AM" src="https://github.com/user-attachments/assets/b025bb91-3846-4ccb-ab94-5bc0029657c5" />


User Defined Specifications
The top level of the cell decides at what level the chip will operate.

The library developer has to decide the supply voltage.

The library also has to decide the metal layer and pin locations.

Design Steps
After defining the inputs in the library, the design should adhere to these inputs.

Design involves three steps:

Circuit Design

Implement the circuit.

Model the PMOS and NMOS transistors to meet the library requirements.

Output: CDL (Circuit Description Language).

Layout Design

Characterisation

<img width="1680" height="1050" alt="Screenshot 2025-07-14 at 1 18 37 AM" src="https://github.com/user-attachments/assets/1c0572bf-406e-4fe0-9ff0-a4504409bf9c" />


---
## 3. Layout Design
---
The first step (implementation of the given function) is already discussed.

The second step is to derive the PMOS and NMOS network graphs.

This is done by Art of Layout – Euler’s Path and Stick Diagram.

It gives the best layout and best performance.

After generating the network graphs, we get the Euler’s Path – a path traced only once.

Based on Euler’s Path, we draw the Stick Diagram.

<img width="1680" height="1050" alt="Screenshot 2025-07-14 at 4 33 09 PM" src="https://github.com/user-attachments/assets/be56a559-4fae-4fb9-a8d7-0425f3ce6a3f" />
<img width="1680" height="1050" alt="Screenshot 2025-07-14 at 4 10 33 PM" src="https://github.com/user-attachments/assets/cd450030-de4d-45c0-bcc5-f9fc652bef89" />

is to convert the stick diagram into a proper layout adhering to the DRC rules. We can implement it in magic.(as shown below)

<img width="1680" height="1050" alt="Screenshot 2025-07-14 at 4 13 17 PM" src="https://github.com/user-attachments/assets/a885d13c-6b5b-4ab3-8baf-61492d89f439" />

Final Steps
Extract parasitics (resistance and capacitance) from the layout.

Perform characterisation in terms of timing.

Layout design output is saved as:

  -GDSII

  -LEF

Extracted SPICE netlist

Characterisation
This step provides output in the form of:

  -Timing

  -Noise

  -Power information

---
## 4. Typical Characterisation Flow
---

To build the characterisation flow from the inputs, follow these steps:

a) Read the model files

b) Read the extracted SPICE netlist

c) Recognize the behaviour of buffer

d) Read the sub-circuit of inverter

e) Attach the power sources

f) Apply the stimulus for characterisation

g) Provide the necessary output capacitors

h) Provide the simulation command:
 
  – For transition simulation → .tran
  
  – For DC simulation → .dc

<img width="1031" height="599" alt="Screenshot 2025-07-14 at 4 40 31 PM" src="https://github.com/user-attachments/assets/761bcc5a-6b4f-4b9d-897c-23e055c5ae0f" />
<img width="1026" height="634" alt="Screenshot 2025-07-14 at 4 40 49 PM" src="https://github.com/user-attachments/assets/dc36ac83-edf3-48df-8648-0f60bcc1fa44" />

Next is to feed all these steps in characterisation software called GUNA.This software will generate timing,noise and power.libs outputs

<img width="1037" height="462" alt="Screenshot 2025-07-14 at 4 41 01 PM" src="https://github.com/user-attachments/assets/e279fb44-0a6f-4407-95a4-befe7a7befd1" />


---
## General Timing Characterization Parameters
---

---
## 1. Timing Threshold Definitions
---

* Understand the various **syntax and semantics** of:

  * `timing.lib`
  * `power.lib`
  * `noise.lib`

* This understanding is necessary for working with **GUNA software**.

* Focus is on understanding the **timing threshold definitions** of the **waveform** itself.


<img width="1026" height="606" alt="Screenshot 2025-07-14 at 4 41 21 PM" src="https://github.com/user-attachments/assets/56b1d11e-5e87-4a31-89f0-8d15934a8ece" />

Waveform of output of 1st inverter is given as input to 2nd inverter.
slew_low_rise_thr It is voltage level below which a rising signal is considered to have started it's transition. Or we can say that slew low rise threshold depicts the value close to 0.slew_low_rise_thr is typically 20% from bottom power supply.

<img width="1040" height="595" alt="Screenshot 2025-07-14 at 4 41 32 PM" src="https://github.com/user-attachments/assets/c6989e13-bb81-4577-9978-ecdd4d7c9630" />

slew_high_rise_thr It is typically 20% from top power supply

<img width="1030" height="555" alt="Screenshot 2025-07-14 at 4 41 40 PM" src="https://github.com/user-attachments/assets/bb772335-f2f5-4dc0-8601-59e79c24c0d9" />

slew_low_fall_thr

<img width="1050" height="558" alt="Screenshot 2025-07-14 at 4 41 51 PM" src="https://github.com/user-attachments/assets/1cfb75c3-826b-4008-9c51-a397c673bb24" />

slew_high_fall_thr

<img width="1042" height="561" alt="Screenshot 2025-07-14 at 4 42 03 PM" src="https://github.com/user-attachments/assets/1df0903e-0702-43fa-9b70-27d27bcde63f" />


General Timing Characterization Parameters

1. Timing Threshold Definitions

* Here we will understand various **syntex** and **symentix** of `timing.lib`, `power.lib`, and `noise.lib`.
* This is necessary to understand the **GUNA** software.
* We will try to understand the **timing threshold definitions** of the waveform itself.

2. Other Definitions

* Now the other definitions include **input waveforms**, taking the **input stimulus** and the **output of the first buffer**.
* **`in_rise_thr`**: It tells the **delay from the given input**, to measure the **arrival time** of a **rising signal** at the **input pin** of a standard cell. It is taken when the input crosses **50%** of the signal.

<img width="1044" height="528" alt="Screenshot 2025-07-14 at 4 42 16 PM" src="https://github.com/user-attachments/assets/a0b63c20-4f3a-40a0-9159-3a918e98b607" />

out_rise_thr Just like input rise, output rise threshold is also 50% of the output waveform.

<img width="1040" height="542" alt="Screenshot 2025-07-14 at 4 42 26 PM" src="https://github.com/user-attachments/assets/891d8a20-e842-4252-9a41-7a92d8d5ab42" />

in_fall_thr

<img width="1046" height="528" alt="Screenshot 2025-07-14 at 4 42 35 PM" src="https://github.com/user-attachments/assets/32d329b5-08ee-4348-af8a-cb120142a9bd" />

out_fall_thr

<img width="1058" height="554" alt="Screenshot 2025-07-14 at 4 42 53 PM" src="https://github.com/user-attachments/assets/ac65e7e0-de34-4f09-8a33-e3b0a0aae734" />


---
## 2. Propagation delay and transition time
---
   
We have in&out_rise_thr and in&out_fall_thr. So if we want to calculate delay--> time(out_thr)-time(in_thr)

<img width="1048" height="605" alt="Screenshot 2025-07-14 at 4 53 22 PM" src="https://github.com/user-attachments/assets/4adf2241-3172-470d-85b3-7c1804b35d97" />

Lte's take an example, here the Red curve is input waveform and blue curve is output waveform taken from 2nd inverter.

<img width="1047" height="591" alt="Screenshot 2025-07-14 at 4 53 34 PM" src="https://github.com/user-attachments/assets/234b9e1f-ec58-4e72-b79e-c2f30c1058b5" />

Threshold Point Selection

* If we shift the threshold points **above 50%**, then we will see that there is a **negative delay** as shown below.
* A **negative delay** means **output arrived before the input**, so it is **not good**.
* Therefore, **choosing a proper threshold point is very very important**.

<img width="1044" height="593" alt="Screenshot 2025-07-14 at 4 53 44 PM" src="https://github.com/user-attachments/assets/8fa53dcd-14af-41e9-844a-f1ccc9b29175" />

Another example of negative delay is given below, here the input slew is too high due to long wires.

<img width="1038" height="597" alt="Screenshot 2025-07-14 at 4 53 56 PM" src="https://github.com/user-attachments/assets/ac8b1af4-330f-44b3-9002-389a7e1f5b02" />

We can see that in_rise_thr point is much higher than out_fall_thr point which results ina negative delay.

<img width="1044" height="566" alt="Screenshot 2025-07-14 at 4 54 06 PM" src="https://github.com/user-attachments/assets/137b19d3-5642-4a36-ac67-8e561ec14dfa" />

Transition Time and Slew Rate**

* Next we will understand the **transition time** which is given by:
  → `time(slew_high_rise_thr) - time(slew_low_rise_thr)`
  → Similarly for fall: `time(slew_high_fall_thr) - time(slew_low_fall_thr)`

* Let's consider **20% of VDD** as **low value** and **80% of VDD** as **high value**.

* So here comes the **slew rate**, i.e., **high-low** for **input and output characteristics**.

<img width="1061" height="566" alt="Screenshot 2025-07-14 at 4 54 17 PM" src="https://github.com/user-attachments/assets/dd6e94af-0ad9-44df-be70-b97f2556eb35" />


# **Sky130 Day 3 – Design Library Cell using Magic Layout and ngspice Characterization**

## **Labs for CMOS Inverter ngspice Simulations**

---
## 1. IO Placer Revision
---
* As we have taken the example of an **inverter**, we will be **designing the cell**.

* We'll **load the Magic file** into the **picorv32a**.

* Till now, we have already done the **floorplan**, now we can also change the **core utilization factor**.

* Open the **floorplan** that we got.

* Earlier, we had set `FP_IO_MODE 1`, so we got **equidistant input-output pins**.

* Now let's **change the configuration** and see what happens.

* Inside `floorplan.tcl` we have:
  `env(FP_IO_MODE) 1`

* Now write in OpenLane as:
  `set ::env(FP_IO_MODE) 2`

* Then run the floorplan again using:
  `run_floorplan`


![WhatsApp Image 2025-07-20 at 16 30 35](https://github.com/user-attachments/assets/3547fc65-4ba2-4a71-9581-53b7347ee27a)


After running the command, check the updated floorplan. Now, the pins are stacked one above the other, reflecting the new I/O placement mode.


![WhatsApp Image 2025-07-20 at 16 30 35 (1)](https://github.com/user-attachments/assets/2ed02fc8-2819-445a-a728-67fa0e3c8eb4)




![WhatsApp Image 2025-07-20 at 16 30 35 (2)](https://github.com/user-attachments/assets/c298c985-1ac4-40e3-90cd-3ad8e8eab0f0)


---
## 2. SPICE Deck Creation for CMOS Inverter
---

* Now we will be doing some **SPICE simulations** and deriving the **real-time MOSFETs**.

* **1st step** is **SPICE deck** creation. It is the **connectivity information** about the **netlist**.

* It has the **inputs provided for simulation**, **tap points** at which we'll take the outputs, and so on.

* We will create the **SPICE deck** for the **complete netlist** with **PMOS** and **NMOS**.

* In this case, we are looking at the **static behaviour** of the CMOS.

* Next, we will define the **component values**, where:

  * PMOS and NMOS are given **W/L values**
  * **Output capacitance load** value is defined

* (Although we know **PMOS should be wider** than NMOS, here we will take the **same values** for both.)


<img width="772" height="818" alt="Screenshot 2025-07-15 at 5 56 12 PM" src="https://github.com/user-attachments/assets/8e111b99-a898-4143-a439-0bb7fe4b2159" />

Component Values: The W/L ratios (width/length) of the PMOS and NMOS transistors are specified. For now, the same size is used for both.

<img width="611" height="517" alt="Screenshot 2025-07-15 at 5 58 25 PM" src="https://github.com/user-attachments/assets/3a1a2d50-eeeb-418d-ade6-1f6c0e9e9752" />

Identify Nodes: Nodes are the points between which components are connected. They are essential for defining the SPICE netlist.

<img width="615" height="473" alt="Screenshot 2025-07-15 at 5 58 35 PM" src="https://github.com/user-attachments/assets/f316ae38-701c-4c0c-a2eb-08f8412efc4e" />

Naming Nodes: Common node names include: in, Vss, Vdd, and out.

<img width="617" height="562" alt="Screenshot 2025-07-15 at 5 58 45 PM" src="https://github.com/user-attachments/assets/91c61a5d-675d-4a7d-8be6-df28ebd4f40c" />

Starting the SPICE Deck
MOSFET Syntax in SPICE:

<img width="641" height="552" alt="Screenshot 2025-07-15 at 5 58 59 PM" src="https://github.com/user-attachments/assets/669cd524-8114-489b-bc8b-e243e90ce6de" />

<img width="644" height="567" alt="Screenshot 2025-07-15 at 5 59 10 PM" src="https://github.com/user-attachments/assets/d4162004-624c-4fde-82f8-4189afc38361" />

M<name> <drain> <gate> <source> <bulk> <model_name> L=<length> W=<width>

<img width="628" height="153" alt="Screenshot 2025-07-15 at 6 02 13 PM" src="https://github.com/user-attachments/assets/1066dd09-e9e1-4b26-b457-b4fa2de98f8a" />

---
## 3.SPICE simulation lab for CMOS inverter
---

Till now, we have described the connectivity information of the CMOS inverter. Next, we will define the connectivity of the other components, such as the load capacitor and voltage sources.


<img width="587" height="331" alt="Screenshot 2025-07-15 at 6 04 52 PM" src="https://github.com/user-attachments/assets/9e9b6e65-eb28-40dd-b5d7-b9d20e1d9075" />

Load Capacitor (Output Capacitance)

<img width="642" height="558" alt="Screenshot 2025-07-15 at 6 05 17 PM" src="https://github.com/user-attachments/assets/997ada36-9679-4fdc-8f82-30a3cbd6b43d" />


The load capacitor is connected between the out node and node 0 (ground).

The value of the capacitor is 10fF.

Cload out 0 10f

Supply Voltage (Vdd)

<img width="637" height="565" alt="Screenshot 2025-07-15 at 6 05 28 PM" src="https://github.com/user-attachments/assets/d36f4574-2a64-4d4f-ba4e-1af85ff9299f" />

The Vdd source is connected between Vdd and node 0.

The voltage value is 2.5V.

Vdd Vdd 0 2.5


<img width="644" height="545" alt="Screenshot 2025-07-15 at 6 05 37 PM" src="https://github.com/user-attachments/assets/84b25b2c-600b-4665-8682-37cfcc7dc46d" />

The input voltage source is connected between Vin and node 0.
The voltage is also 2.5V.
Vin in 0 2.5


These definitions complete the required component connections for simulating the CMOS inverter with proper input, power, and output load conditions.

Now we need to add the simulation commands to perform a DC sweep. In this case, we're sweeping the input voltage Vin from 0V to 2.5V with a step size of 0.05V. This allows us to observe how Vout changes as Vin varies.

DC Sweep Command

<img width="635" height="156" alt="Screenshot 2025-07-15 at 6 06 02 PM" src="https://github.com/user-attachments/assets/bdb9a6b1-971f-4b07-b12c-94c9d46e0aae" />


.dc Vin 0 2.5 0.05

Vin → name of the voltage source to sweep

0 → starting value

2.5 → ending value

0.05 → step size

This command will simulate the behavior of the CMOS inverter across all input voltages from 0V to 2.5V and generate the corresponding output values, helping us analyze the VTC (Voltage Transfer Characteristic).

Final step is to include the model files, which contain the complete description of NMOS and PMOS transistors.


<img width="637" height="142" alt="Screenshot 2025-07-15 at 6 06 23 PM" src="https://github.com/user-attachments/assets/efbc5cb4-b2c3-4868-b785-8e671958f9fc" />

<img width="641" height="290" alt="Screenshot 2025-07-15 at 6 06 39 PM" src="https://github.com/user-attachments/assets/20a78ab1-f9b8-4a24-82ef-abc7e1bba523" />

Lets do the spice simulation for the following specifications


<img width="640" height="82" alt="Screenshot 2025-07-15 at 6 06 48 PM" src="https://github.com/user-attachments/assets/387a7745-8e93-48fa-82c4-dfc17e1b6a8a" />

The plot obtained is

<img width="637" height="463" alt="Screenshot 2025-07-15 at 6 07 05 PM" src="https://github.com/user-attachments/assets/08f738eb-2592-4366-85bb-5a2f5c752967" />

Now, we perform another simulation where the PMOS width is set to three times the NMOS width. After running the simulation with this updated sizing, we obtain the output graph as shown below.


<img width="641" height="76" alt="Screenshot 2025-07-15 at 6 07 23 PM" src="https://github.com/user-attachments/assets/0a42fd47-a26a-4913-a8cb-7f15e706401e" />

The plot obtained is

<img width="640" height="487" alt="Screenshot 2025-07-15 at 6 07 36 PM" src="https://github.com/user-attachments/assets/d884182a-cfb0-48d5-a84c-3283bf2577dd" />

The difference between the two graphs is that, in the second graph, the transfer characteristic lies exactly in the middle of the graph. In contrast, in the first graph, the transfer characteristic is shifted to the left of the center.

---
## 4. Switching Threshold Vm
---

* Previously, in the first case we took **Wn/Ln = Wp/Lp = 1.5**,
  whereas in the second case we took **Wp/Lp > Wn/Ln**.

* Clearly, we saw **waveform shift** in the second case.

* Both have **different applications**.

* Even though we changed the **width/length ratio**,
  we saw the **graph is same** in both cases.

* This shows that the **CMOS inverter is a robust device**.

* The **behaviour of the inverter remains the same** despite the changes.

* We will do the **Static Behaviour Evaluation** showing the **robustness** of the CMOS inverter.

* The parameters which define the same are:

<img width="613" height="278" alt="Screenshot 2025-07-16 at 7 11 47 PM" src="https://github.com/user-attachments/assets/b4775a4a-c6cb-4f31-ad91-3d548cda7310" />

In this figure, we observe that at Vm ≈ 0.9V, the condition Vin = Vout is met. This point is critical for CMOS operation because at Vm, there is a high possibility that both the PMOS and NMOS transistors are partially turned on.

<img width="594" height="415" alt="Screenshot 2025-07-16 at 7 12 54 PM" src="https://github.com/user-attachments/assets/39132c89-d0c6-42f8-920e-4c55ce42cb6b" />


* When **both transistors conduct simultaneously**, it creates a **direct path from Vdd to Vss** (power to ground), resulting in **leakage current**.

* This current can lead to **increased power consumption** and may affect **circuit reliability** if not properly controlled.

* By comparing the **two graphs**, we gain a clear understanding of the **switching threshold voltage (Vm)** and how it plays a key role in defining the **operating region** and **power behavior** of a **CMOS inverter**.


<img width="598" height="277" alt="Screenshot 2025-07-16 at 7 14 09 PM" src="https://github.com/user-attachments/assets/077a6670-c349-4c00-a102-b3d10b12953a" />


* In the graph below, we can identify the **operating regions** of the **PMOS** and **NMOS** transistors at different points along the **transfer characteristic curve**.

* The **direction of current flow** is different for each:

  * For **NMOS**, current flows from **drain to source** (typically from **Vout to GND**).
  * For **PMOS**, current flows from **source to drain** (typically from **Vdd to Vout**).

* By analyzing the graph, we can determine at each region (**cutoff**, **linear**, **saturation**) whether the NMOS or PMOS is **conducting**, and in **what mode**.

* This is helpful for understanding the **dynamic behavior** and **power dissipation** of the **CMOS inverter** across its **input voltage range**.

<img width="595" height="413" alt="Screenshot 2025-07-16 at 7 16 05 PM" src="https://github.com/user-attachments/assets/57eb266f-7858-4ef4-ac43-079412af5b7f" />

---
## 5.Static and dynamic simulation of CMOS inverter
---

Now we will try to prove the robustness of CMOS Inverter with different W/L ratios in SPICE simulator and calculating the Vm.


<img width="595" height="341" alt="Screenshot 2025-07-16 at 7 17 30 PM" src="https://github.com/user-attachments/assets/06a1c140-7d6a-49f8-b385-cb4c5886a0f2" />

We now move forward by studying the effect of changing the PMOS width-to-length ratio (W/L) as an integer multiple of the NMOS (W/L). The goal is to evaluate the robustness of the switching threshold (Vm) under different sizing conditions.

Earlier, we had already simulated the case where:

(W/L of PMOS) / (W/L of NMOS) = 1

<img width="594" height="489" alt="Screenshot 2025-07-16 at 7 18 26 PM" src="https://github.com/user-attachments/assets/84a32f0f-29a0-4c21-9ebd-2336e5768214" />

Dynamic Simulation
In this step, we shift from DC analysis to dynamic (transient) simulation.


<img width="610" height="389" alt="Screenshot 2025-07-16 at 7 20 01 PM" src="https://github.com/user-attachments/assets/a9068549-ecb5-4ef7-906e-56be22fd0a07" />


An input pulse is defined in the SPICE deck.
This pulse waveform is applied to the CMOS inverter as the input signal, and we run a transient analysis using the .tran command.


<img width="589" height="470" alt="Screenshot 2025-07-16 at 7 20 46 PM" src="https://github.com/user-attachments/assets/96acfdb3-0a8f-44ce-8e5f-c592f340a618" />


Purpose of This Simulation
Through this dynamic simulation, we can observe:

Rise delay (low → high transition at output)
Fall delay (high → low transition at output)
How these delays change with variations in Vm (caused by different PMOS/NMOS sizing)
In this setup, everything else remains constant—only the input waveform and the simulation type are changed. This helps us study how Vm impacts switching speed and symmetry, which are critical for timing analysis and circuit performance.

To calculate the delay of a CMOS inverter, we need to plot both the input and output waveforms against time.

<img width="594" height="461" alt="Screenshot 2025-07-16 at 7 21 31 PM" src="https://github.com/user-attachments/assets/5e8da80b-55af-45c8-8363-7643cca7e53e" />


Delay Calculation Method
Delay is measured from the point where the input crosses 50% of VDD to the point where the output crosses 50% of VDD.
In this case, since VDD = 2.5V, the 50% threshold is 1.25V.
Step-by-Step Process
Zoom in on the waveform around the switching points.

Note the timestamps where:

Vin = 1.25V
Vout = 1.25V
Rise Delay
Input is falling, output is rising.
Delay = Time (Vout reaches 1.25V) − Time (Vin falls to 1.25V)
Delay = 1.16276 ns − 1.01446 ns = 0.1483 ns

<img width="588" height="454" alt="Screenshot 2025-07-16 at 7 22 19 PM" src="https://github.com/user-attachments/assets/0e505f1e-b771-44e4-92d5-a5597c88a817" />


<img width="595" height="471" alt="Screenshot 2025-07-16 at 7 22 28 PM" src="https://github.com/user-attachments/assets/79d51c0c-64a9-4ca3-afc5-ff675165d9eb" />


Fall Delay
Input is rising, output is falling.
Delay = Time (Vout falls to 1.25V) − Time (Vin rises to 1.25V)
Delay = 2.07653 ns − 2.00486 ns = 0.07167 ns


<img width="601" height="443" alt="Screenshot 2025-07-16 at 7 23 20 PM" src="https://github.com/user-attachments/assets/0a146cd3-fc54-42df-b29f-3d2ef624ebe5" />


<img width="591" height="454" alt="Screenshot 2025-07-16 at 7 23 31 PM" src="https://github.com/user-attachments/assets/3b596963-1129-407d-a285-631b4a5c1ea5" />


This analysis gives a clear view of how the inverter responds to input changes and helps in evaluating its timing performance.

---
## 6. Lab steps to git clone vsdstdcelldesign
---

What is git clone?

The git clone command is used to download a GitHub repository (or any Git repo) to your local machine.

Syntax of git clone

git clone <repository_url>
Example Usage

Let’s say you want to clone the OpenLane repository from GitHub.

Copy the URL of the repository (HTTPS link):

https://github.com/The-OpenROAD-Project/OpenLane.git
Open terminal or command prompt, and run:

git clone https://github.com/The-OpenROAD-Project/OpenLane.git
This command will:

Create a folder named OpenLane
Download all the code, branches, and history from the repo into that folder
Optional: Clone into a Custom Folder Name

git clone https://github.com/The-OpenROAD-Project/OpenLane.git my_openlane
This clones the repo into a folder named my_openlane.

After Cloning

You can go into the cloned repo and start working:

cd OpenLane
To get the clone, copy the clone address from reporetery and paste in openlane terminal after the command git clone. this will create the folder called "vsdstdcelldesign" in openlane directory.


<img width="832" height="424" alt="Screenshot 2025-07-25 at 5 44 14 PM" src="https://github.com/user-attachments/assets/5c4c984a-d30c-4dbe-b7ac-81e365dfd777" />


copy the sky130.tech file in vsdstdcelldesign directory


<img width="834" height="441" alt="Screenshot 2025-07-25 at 5 44 38 PM" src="https://github.com/user-attachments/assets/9d68b2ad-f25d-4bf9-99c0-8cd9e73cd8b4" />


Now to view the invereter layout

<img width="832" height="166" alt="Screenshot 2025-07-25 at 5 44 52 PM" src="https://github.com/user-attachments/assets/d93ab7cf-e7dd-4e7c-8d60-812a6b291fc2" />


<img width="830" height="430" alt="Screenshot 2025-07-25 at 5 45 02 PM" src="https://github.com/user-attachments/assets/faf76b6d-055b-4110-a01d-74e4ec1a1517" />



## Inception of layout ̂CMOS fabrication process

## 1. Create Active regions

1. We will create a 16 mask CMOS process.
Selecting a substrate- THe complete layout is laid onto a substrate,here we will select the most commonly used substrate i.e.a ptype Si substrate.


<img width="1333" height="398" alt="Screenshot 2025-07-20 at 6 59 45 PM" src="https://github.com/user-attachments/assets/0b6873e9-c0f7-45e4-a329-b22635cba0b2" />



2. Create the active regions for transistors- Active regions are the pockets where we will dope with n type.
For this we need to create the isolation so that the pockets do not interact with each other, so we will grow a ~40nm SiO2 layer on the substrate.
Next we will deposite a ~80nm layer of Si3N4 on top of SiO2.
Now to make the active region pockets we will deposit the ~1micron layer of photoresist to create the masks.
Where we want to create the wells there will put masks.
And UV light drom the top.

<img width="930" height="408" alt="Screenshot 2025-07-20 at 7 01 03 PM" src="https://github.com/user-attachments/assets/097df045-ce10-448b-9fa5-6819d943499b" />

<img width="920" height="444" alt="Screenshot 2025-07-20 at 7 01 17 PM" src="https://github.com/user-attachments/assets/7dee705c-2532-48d0-9259-7dc7c8791184" />

<img width="923" height="429" alt="Screenshot 2025-07-20 at 7 01 28 PM" src="https://github.com/user-attachments/assets/ff0db60c-7cdf-4ec2-b120-814b188d7e32" />

<img width="928" height="424" alt="Screenshot 2025-07-20 at 7 01 39 PM" src="https://github.com/user-attachments/assets/000c4fc2-c715-4252-a942-e8c892790d15" />


After this the extra regions that were being exposed to the UV light are washed away.

<img width="911" height="512" alt="Screenshot 2025-07-20 at 7 02 35 PM" src="https://github.com/user-attachments/assets/ac4373fe-efd2-46a7-9eff-c390c458d29e" />


Next step is to remove the mask and etch out the exposed area. The area which has photoresit will be saved from the etchant.

<img width="920" height="429" alt="Screenshot 2025-07-20 at 7 02 56 PM" src="https://github.com/user-attachments/assets/195b4516-753e-4f1d-b385-69604d7eeb1c" />


After this the resist is also removed and we place the substrate into high temperature furnace to grow the SiO2 layer on the exposed area.

<img width="901" height="358" alt="Screenshot 2025-07-20 at 7 03 28 PM" src="https://github.com/user-attachments/assets/045c5d74-050c-4820-b3be-f56b4e4a3529" />


Si3N4 was able to protect the areas underneath it, but couldn't protect the edges.

<img width="938" height="373" alt="Screenshot 2025-07-20 at 7 03 50 PM" src="https://github.com/user-attachments/assets/d0a72d2d-2e20-418a-b06a-7ea80ede3faa" />


Now the transistors which will be fabricated are now isolated, this process is called 'LOCOS' Which is 'local oxidation of silicon', and the area which protects transistor from communicating is called 'Bird's Beak'.

<img width="958" height="402" alt="Screenshot 2025-07-20 at 7 04 23 PM" src="https://github.com/user-attachments/assets/ddff9558-8673-45ef-a72d-2b0e7e214842" />


Also the Si3N4 will be stripped out using hot phosphoric acid, resulting in an isolation layer.


<img width="934" height="361" alt="Screenshot 2025-07-20 at 7 04 50 PM" src="https://github.com/user-attachments/assets/ec386e26-5b7c-4260-ba6d-16e84aa23697" />



## 2. Formation of N-well and P-well


3)N-well and P-well Formation
We cannot form both P-well and N-well simultaneously, as the doping requirements are different. Therefore, we must protect one region while forming the other using a photoresist layer.

To form the P-well, we proceed as follows:

First, deposit a photoresist layer on the wafer.
Then, using Mask 2 and UV light exposure, we pattern the photoresist to define the areas where the P-well is to be created.
The exposed regions are developed, opening windows for P-type dopant implantation while protecting the rest of the wafer.
This selective process ensures controlled well formation in the desired regions. The same method is repeated later for N-well formation, using a different mask and dopant type.

<img width="784" height="486" alt="Screenshot 2025-07-20 at 7 11 08 PM" src="https://github.com/user-attachments/assets/232a4b55-2b5b-4c7c-8767-a7ea01ead653" />




Now, the area where we want to form the P-well is exposed after the photoresist is patterned. The mask is then removed, and the wafer is subjected to ion implantation using Boron as the dopant.

The implantation energy is typically around 200 keV.
This step introduces P-type dopants into the exposed silicon region, but at this stage, it is still referred to as a P-type implant.
To activate the dopants and drive them deeper into the substrate, a high-temperature annealing process is performed. After annealing, the doped region becomes a fully formed P-well.


<img width="808" height="404" alt="Screenshot 2025-07-20 at 7 11 31 PM" src="https://github.com/user-attachments/assets/eb991b80-d2ae-4b5c-8994-7c7fb39834c2" />


A similar process is followed to form the N-well:

* Apply a new photoresist layer and use Mask 3 to define the N-well regions.
* Expose the wafer to UV light, pattern the photoresist, and develop it to expose only the areas where the N-well is to be formed.
* Remove the photoresist from exposed regions.
* Perform ion implantation using Phosphorus ions (N-type dopant), typically at an energy of around 200 keV.
* Finally, carry out high-temperature annealing to activate the dopants and drive them into the silicon.
This completes the N-well formation.

<img width="802" height="379" alt="Screenshot 2025-07-20 at 7 12 43 PM" src="https://github.com/user-attachments/assets/8e60b5b4-2f60-4ab7-b88a-a480f35561e7" />



Till now, the depth of the wells (P-well and N-well) has not been fully established. To achieve the desired depth and proper dopant distribution, the wafer is placed into a high-temperature furnace.

This step is known as drive-in diffusion.

During this process, the implanted dopants diffuse deeper into the silicon substrate.
The depth and concentration of the wells are determined by the temperature and duration of this step.
This completes the proper formation of P-well and N-well regions with defined depths, making them ready for active device fabrication.

<img width="819" height="454" alt="Screenshot 2025-07-20 at 7 13 06 PM" src="https://github.com/user-attachments/assets/442b2efe-5878-4c1e-8614-777408f1e3e4" />

<img width="815" height="474" alt="Screenshot 2025-07-20 at 7 13 18 PM" src="https://github.com/user-attachments/assets/84503973-1377-4db6-903b-368eb9728787" />

<img width="808" height="376" alt="Screenshot 2025-07-20 at 7 13 28 PM" src="https://github.com/user-attachments/assets/39420609-ca84-4982-8e0e-b9670b9c56bf" />



## 3. Formation of gate terminal


4) Gate Formation
   
The gate terminal is the most critical terminal of both PMOS and NMOS transistors, as it directly controls the threshold voltage (Vth) of the device.

The threshold voltage is influenced by two main factors:

* Doping concentration in the channel region
* Oxide capacitance (which depends on oxide thickness and permittivity)

Step 1: Channel Doping Adjustment
To adjust the doping concentration in the channel and help control the threshold voltage, we perform channel doping as follows:

Apply Mask 4 to define the gate region.
Carry out ion implantation using Boron ions (a P-type dopant).
The implantation is done at a lower energy, typically around 60 keV, so the dopants stay close to the surface.
This step ensures the channel is properly doped before the actual gate structure is built, allowing for precise threshold voltage tuning.

<img width="823" height="400" alt="Screenshot 2025-07-25 at 5 48 09 PM" src="https://github.com/user-attachments/assets/4b25f507-e7b6-4b81-bd3e-396954ced477" />

<img width="819" height="449" alt="Screenshot 2025-07-25 at 5 48 17 PM" src="https://github.com/user-attachments/assets/3fbf4717-302f-42c7-9c70-812f112a2817" />

The same process is repeated for the N-well region as well:

* This time, we use Mask 5 to define the required area.
* Perform ion implantation using Arsenic ions (an N-type dopant).
* The implantation is done at low energy, similar to the previous step, to control the surface doping concentration.
This step ensures proper channel doping for PMOS transistors formed in the N-well, contributing to accurate threshold voltage control.

<img width="817" height="414" alt="Screenshot 2025-07-25 at 5 48 30 PM" src="https://github.com/user-attachments/assets/fca870c9-36b7-4316-8628-74ca27699177" />

<img width="821" height="482" alt="Screenshot 2025-07-25 at 5 48 42 PM" src="https://github.com/user-attachments/assets/6ce14870-88af-4b79-95ab-fd6c547632ed" />


We will now fabricate a thick layer ~0.4 micron of polysilicon,and expose to very light Ntype (arsenic or phosphorus) layer by ion implantation for low gate resistance.

<img width="808" height="352" alt="Screenshot 2025-07-25 at 5 54 24 PM" src="https://github.com/user-attachments/assets/b98e2854-c586-486d-8c06-05b1e3281075" />

Then we will deposit Mask6 on top

<img width="815" height="445" alt="Screenshot 2025-07-25 at 5 54 45 PM" src="https://github.com/user-attachments/assets/969a28dd-aafd-40d5-a4ac-ea2f76649aca" />


Expose to the UV light, which washes away the exposed area, and the remaining area that was out from the photoresist is etched away.In this way we will get the polysilicon gate.


<img width="814" height="446" alt="Screenshot 2025-07-25 at 5 54 57 PM" src="https://github.com/user-attachments/assets/3ad85ed6-1163-428c-a15c-99792616e7fa" />


The remaining photoresist is removed. We will get the substrate, the oxide layer and controlled gate kayer of polysilicon.


<img width="823" height="382" alt="Screenshot 2025-07-25 at 5 55 07 PM" src="https://github.com/user-attachments/assets/fedfcf9f-b254-47f2-804d-5607701984d8" />

---
## 4.Lightly doped drain (LDD) formation
---

5)LDD formation

For PMOS we are tyring to build the P+,P-,N doping profile, where source is P+ doped, drain is lightly doped and substrate is N type. SImilarly for NMOS the doping profiles are N+,N-,P.
This profile is maintained due to two reasons:
Hot Electron effect- when device size reduces-->Electric field increases(E=V/d)-->high energy carriers break Si-Si bonds--> this energy crosses 3.2eV barrier between Si conduction band and SiO2 conduction band.
Short channel effect- due to low devices size-->gate length is changed from 1micron to 0.5 micron-->the drain area penetrates into channel area-->difficult for gate to control current between source and drain.


<img width="831" height="306" alt="Screenshot 2025-07-25 at 7 37 42 PM" src="https://github.com/user-attachments/assets/9f47bb5c-f041-4465-8f1c-5e8975ecffca" />




After creating the Mask7 and creating impurity of Ntype over pwell, and due to ion implantation and by controlling the doping concentration we get the N- implants.

<img width="816" height="491" alt="Screenshot 2025-07-25 at 7 38 06 PM" src="https://github.com/user-attachments/assets/28051694-f1e9-4136-84c2-4196910bccea" />



Now we will create Mask8 and protect this layer and expose the other layer with Boron such a way that P- implants are created.


<img width="826" height="500" alt="Screenshot 2025-07-25 at 7 38 31 PM" src="https://github.com/user-attachments/assets/8e351bfc-9eed-4bb5-a8ba-9aaca903547a" />


But the actual structure will be affected by these implants, so to avoid this we will create 'side wall spacers' by depositing a thick SiO2 or Si3N4 layer on the gate terminal.Then doing the Plasma anisotropic Etching to remove the oxide layer. This etching does not remove side walls, there it will create "side wall spacers'.



<img width="833" height="344" alt="Screenshot 2025-07-25 at 7 38 52 PM" src="https://github.com/user-attachments/assets/e6d51580-d278-41f6-bfe1-0fa525d3d67d" />

---
## 5. Source and drain formation
---

6) Source and drain formation
Before the formation of Source and Drain,a thin screen layer of oxide is deposited to avoid channeling effect.


<img width="811" height="376" alt="Screenshot 2025-07-25 at 7 41 18 PM" src="https://github.com/user-attachments/assets/2c546d2f-d201-4941-baa7-5f38454dd580" />


For Source and Drain formation, we deposit make Mask9 on n substrate and exposing the p substrate to Arsenic with energy ~75eV. The side wall spacers will protect the LDD so that channeling does not happen.We will get the N+ structure required.

<img width="817" height="503" alt="Screenshot 2025-07-25 at 7 41 30 PM" src="https://github.com/user-attachments/assets/6a15690b-cdea-4d19-abf2-ee9963bee0d7" />


Similarly we will mask this layer using Mask10 and expose the n substrate to Boron.


<img width="820" height="492" alt="Screenshot 2025-07-25 at 7 43 15 PM" src="https://github.com/user-attachments/assets/1f668db2-c08e-4bec-940a-7dae10a0123c" />



Now we will put the structure under high temperature for Annealing, it will push the dopants more inside the substrate and there will be uniform distribution.


<img width="816" height="414" alt="Screenshot 2025-07-25 at 7 43 41 PM" src="https://github.com/user-attachments/assets/9f375146-9744-42a6-a36a-f7be8c7b714d" />

---
## 6. Local interconnect formation
---

7) Steps to form contacts and interconnects(local)
Contacts are really important, as these are the only users a user can connect to the circuit.For thsi first we will etch out the thin oxide layer for avoiding channeling effect using HF.

<img width="820" height="345" alt="Screenshot 2025-07-25 at 7 47 26 PM" src="https://github.com/user-attachments/assets/5f543867-776f-47f2-87bf-66f8cd27af21" />



For creating local interconnects, first we will deposit Titanium suing sputtering process all over the substrate.


<img width="817" height="368" alt="Screenshot 2025-07-25 at 7 47 53 PM" src="https://github.com/user-attachments/assets/3a396931-7956-49cc-8156-4e4b5c50c338" />


Next step is to create the connects between titanium and source drain. This is done by heating the wafer at an ambient temperature of 600-700 degreese celcius under N2 gas for 60sec. This will result in TiSi2(a low resistive metal contact on gate) contacts created on source and drain. Also TiN layer will be formed, it is used only for local communication.


<img width="823" height="391" alt="Screenshot 2025-07-25 at 7 48 15 PM" src="https://github.com/user-attachments/assets/c73f61ef-d30e-419f-8570-61f3d5933f2a" />


To bring up the required contacts on top, we will put Mask11 and etch out the area we want to be coming out. We want Source, Drain and Gate area to be coming out.

<img width="819" height="530" alt="Screenshot 2025-07-25 at 7 48 41 PM" src="https://github.com/user-attachments/assets/8d2f582f-1fce-4208-886d-3f3b75bdf68b" />


We will etch out the extra TiN layer using RCA cleaning.


<img width="767" height="296" alt="Screenshot 2025-07-25 at 7 49 05 PM" src="https://github.com/user-attachments/assets/d0d4b8ee-7c50-4e5e-9309-12fb77da35f3" />


<img width="822" height="467" alt="Screenshot 2025-07-25 at 7 49 14 PM" src="https://github.com/user-attachments/assets/fe5ab0a0-bd0e-4393-b8fb-9beb1c85d0af" />



---
## 7. Higher Level Metal Formation
---

8) Higher level metal formation
Here we observe there is non planarity which is not good for depositing higher metal interconnects.So we will planarise this surface by using thick layer of SiO2 which is doped with phosphorus and boron. The reason of doping is that phosphorus protects the layer from reactive sodium ions and boron is used to reduce the temperature as this wafer will be exposed to certain high temperature so boron will help in reducing the temperature.

<img width="827" height="429" alt="Screenshot 2025-07-25 at 7 50 53 PM" src="https://github.com/user-attachments/assets/74ea8e7a-d94b-4174-b9d2-eff42e9a055c" />




To remove th hills and bumps we do polishing, CMP.

<img width="816" height="391" alt="Screenshot 2025-07-25 at 7 51 23 PM" src="https://github.com/user-attachments/assets/5845e432-d338-4ec6-8e8a-30834dce0c5d" />



Next is creating the metal contacts by drilling, so this also done by photlithography technique. By using Mask12.

<img width="819" height="434" alt="Screenshot 2025-07-25 at 7 51 41 PM" src="https://github.com/user-attachments/assets/f9001548-ae79-44d3-9fbe-2ee32d219557" />



Now we will remove the mask by washing away the photoresist. We will create thin layer ~10nm of TiN, it acts as Adhesion layer between SiO2 and acts a barrier layer for bottom and top interconnects.

<img width="810" height="455" alt="Screenshot 2025-07-25 at 7 51 59 PM" src="https://github.com/user-attachments/assets/d0461224-92c1-4628-91fe-3afc2d361284" />


Then we will deposit a blanket of Tungsten(W) layer, this will help to create a very good contact from bottom to top.

<img width="819" height="403" alt="Screenshot 2025-07-25 at 7 52 15 PM" src="https://github.com/user-attachments/assets/e463aa01-d570-466e-9f9a-6b2c6f53dcd1" />


NExt, is CMP, removing the extra tungsten from top

<img width="825" height="409" alt="Screenshot 2025-07-25 at 7 52 33 PM" src="https://github.com/user-attachments/assets/c4d7ba22-e73a-4178-b9b3-e4afe4ae0842" />

Now we will deposit metal Aluminium layer on top to take the metal contacts above. Further we will mask to expose the specific areas.

<img width="821" height="441" alt="Screenshot 2025-07-25 at 7 53 07 PM" src="https://github.com/user-attachments/assets/fcf23639-f8f6-4dd4-8db9-23749c8fd7d3" />

<img width="828" height="414" alt="Screenshot 2025-07-25 at 7 53 15 PM" src="https://github.com/user-attachments/assets/7a1e75e2-648e-4242-8f4f-9fcd929a2a0c" />

We got the first layer of metal interconnect below.


<img width="815" height="422" alt="Screenshot 2025-07-25 at 7 54 56 PM" src="https://github.com/user-attachments/assets/fcc8680a-15e5-4e13-a8b0-c646d5b2825f" />

We will repeat the above processes to get further layer of metal interconnects.

<img width="835" height="416" alt="Screenshot 2025-07-26 at 12 12 50 PM" src="https://github.com/user-attachments/assets/6dbaec83-34ba-40e6-8e97-32a7464f3370" />



<img width="825" height="423" alt="Screenshot 2025-07-26 at 12 13 02 PM" src="https://github.com/user-attachments/assets/e16fd9c2-bc5d-4dac-9192-2b286ad57863" />


After Mask14 again a thin layer or TiN is deposited.


<img width="818" height="433" alt="Screenshot 2025-07-26 at 12 13 31 PM" src="https://github.com/user-attachments/assets/a6c87047-2833-4ae2-88f6-7ad406f334dc" />


Now agaian depositing Tungsten(W) on top.


<img width="824" height="422" alt="Screenshot 2025-07-26 at 12 13 52 PM" src="https://github.com/user-attachments/assets/2763880a-074f-4931-8ef1-db2bdbf1e0cc" />



Now we will deposit the third layer of interconnect using Mask15. Also the thickness is more than the bottom layer. When we go from bottom to top the thickness of metal layers increase.



<img width="843" height="439" alt="Screenshot 2025-07-26 at 12 14 22 PM" src="https://github.com/user-attachments/assets/87a4414a-1b5e-4ae5-a199-050eb82e58da" />


After this we will deposit the Si3N4 layer, we use Si3N4 to protect the chip as this is a good protectant layer from the outside world.

<img width="835" height="428" alt="Screenshot 2025-07-26 at 12 14 48 PM" src="https://github.com/user-attachments/assets/0936d777-01c8-42e1-8a15-1e98c7c8446a" />



Finally, we will use Mask16 to drill out the final contacts outside.

<img width="818" height="565" alt="Screenshot 2025-07-26 at 12 15 20 PM" src="https://github.com/user-attachments/assets/034642ad-d70c-498a-bdcb-56902b325da9" />

---
8.Lab introduction to Sky130 basic layers layout and LEF using inverter
---

The layers which we see here are required for basic CMOS inverter.The above one is PMOS and below is NMOS, Red line is Polysilicon.
On the riight side we see color palatte which shows the layers.
In skywater130A the first layer is local interconnect layer which is shown by light blue colour, the purple colour is Metal 1, pink color is Metal 2, n well is shown by solid slanting dashed lines


<img width="829" height="392" alt="Screenshot 2025-07-26 at 1 23 36 PM" src="https://github.com/user-attachments/assets/8cdb2d36-2cd8-45f5-86e8-73c24dcea591" />

We know when a poly crosses n-diffusion it's an NMOS and similarly when a ploy crosses p-diffusion it's a PMOS.
We can check this if it holds true or not by selecting that part and type 'what' on tkcon.


![WhatsApp Image 2025-07-26 at 13 20 33 (1)](https://github.com/user-attachments/assets/e5bc6655-0b45-406b-9fce-efb6174199c3)

Similarly we can do for PMOS as well.

Now to check if the PMOS drain is connected to NMOS drain, in magic press s three times after placing the cursor over drain.

![WhatsApp Image 2025-07-26 at 13 20 33](https://github.com/user-attachments/assets/c2997a59-c1e4-4114-b72b-afbfd3e9d242)


Also in CMOS the source of PMOS is connected to VDD and source of NMOS is connected to GND.

![WhatsApp Image 2025-07-26 at 13 20 34](https://github.com/user-attachments/assets/aa0fc346-2eef-4f73-a687-52f3f276b5fd)

<img width="579" height="525" alt="Screenshot 2025-07-26 at 1 25 43 PM" src="https://github.com/user-attachments/assets/15254865-4530-4542-aa42-44f1bdb5b572" />

---
## 9.Lab steps to create std cell layout and extract spice netlist
---

The CMOS inverter we see is being taken from the repository https://github.com/nickson-jose/vsdstdcelldesign Also, we need to ensure the final design needs to be DRC(design rule check) clean.
How to know the logical involved in formation of inverter? FOr that we will extract SPICE and do SPICE simulations in ngspice.
For this we will go to tkcon, and see where we are by pwd, then we will type extract all.


![WhatsApp Image 2025-07-26 at 13 27 51 (1)](https://github.com/user-attachments/assets/06842a0c-dcf7-4197-b765-fcd633ebb00a)

We will see if that has been extracted. So sky130A_inv_ext is present

![WhatsApp Image 2025-07-26 at 13 27 51 (2)](https://github.com/user-attachments/assets/f23b9a02-5360-41b5-a32d-e105f277e2f1)

Next we will use this .ext file to create our SPICE file which will be used in ngspice tool.In tkcon we will write the command ext2spice cthresh 0 rthresh 0, this will extract the parasitic capacitances and resistances, then we will write ext2spice and enter.

![WhatsApp Image 2025-07-26 at 13 27 51 (3)](https://github.com/user-attachments/assets/f4083785-49d5-4041-bd74-c86eab8f5be9)


After we will see that spice file has been created.


![WhatsApp Image 2025-07-26 at 13 27 51 (4)](https://github.com/user-attachments/assets/898fa5d5-ede0-4f4b-92ca-e5e3fe5e59ba)


We will now check what's there inside the spice file.

![WhatsApp Image 2025-07-26 at 13 27 51](https://github.com/user-attachments/assets/da7cb8ed-0543-4338-85a8-9e2f52d078c2)

---
## Sky130 Tech File Labs
---
## 1.Lab steps to create final SPICE deck using Sky130 tech
---

Let us try to read the spice deck.

![WhatsApp Image 2025-07-26 at 13 35 53 (2)](https://github.com/user-attachments/assets/c5807e5f-b50f-46f4-ac80-dc6703bd814b)

We need to take the dimensions as the dimension of grid in spice model that we have extracted. SO we will edit the values in SPICE deck according to what mentioned


![WhatsApp Image 2025-07-26 at 13 35 53 (1)](https://github.com/user-attachments/assets/41a1e6e0-a19f-420c-9724-4642e1ca2c18)

Also include the pmos and nmos files which are there in the libs folder. Use command .include ./libs/pshort.lib for PMOS and .include ./libs/nshort.lib command for NMOS.

![WhatsApp Image 2025-07-26 at 13 35 53 (3)](https://github.com/user-attachments/assets/6f6e5588-633d-43eb-b5e3-cb9a337131b3)

Now make the definition for the supply voltage VDD VPWR 0 3.3V , VSS VGND 0 0V, and Input files Va A VGND PULSE(0v 3.3V 0 0.1ns 2ns 4ns). Also add the command .tran 1n 20n, .control , run,.endc,.end.
Also add the model files of nmos and pmos.


![WhatsApp Image 2025-07-26 at 13 35 53 (4)](https://github.com/user-attachments/assets/23bd9299-b58a-49fb-bebc-e206f80f86ab)

Now our SPICE deck is ready, run ngspice sky130_inv.spice.


![WhatsApp Image 2025-07-26 at 13 35 53 (8)](https://github.com/user-attachments/assets/95e51807-40e4-4034-a948-36d72e33ace6)

Now to plot the graph: plot y vs time a.

![WhatsApp Image 2025-07-26 at 13 35 54](https://github.com/user-attachments/assets/fe73df80-eae4-4c8a-bd38-b3fe68cfa6e4)

we can see some spikes. So we will load the spice file again, C3 change 0.24fF to 2fF.Again run ngspice

![WhatsApp Image 2025-07-26 at 13 35 53](https://github.com/user-attachments/assets/3a9a13aa-4476-4ff1-a205-a8c763596361)

![WhatsApp Image 2025-07-26 at 13 35 54 (1)](https://github.com/user-attachments/assets/2aaeb7ca-8d51-4468-a3a0-38e3efbf792c)

---
## 2.Lab steps to characterize inverter using sky130 model files
---

We need to find different parameters; 'rise tran', 'fall tran', 'propagation delay', 'fall cell delay'
a) rise tran-time taken by o/p to transit from 20% of VDD to 80% of VDD.

<img width="298" height="44" alt="Screenshot 2025-07-26 at 1 51 13 PM" src="https://github.com/user-attachments/assets/31950cc9-d671-4ca0-98b4-1be27733f0a6" />

<img width="277" height="31" alt="Screenshot 2025-07-26 at 1 51 25 PM" src="https://github.com/user-attachments/assets/e6d2a220-7e5b-4dec-9568-1fec9266d978" />

The rise time=(2.245-2.181)ns=64ps

b) fall time-time taken by o/p to transit from 80% of VDD to 20% of VDD.

<img width="316" height="65" alt="Screenshot 2025-07-26 at 1 51 35 PM" src="https://github.com/user-attachments/assets/1fd5c0ed-264b-4c41-8b5a-83c07eb9734f" />

the fall time=(8.01307-4.052)ns=3.96ns

c)cell rise delay/propagation delay-time difference between 50% of i/p and 50% of o/p when output is rising.

<img width="283" height="74" alt="Screenshot 2025-07-26 at 1 51 44 PM" src="https://github.com/user-attachments/assets/241a97ce-b3b3-43dc-989c-82b64b11bae8" />

The cell rise delay=(2.21036-2.1496)ns=60.76ps

d)cell fall delay-time difference between 50% of i/p and 50% of o/p when output is falling.

<img width="264" height="66" alt="Screenshot 2025-07-26 at 1 51 54 PM" src="https://github.com/user-attachments/assets/4d7b046d-51d4-4a35-95f9-a33e23a62115" />

The cell fall delay=(4.077-4.04988)ns=27.2ps

Therefore, we successfully have done the process calculations and characterize our inverter. Next we will create a LEF file and plugin the LEF file into picorv32a.

---
## 3.Lab introduction to Magic tool options and DRC rules
---

We need to understand the DRC rules.
For this we can go to website: http://opencircuitdesign.com/ , and learn about Magic tool and various DRC rules.
To know about skywater130 pdks: https://www.skywatertechnology.com/sky130-open-source-pdk/.
Github repository for skywater-pdks: https://github.com/google/skywater-pdk.


![WhatsApp Image 2025-07-26 at 13 59 32](https://github.com/user-attachments/assets/3c1a7883-1d75-46d6-adcc-c179cf7dcd55)

![WhatsApp Image 2025-07-26 at 13 59 32 (1)](https://github.com/user-attachments/assets/e21e264a-b646-47ca-905c-0776f56d7982)

Do ls -al to list down what is there inside.
There is a .magicrc directory in this, open it using vim .magicrc.It is the starup for magic, it verifies the technology file for magic.Although not suggested to make any changes in this directory.

![WhatsApp Image 2025-07-26 at 13 59 32 (2)](https://github.com/user-attachments/assets/5925d599-be6e-49fb-82ce-f121dfe19aa0)

---
## 5.Lab introduction to Magic and steps to load Sky130 tech-rules
---

Use the command below to open the Magic tool with improved graphics:

magic -d XR &
After Magic opens, go to the File menu and select Open, then choose the file:

met3.mag
This file contains different layout patterns, each associated with various DRC violations, represented as rule numbers. Each number corresponds to a specific design rule being violated in that region of the layout.

![WhatsApp Image 2025-07-26 at 13 59 32 (1)](https://github.com/user-attachments/assets/7a0c7fb3-3c1e-426c-b641-fa22f8fa31f5)


These rule numbers can be found in the Google-Skywater PDK documentation, which provides detailed explanations of each design rule, including layer specifications, spacing, width, enclosure, and other constraints. The reference for these rules is available at:

https://skywater-pdk.readthedocs.io/en/main/rules/periphery.html

<img width="821" height="410" alt="Screenshot 2025-07-26 at 2 22 25 PM" src="https://github.com/user-attachments/assets/0dbce8e3-6c7c-4846-af03-d1cc28af0f21" />

Now select any layout area and check the DRC violations using the "Why" option in the Tkcon window. This command will display the reason for the DRC error, along with the corresponding rule number and a brief description of the violation.


![WhatsApp Image 2025-07-26 at 13 59 33 (1)](https://github.com/user-attachments/assets/0bc94d22-ba3d-4081-bd1c-56f01d267bcd)


<img width="830" height="411" alt="Screenshot 2025-07-26 at 2 23 56 PM" src="https://github.com/user-attachments/assets/498fb76b-39f3-42be-a815-ce3ea1762d56" />

To viualze metal 3 and vias, select a blank area in the layout window. Hover the mouse pointer over the Metal3 contact icon, then press the 'p' key to pick that icon. Then execute the following command in the Tkcon tab:
```tcl
cif see VIA2
```
A group of black squares will appear inside the selected area. These represent the VIA2 layer, which indicates the vias connecting Metal2 to Metal3.


![WhatsApp Image 2025-07-26 at 13 59 33 (2)](https://github.com/user-attachments/assets/e8008a96-e5be-432e-8a50-a42e72dfac12)

---
## 6.Lab exercise to fix poly.9 error in Sky130 tech-file
---

Now, open the poly.mag file in the Magic tool using the following command in the Tkcon terminal:
```tcl
load poly.mag
```
This will load the poly layout, which can then be analyzed or edited further as required.

![WhatsApp Image 2025-07-26 at 13 59 33 (3)](https://github.com/user-attachments/assets/a3d244a5-88b7-427c-a9fa-70a7f3f0aecf)

Now consider the rule poly.9 and refer to the Google-Skywater PDK documentation to understand the details of this rule.
The rule poly.9 in the Google‑SkyWater PDK specifies:

![WhatsApp Image 2025-07-26 at 13 59 33 (4)](https://github.com/user-attachments/assets/fea444ef-3cc0-41fe-9858-992d07886620)

<img width="829" height="417" alt="Screenshot 2025-07-26 at 3 33 22 PM" src="https://github.com/user-attachments/assets/1c51281b-f594-469c-9827-9b6b8e59b574" />

Search for 'poly.9' in the sky130A.tech file. You will see a few DRCs but apparently there are no attempts to fix DRCs associated to distance between polyresistor to poly. So we will add these DRCs manually. Apply the necessary corrections to the 'poly.9' rule in both sections to resolve the DRC violation.


![WhatsApp Image 2025-07-26 at 13 59 33 (5)](https://github.com/user-attachments/assets/ea94f437-d840-4e76-8e09-b2741b480b3f)

we can see that the distance between npolyres and poly is 0.21u and as per the poly.9 DRCs checks any distance below 0.42u should viloate the rule so its clear that there are no DRCs check rules for distance between polyres and poly and hence we add them manually. below are the pictures showing the additions to sky130A.tech


![WhatsApp Image 2025-07-26 at 13 59 33 (6)](https://github.com/user-attachments/assets/1134a2c1-5e50-4bef-bb86-c86dc2a30947)



---
## Sky 130 Day 4- Pre Layout timing analysis and importance of good clock tree
---

Timing Modelling using Delay Tables

Lab Steps: Converting Grid Info to Track Info

Up to this point, we've completed the **floorplanning** and **placement** phases. We also have the `.mag` file and have learned how to extract its **SPICE model** for characterization.

However, for **placement and routing** in OpenLane, we do not need the entire `.mag` file. What is required are only the following:

* Inner and outer boundaries
* Power and ground rails
* Input and output ports

This is where the **LEF (Library Exchange Format)** file becomes important. The LEF file serves to **protect intellectual property (IP)** by including only the physical layout information necessary for placement and routing—without any internal transistor-level details.

Our next goal is to **extract the LEF file** from the `.mag` file. Once we have the LEF, it will be integrated into the **picorv32a** flow.

Guidelines for Creating Standard Cells

There are specific design rules to follow while creating standard cells:

1. **Ports Alignment**:
   All **input and output ports** must be placed at the **intersection of vertical and horizontal routing tracks**.

2. **Cell Dimensions**:

   * The **width** of the standard cell must be an **odd multiple** of the **horizontal track pitch**.
   * The **height** must be an **odd multiple** of the **vertical track pitch**.

What is a Track?

To understand what a **track** is, navigate to the following path:

```
openlane_working_dir/pdks/sky130A/libs.tech/openlane/sky130_fd_sc_hd/
```

In this directory, you'll find a file named `tracks.info`. This file defines the routing tracks—essentially, **grid lines** on which metal layers are allowed to route signals. These tracks dictate where wires can be legally placed and where pins should align in the design.


![WhatsApp Image 2025-07-26 at 15 46 02 (1)](https://github.com/user-attachments/assets/13f522da-b19e-4553-8e81-562ba53a864c)

![WhatsApp Image 2025-07-26 at 15 46 02 (2)](https://github.com/user-attachments/assets/aa0a8eb7-5e8c-47db-8df6-3da9e3bc95ff)

![WhatsApp Image 2025-07-26 at 15 46 02 (3)](https://github.com/user-attachments/assets/0aa009d1-85cb-4a40-b860-4d5b660a530c)


![WhatsApp Image 2025-07-26 at 15 46 02 (4)](https://github.com/user-attachments/assets/98d85db1-168e-4ca8-b703-277450ed1081)


In the `tracks.info` file, the entry:

```
li1 x 0.23 0.46
```

indicates that for the **li1** metal layer:

* The **horizontal track offset** is **0.23** units.
* The **horizontal pitch** (spacing between tracks) is **0.46** units.

Similarly:

```
li1 y 0.23 0.46
```

means:

* The **vertical track offset** is also **0.23** units.
* The **vertical pitch** is **0.46** units.

According to standard cell design guidelines, **input and output ports** must be placed at the **intersection of the horizontal and vertical tracks** of the **li1** layer, because port definitions are typically made using the `li1` metal.

To visually verify these tracks in **Magic** layout editor:

* Press the `g` key to enable the **grid view**.
* Zoom in on the layout.
* You'll observe small square boxes appearing—these represent the **track intersections** and serve as alignment guides for placing ports and drawing metal routes accurately.


![WhatsApp Image 2025-07-26 at 15 46 03](https://github.com/user-attachments/assets/8fb869d1-c5c5-41e2-9232-b9f2c017db4e)

Next, we align this grid with the track pitch values (offset = 0.23, pitch = 0.46) to verify whether the ports A and Y are correctly placed at the intersections of the horizontal and vertical tracks of the li1 metal layer.

Now, open the Tkcon window and, using the reference from the track.info file, set the grid according to the given offset and pitch values. The commmands are shown in the picture below.

![WhatsApp Image 2025-07-26 at 15 46 03 (1)](https://github.com/user-attachments/assets/f16535b7-1181-4ee9-a0c6-ecb96eccc2db)


![WhatsApp Image 2025-07-26 at 15 46 03 (2)](https://github.com/user-attachments/assets/73d7d1d1-8209-48d1-ba99-c51410fed83e)


so the routing of li1 layer can only happen along this grid as shown above. we can also see that the input and output port is at the intersection of horizontal and vertical metal layer.


![WhatsApp Image 2025-07-26 at 15 46 02](https://github.com/user-attachments/assets/bc146853-3bec-4d44-bc12-cdf8c339772a)

The intersection ensures that the routing can connect to the port from both the horizontal and vertical directions effectively. Now, we can observe that the ports are placed exactly at the intersection of the tracks, satisfying the first requirement. Additionally, within the cell boundaries, 3 grid boxes are covered, which confirms that the second requirement related to the cell width being an odd multiple of the track pitch is also satisfied.


---
## Lab Steps to Convert Magic Layout to Standard Cell LEF
---

Port Class

Defines the direction of the port:

* **INPUT** – The port is an input.
* **OUTPUT** – The port is an output.
* **INOUT** – The port is bidirectional (can act as both input and output).

Port Use

Defines the functional purpose of the port:

* **SIGNAL** – Regular signal port (input/output/inout).
* **POWER** – Power supply port (e.g., VDD).
* **GROUND** – Ground port (e.g., VSS).
* **CLOCK** – Clock signal port.
* **ANALOG** – Analog signal port (if applicable).


![WhatsApp Image 2025-07-26 at 15 59 52 (1)](https://github.com/user-attachments/assets/a5210691-1430-45f3-b12f-4c8fe7b43c56)

After these parameters are set (which is already configured for us), we are now ready to extract the .lef file from the .mag file.

before we extract lets give this cell a coustom name. Right now the cell name is sky130_inv. follow the below image.

![WhatsApp Image 2025-07-26 at 15 59 52 (2)](https://github.com/user-attachments/assets/07363a72-e4ae-4fe8-b540-c0e66c7bbc90)


![WhatsApp Image 2025-07-26 at 15 59 52 (3)](https://github.com/user-attachments/assets/ab0fe25d-bfe3-450e-9f28-2c7c62fede09)


Now, open the layout file in Magic using the following command:
```
magic -T sky130A.tech sky130_vsdinv.mag &
```
```
To extract the .lef file, enter the following command in the Tkcon window:
```
lef write


![WhatsApp Image 2025-07-26 at 15 59 52 (4)](https://github.com/user-attachments/assets/34f2cabc-361e-459e-9a20-0d6af975db3a)


![WhatsApp Image 2025-07-26 at 15 59 52 (5)](https://github.com/user-attachments/assets/c51adcce-b5ac-41bd-850b-0ab0d9c4a467)


![WhatsApp Image 2025-07-26 at 15 59 52 (6)](https://github.com/user-attachments/assets/26c09d0b-bc31-4fc7-abbe-060664b34ca5)

This will generate the .lef file in the vsdstdcellsdesign directory. You can verify its creation by using the command:
```tcl
ls -ltr
```

![WhatsApp Image 2025-07-26 at 15 59 52 (7)](https://github.com/user-attachments/assets/b6a8ee8c-d580-4531-87d7-e98c449df404)

The contents of the .lef file generated is shown below


![WhatsApp Image 2025-07-26 at 15 59 52 (8)](https://github.com/user-attachments/assets/b852c4ee-05d9-44d4-b6e3-3c427e889bd1)


![WhatsApp Image 2025-07-26 at 15 59 52](https://github.com/user-attachments/assets/338d5ec7-311d-4f3d-af4b-59f05d46c944)




























