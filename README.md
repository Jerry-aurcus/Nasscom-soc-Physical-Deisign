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

