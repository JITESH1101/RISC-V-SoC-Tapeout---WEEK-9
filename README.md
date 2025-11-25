# RISC-V SoC Tapeout: Weeks 7-9
## Understanding SoC Design, Simulation, Physical Design, and STA

This repository documents the learnings and execution of a RISC-V SoC tapeout program. It covers SoC fundamentals, the BabySoC model, simulation flows, physical design using OpenROAD, and Static Timing Analysis (STA).

## Table of Contents
1. [Understanding SoC Design Fundamentals](#understanding-soc-design-fundamentals-and-the-babysoc-learning-model)
    - [What is an SoC?](#what-is-a-system-on-chip-soc)
    - [Core Components](#core-components-of-a-modern-soc)
    - [BabySoC Model](#babysoc-a-simplified-model-for-learning)
2. [VSDBabySoC Simulation & Synthesis Flow](#vsdbabysoc-simulation--synthesis-flow)
    - [Project Structure](#project-structure)
    - [TLV to Verilog Conversion](#tlv-to-verilog-conversion-for-rvmyth)
    - [Pre-Synthesis Simulation](#pre-synthesis-simulation-flow)
    - [Synthesis Flow](#synthesis-flow)
    - [Post-Synthesis Simulation](#post-synthesis-simulation-flow)
3. [Fundamentals of STA](#fundamentals-of-sta-static-timing-analysis)
    - [OpenSTA Inputs & Modeling](#opensta-inputs)
    - [Timing Paths & Slack](#timing-paths)
    - [SDC Constraints](#common-sdc-constraints)
    - [Running OpenSTA](#installation-of-opensta)
4. [Physical Design (OpenROAD) - Week 7](#risc-v-soc-tapeout---week-7)
    - [Environment Setup](#part-1-setting-up-the-environment-openroad)
    - [Configuring the Project](#part-2-configuring-the-babysoc-project)
    - [Running the Flow (Synth to Route)](#part-3-running-the-full-flow-the-fun-part)
    - [SPEF Extraction](#part-4-what-is-spef)
5. [Post-Routing STA](#post-routing-sta-analysis-of-vsdbabysoc-design)

---

# Understanding SoC Design Fundamentals and the BabySoC Learning Model

This document outlines the fundamental concepts of System-on-Chip (SoC) design, explored as part of a RISC-V tapeout program. The focus is maintained on core architectural principles, the role of simplified models in education, and the importance of a structured design flow from functional modelling to physical implementation.

## What is a System-on-Chip (SoC)?

A System-on-Chip, or SoC, is an integrated circuit that consolidates all the essential components of a computer or electronic system onto a single microchip. This represents an evolution from traditional printed circuit boards (PCBs) where separate chips for processing, memory, and peripherals were interconnected. In modern electronics, this high degree of integration is essential for achieving the compact size, power efficiency, and high performance demanded by devices ranging from smartphones and wearables to complex automotive and IoT systems.

By placing all functional units in close proximity on the same piece of silicon, communication delays and the power required to drive signals between chips are significantly reduced. This leads to a more optimized and cost-effective design, a principle often summarized by the Power, Performance, and Area (PPA) optimization goals. A well-designed SoC consumes less power, delivers higher performance, and occupies a smaller physical footprint than its multi-chip counterparts.

## Core Components of a Modern SoC

A typical SoC architecture is composed of several distinct functional blocks, or Intellectual Property (IP) cores, that are integrated to work in unison. While the exact composition varies based on the intended application, several key components are almost always present:

* **Central Processing Unit (CPU):** The CPU is the computational core of the SoC, responsible for executing instructions and processing data. Modern SoCs often feature multi-core CPUs to handle complex tasks and manage operating systems. These can be homogeneous (all cores are identical) or heterogeneous (a mix of high-performance and high-efficiency cores). In the context of this program, the focus is on the open-source RISC-V instruction set architecture (ISA), which offers a compelling alternative to proprietary architectures.
* **Memory and Memory Subsystem:** Memory systems are critical for storing instructions and data. A sophisticated hierarchy is typically employed. This includes small, fast on-chip memory like L1 and L2 caches for immediate data access, larger blocks of embedded SRAM, and controllers for interfacing with external DRAM (Dynamic Random Access Memory). The subsystem also includes non-volatile memory such as Flash for permanent storage of the operating system and user data. The memory controller is a crucial piece of IP that manages the flow of data between the processor and the various memory types.
* **Graphics Processing Unit (GPU):** For systems requiring a visual interface, the GPU is a specialized processor designed to accelerate the creation of images for output to a display. It handles parallel operations efficiently, making it ideal for rendering 2D and 3D graphics, processing video, and even for general-purpose computing tasks in some applications (GPGPU).
* **Digital Signal Processor (DSP):** DSPs are highly specialized microprocessors with architectures optimized for the real-time processing of signals. In an SoC, a DSP is often used for computationally intensive tasks like audio decoding/encoding, noise reduction in voice calls, or analyzing sensor data, offloading these functions from the main CPU to improve efficiency.
* **Peripherals and I/O:** These are hardware blocks that provide specific functionalities and interface with the outside world. Peripherals can include controllers for standard communication interfaces like USB, I2C, SPI, and UART. Other specialized peripherals might include camera interfaces, display controllers, and security engines for cryptographic acceleration. Input/Output (I/O) ports are the physical connections that allow the SoC to send and receive data from external devices.
* **Interconnect:** The interconnect fabric acts as the communication backbone of the SoC. It is a system of buses or, in more complex designs, a Network-on-Chip (NoC) that facilitates data transfer between the CPU, memory, and various peripherals. The design of the interconnect is crucial for ensuring efficient data flow and avoiding performance bottlenecks. Advanced interconnects like AMBA AXI from ARM are standard in the industry, defining protocols for how different IP cores communicate, handle transactions, and manage data bandwidth.
* **Power Management Unit (PMU):** The PMU is responsible for managing the power consumption of the entire SoC. It controls voltage levels and clock frequencies for different blocks, enabling techniques like dynamic voltage and frequency scaling (DVFS). This allows parts of the chip to be powered down when not in use, which is critical for extending battery life in mobile and portable devices.

<img width="1280" height="720" alt="image" src="https://github.com/user-attachments/assets/10dd4d49-9d93-4e98-ab10-fee88987f09b" />

## BabySoC: A Simplified Model for Learning

The BabySoC project is presented as a simplified yet effective model for learning fundamental SoC concepts. Its design is intentionally minimalistic, stripping away the immense complexity of a commercial SoC to allow learning to be focused on the core principles of component integration and interaction. The educational value of BabySoC is rooted in its focused architecture, which includes:

* **An RVMYTH processor core:** This provides a clear, hands-on example of a RISC-V-based CPU. Its open-source nature is particularly valuable in an educational context, allowing for deep inspection of the architecture without proprietary restrictions. It serves as the "brain" of the system, demonstrating how a processor core executes a program to control other components.
* **A Phase-Locked Loop (PLL):** This IP core is included for stable on-chip clock generation, a critical element for teaching system timing and synchronization. In any synchronous digital system, a stable clock is paramount. An on-chip PLL is necessary because external clock sources can introduce timing variations (jitter), and distributing a single clock across a chip can lead to delays and skew. Furthermore, different blocks within an SoC may require different clock frequencies. The PLL demonstrates how an SoC can generate a precise, stable internal clock from a less precise external source, a fundamental requirement for reliable high-speed operation.

<img width="885" height="535" alt="image" src="https://github.com/user-attachments/assets/a63008da-e679-4214-b999-4ad683869d91" />

* **A 10-bit Digital-to-Analog Converter (DAC):** This serves as a practical example of a mixed-signal peripheral for interfacing with external analog systems. It tangibly demonstrates how the digital "ones and zeros" processed by the CPU can be translated into a real-world analog signal. The DAC in this context receives digital values from the RVMYTH processor and converts them into a continuous analog voltage. This function is essential for producing audio or video output for devices like televisions or speakers. Common implementations like the R-2R Ladder DAC are favored for their simplicity and scalability. The inclusion of the DAC bridges the gap between the purely digital domain of the processor and the analog nature of the physical world.

By working with BabySoC, the process of integrating a CPU with essential peripherals is made tangible. It provides a clear platform for understanding how digital processing is linked with timing control and analog output.

<img width="2270" height="1260" alt="image" src="https://github.com/user-attachments/assets/e29c6203-a12f-474d-8f6b-b0c1281e15ac" />

## The Critical Role of Functional Modelling and Design Flow

In the SoC design lifecycle, a crucial step precedes the hardware implementation phases of Register-Transfer Level (RTL) design and physical layout. This initial stage is known as functional modelling.

Functional modelling involves creating a high-level, abstract representation of the SoC, often in a language like C++ or SystemC. The purpose of this model is to verify the architectural design and intended functionality before committing to the complexities of hardware description. This "model-first" approach is indispensable for several reasons: it allows for the early detection of architectural flaws and logical errors, helps in analyzing system performance under various conditions, and provides a framework for exploring different design trade-offs with rapid iteration.

By validating the design at this abstract level, potential issues are identified and resolved when the cost and time required for correction are minimal. The functional model often serves as a "golden reference" against which the hardware implementation is later verified. This foundational step ensures that the subsequent RTL and physical design stages are built upon a functionally correct and well-vetted architecture, significantly mitigating risks in the long and expensive tapeout process. The typical design flow continues from this model to RTL design (using Verilog or VHDL), extensive verification, logic synthesis, and finally the physical design stages of floorplanning, placement, and routing before the design is sent for fabrication.

---

# VSDBabySoC Simulation & Synthesis Flow

## Project Structure

Here is my project Structure of the directory where i used all the simulations and synthesis

* `src/include/` - Contains header files (*.vh) with necessary macros or parameter definitions.
* `src/module/` - Contains Verilog files for each module in the SoC design.
* `output/` - Directory where compiled outputs and simulation files will be generated.

```text
.
├── images
│   ├── centralized_avsddac.png
│   ├── inside_dac.png
│   ├── inside_pll.png
│   ├── openlane_flow.png
│   ├── physical_design.png
│   ├── post_routing_sim.png
│   ├── post_synth_sim.png
│   ├── pre_synth_sim.png
│   ├── rvmyth_layout.png
│   ├── selected_dac.png
│   ├── selected_pll.png
│   ├── vsdbabysoc_block_diagram.png
│   └── vsdbabysoc_layout.png
├── LICENSE
├── Makefile
├── output
│   ├── post_synth_sim
│   │   ├── post_synth_sim.out
│   │   └── post_synth_sim.vcd
│   └── pre_synth_sim
│       ├── pre_synth_sim.out
│       └── pre_synth_sim.vcd
├── README.md
├── sp_env
│   ├── bin
│   ├── include
│   └── lib
├── src
│   ├── gds
│   │   ├── avsddac.gds
│   │   └── avsdpll.gds
│   ├── gls_model
│   │   ├── primitives.v
│   │   └── sky130_fd_sc_hd.v
│   ├── include
│   │   ├── sandpiper_gen.vh
│   │   ├── sandpiper.vh
│   │   ├── sp_default.vh
│   │   └── sp_verilog.vh
│   ├── layout_conf
│   │   ├── rvmyth
│   │   └── vsdbabysoc
│   ├── lef
│   │   ├── avsddac.lef
│   │   └── avsdpll.lef
│   ├── lib
│   │   ├── avsddac.lib
│   │   ├── avsdpll.lib
│   │   └── sky130_fd_sc_hd__tt_025C_1v80.lib
│   ├── module
│   │   ├── avsddac.v
│   │   ├── avsdpll.v
│   │   ├── clk_gate.v
│   │   ├── primitives.v
│   │   ├── pseudo_rand_gen.sv
│   │   ├── pseudo_rand.sv
│   │   ├── rvmyth_gen.v
│   │   ├── rvmyth.tlv
│   │   ├── rvmyth.v
│   │   ├── sky130_fd_sc_hd.v
│   │   ├── testbench.rvmyth.post-routing.v
│   │   ├── testbench.v
│   │   ├── vsdbabysoc.synth.v
│   │   └── vsdbabysoc.v
│   ├── script
│   │   ├── sta.conf
│   │   ├── verilog_to_lib.pl
│   │   └── yosys.ys
│   └── sdc
│       ├── vsdbabysoc_layout.sdc
│       └── vsdbabysoc_synthesis.sdc
└── vsdbabysoc.synth.v
