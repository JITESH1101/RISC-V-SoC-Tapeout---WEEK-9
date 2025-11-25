# RISC-V-SoC-Tapeout---WEEK-9

# Understanding SoC Design Fundamentals and the BabySoC Learning Model

This document outlines the fundamental concepts of System-on-Chip (SoC) design, explored as part of a RISC-V tapeout program. The focus is maintained on core architectural principles, the role of simplified models in education, and the importance of a structured design flow from functional modelling to physical implementation.

# What is a System-on-Chip (SoC)?

A System-on-Chip, or SoC, is an integrated circuit that consolidates all the essential components of a computer or electronic system onto a single microchip. This represents an evolution from traditional printed circuit boards (PCBs) where separate chips for processing, memory, and peripherals were interconnected. In modern electronics, this high degree of integration is essential for achieving the compact size, power efficiency, and high performance demanded by devices ranging from smartphones and wearables to complex automotive and IoT systems.

By placing all functional units in close proximity on the same piece of silicon, communication delays and the power required to drive signals between chips are significantly reduced. This leads to a more optimized and cost-effective design, a principle often summarized by the Power, Performance, and Area (PPA) optimization goals. A well-designed SoC consumes less power, delivers higher performance, and occupies a smaller physical footprint than its multi-chip counterparts.

# Core Components of a Modern SoC

A typical SoC architecture is composed of several distinct functional blocks, or Intellectual Property (IP) cores, that are integrated to work in unison. While the exact composition varies based on the intended application, several key components are almost always present:

   - Central Processing Unit (CPU): The CPU is the computational core of the SoC, responsible for executing instructions and processing data. Modern SoCs often feature multi-core CPUs to handle complex tasks and manage operating systems. These can be homogeneous (all cores are identical) or heterogeneous (a mix of high-performance and high-efficiency cores). In the context of this program, the focus is on the open-source RISC-V instruction set architecture (ISA), which offers a compelling alternative to proprietary architectures.

   - Memory and Memory Subsystem: Memory systems are critical for storing instructions and data. A sophisticated hierarchy is typically employed. This includes small, fast on-chip memory like L1 and L2 caches for immediate data access, larger blocks of embedded SRAM, and controllers for interfacing with external DRAM (Dynamic Random Access Memory). The subsystem also includes non-volatile memory such as Flash for permanent storage of the operating system and user data. The memory controller is a crucial piece of IP that manages the flow of data between the processor and the various memory types.

   - Graphics Processing Unit (GPU): For systems requiring a visual interface, the GPU is a specialized processor designed to accelerate the creation of images for output to a display. It handles parallel operations efficiently, making it ideal for rendering 2D and 3D graphics, processing video, and even for general-purpose computing tasks in some applications (GPGPU).

   - Digital Signal Processor (DSP): DSPs are highly specialized microprocessors with architectures optimized for the real-time processing of signals. In an SoC, a DSP is often used for computationally intensive tasks like audio decoding/encoding, noise reduction in voice calls, or analyzing sensor data, offloading these functions from the main CPU to improve efficiency.

   - Peripherals and I/O: These are hardware blocks that provide specific functionalities and interface with the outside world. Peripherals can include controllers for standard communication interfaces like USB, I2C, SPI, and UART. Other specialized peripherals might include camera interfaces, display controllers, and security engines for cryptographic acceleration. Input/Output (I/O) ports are the physical connections that allow the SoC to send and receive data from external devices.

   - Interconnect: The interconnect fabric acts as the communication backbone of the SoC. It is a system of buses or, in more complex designs, a Network-on-Chip (NoC) that facilitates data transfer between the CPU, memory, and various peripherals. The design of the interconnect is crucial for ensuring efficient data flow and avoiding performance bottlenecks. Advanced interconnects like AMBA AXI from ARM are standard in the industry, defining protocols for how different IP cores communicate, handle transactions, and manage data bandwidth.

   - Power Management Unit (PMU): The PMU is responsible for managing the power consumption of the entire SoC. It controls voltage levels and clock frequencies for different blocks, enabling techniques like dynamic voltage and frequency scaling (DVFS). This allows parts of the chip to be powered down when not in use, which is critical for extending battery life in mobile and portable devices.

<img width="1280" height="720" alt="image" src="https://github.com/user-attachments/assets/10dd4d49-9d93-4e98-ab10-fee88987f09b" />


# BabySoC: A Simplified Model for Learning

The BabySoC project is presented as a simplified yet effective model for learning fundamental SoC concepts. Its design is intentionally minimalistic, stripping away the immense complexity of a commercial SoC to allow learning to be focused on the core principles of component integration and interaction. The educational value of BabySoC is rooted in its focused architecture, which includes:

   - An RVMYTH processor core: This provides a clear, hands-on example of a RISC-V-based CPU. Its open-source nature is particularly valuable in an educational context, allowing for deep inspection of the architecture without proprietary restrictions. It serves as the "brain" of the system, demonstrating how a processor core executes a program to control other components.

   - A Phase-Locked Loop (PLL): This IP core is included for stable on-chip clock generation, a critical element for teaching system timing and synchronization. In any synchronous digital system, a stable clock is paramount. An on-chip PLL is necessary because external clock sources can introduce timing variations (jitter), and distributing a single clock across a chip can lead to delays and skew. Furthermore, different blocks within an SoC may require different clock frequencies. The PLL demonstrates how an SoC can generate a precise, stable internal clock from a less precise external source, a fundamental requirement for reliable high-speed operation.

<img width="885" height="535" alt="image" src="https://github.com/user-attachments/assets/a63008da-e679-4214-b999-4ad683869d91" />


   - A 10-bit Digital-to-Analog Converter (DAC): This serves as a practical example of a mixed-signal peripheral for interfacing with external analog systems. It tangibly demonstrates how the digital "ones and zeros" processed by the CPU can be translated into a real-world analog signal. The DAC in this context receives digital values from the RVMYTH processor and converts them into a continuous analog voltage. This function is essential for producing audio or video output for devices like televisions or speakers. Common implementations like the R-2R Ladder DAC are favored for their simplicity and scalability. The inclusion of the DAC bridges the gap between the purely digital domain of the processor and the analog nature of the physical world.

By working with BabySoC, the process of integrating a CPU with essential peripherals is made tangible. It provides a clear platform for understanding how digital processing is linked with timing control and analog output.

<img width="2270" height="1260" alt="image" src="https://github.com/user-attachments/assets/e29c6203-a12f-474d-8f6b-b0c1281e15ac" />

# The Critical Role of Functional Modelling and Design Flow

In the SoC design lifecycle, a crucial step precedes the hardware implementation phases of Register-Transfer Level (RTL) design and physical layout. This initial stage is known as functional modelling.

Functional modelling involves creating a high-level, abstract representation of the SoC, often in a language like C++ or SystemC. The purpose of this model is to verify the architectural design and intended functionality before committing to the complexities of hardware description. This "model-first" approach is indispensable for several reasons: it allows for the early detection of architectural flaws and logical errors, helps in analyzing system performance under various conditions, and provides a framework for exploring different design trade-offs with rapid iteration.

By validating the design at this abstract level, potential issues are identified and resolved when the cost and time required for correction are minimal. The functional model often serves as a "golden reference" against which the hardware implementation is later verified. This foundational step ensures that the subsequent RTL and physical design stages are built upon a functionally correct and well-vetted architecture, significantly mitigating risks in the long and expensive tapeout process. The typical design flow continues from this model to RTL design (using Verilog or VHDL), extensive verification, logic synthesis, and finally the physical design stages of floorplanning, placement, and routing before the design is sent for fabrication.


# VSDBabySoC Simulation & Synthesis Flow

# Project Structure

Here is my project Structure of the directory where i used all the simulations and synthesis


  - ``src/include/ ``- Contains header files (*.vh) with necessary macros or parameter definitions.
  - ``src/module/ ``- Contains Verilog files for each module in the SoC design.
  - ``output/ ``- Directory where compiled outputs and simulation files will be generated.


```
.
├── images
│   ├── centralized_avsddac.png
│   ├── inside_dac.png
│   ├── inside_pll.png
│   ├── openlane_flow.png
│   ├── physical_design.png
│   ├── post_routing_sim.png
│   ├── post_synth_sim.png
│   ├── pre_synth_sim.png
│   ├── rvmyth_layout.png
│   ├── selected_dac.png
│   ├── selected_pll.png
│   ├── vsdbabysoc_block_diagram.png
│   └── vsdbabysoc_layout.png
├── LICENSE
├── Makefile
├── output
│   ├── post_synth_sim
│   │   ├── post_synth_sim.out
│   │   └── post_synth_sim.vcd
│   └── pre_synth_sim
│       ├── pre_synth_sim.out
│       └── pre_synth_sim.vcd
├── README.md
├── sp_env
│   ├── bin
│   │   ├── activate
│   │   ├── activate.csh
│   │   ├── activate.fish
│   │   ├── Activate.ps1
│   │   ├── normalizer
│   │   ├── pip
│   │   ├── pip3
│   │   ├── pip3.12
│   │   ├── python -> python3
│   │   ├── python3 -> /usr/bin/python3
│   │   ├── python3.12 -> python3
│   │   └── sandpiper-saas
│   ├── include
│   │   └── python3.12
│   ├── lib
│   │   └── python3.12
│   │       └── site-packages
│   │           ├── argparse-1.4.0.dist-info
│   │           │   ├── DESCRIPTION.rst
│   │           │   ├── INSTALLER
│   │           │   ├── METADATA
│   │           │   ├── metadata.json
│   │           │   ├── RECORD
│   │           │   ├── top_level.txt
│   │           │   └── WHEEL
│   │           ├── argparse.py
│   │           ├── certifi
│   │           │   ├── cacert.pem
│   │           │   ├── core.py
│   │           │   ├── __init__.py
│   │           │   ├── __main__.py
│   │           │   ├── __pycache__
│   │           │   │   ├── core.cpython-312.pyc
│   │           │   │   ├── __init__.cpython-312.pyc
│   │           │   │   └── __main__.cpython-312.pyc
│   │           │   └── py.typed
│   │           ├── certifi-2025.8.3.dist-info
│   │           │   ├── INSTALLER
│   │           │   ├── licenses
│   │           │   │   └── LICENSE
│   │           │   ├── METADATA
│   │           │   ├── RECORD
│   │           │   ├── top_level.txt
│   │           │   └── WHEEL
│   │           ├── charset_normalizer
│   │           │   ├── api.py
│   │           │   ├── cd.py
│   │           │   ├── cli
│   │           │   │   ├── __init__.py
│   │           │   │   ├── __main__.py
│   │           │   │   └── __pycache__
│   │           │   │       ├── __init__.cpython-312.pyc
│   │           │   │       └── __main__.cpython-312.pyc
│   │           │   ├── constant.py
│   │           │   ├── __init__.py
│   │           │   ├── legacy.py
│   │           │   ├── __main__.py
│   │           │   ├── md.cpython-312-x86_64-linux-gnu.so
│   │           │   ├── md__mypyc.cpython-312-x86_64-linux-gnu.so
│   │           │   ├── md.py
│   │           │   ├── models.py
│   │           │   ├── __pycache__
│   │           │   │   ├── api.cpython-312.pyc
│   │           │   │   ├── cd.cpython-312.pyc
│   │           │   │   ├── constant.cpython-312.pyc
│   │           │   │   ├── __init__.cpython-312.pyc
│   │           │   │   ├── legacy.cpython-312.pyc
│   │           │   │   ├── __main__.cpython-312.pyc
│   │           │   │   ├── md.cpython-312.pyc
│   │           │   │   ├── models.cpython-312.pyc
│   │           │   │   ├── utils.cpython-312.pyc
│   │           │   │   └── version.cpython-312.pyc
│   │           │   ├── py.typed
│   │           │   ├── utils.py
│   │           │   └── version.py
│   │           ├── charset_normalizer-3.4.3.dist-info
│   │           │   ├── entry_points.txt
│   │           │   ├── INSTALLER
│   │           │   ├── licenses
│   │           │   │   └── LICENSE
│   │           │   ├── METADATA
│   │           │   ├── RECORD
│   │           │   ├── top_level.txt
│   │           │   └── WHEEL
│   │           ├── click
│   │           │   ├── _compat.py
│   │           │   ├── core.py
│   │           │   ├── decorators.py
│   │           │   ├── exceptions.py
│   │           │   ├── formatting.py
│   │           │   ├── globals.py
│   │           │   ├── __init__.py
│   │           │   ├── parser.py
│   │           │   ├── __pycache__
│   │           │   │   ├── _compat.cpython-312.pyc
│   │           │   │   ├── core.cpython-312.pyc
│   │           │   │   ├── decorators.cpython-312.pyc
│   │           │   │   ├── exceptions.cpython-312.pyc
│   │           │   │   ├── formatting.cpython-312.pyc
│   │           │   │   ├── globals.cpython-312.pyc
│   │           │   │   ├── __init__.cpython-312.pyc
│   │           │   │   ├── parser.cpython-312.pyc
│   │           │   │   ├── shell_completion.cpython-312.pyc
│   │           │   │   ├── termui.cpython-312.pyc
│   │           │   │   ├── _termui_impl.cpython-312.pyc
│   │           │   │   ├── testing.cpython-312.pyc
│   │           │   │   ├── _textwrap.cpython-312.pyc
│   │           │   │   ├── types.cpython-312.pyc
│   │           │   │   ├── _utils.cpython-312.pyc
│   │           │   │   ├── utils.cpython-312.pyc
│   │           │   │   └── _winconsole.cpython-312.pyc
│   │           │   ├── py.typed
│   │           │   ├── shell_completion.py
│   │           │   ├── _termui_impl.py
│   │           │   ├── termui.py
│   │           │   ├── testing.py
│   │           │   ├── _textwrap.py
│   │           │   ├── types.py
│   │           │   ├── _utils.py
│   │           │   ├── utils.py
│   │           │   └── _winconsole.py
│   │           ├── click-8.3.0.dist-info
│   │           │   ├── INSTALLER
│   │           │   ├── licenses
│   │           │   │   └── LICENSE.txt
│   │           │   ├── METADATA
│   │           │   ├── RECORD
│   │           │   ├── REQUESTED
│   │           │   └── WHEEL
│   │           ├── idna
│   │           │   ├── codec.py
│   │           │   ├── compat.py
│   │           │   ├── core.py
│   │           │   ├── idnadata.py
│   │           │   ├── __init__.py
│   │           │   ├── intranges.py
│   │           │   ├── package_data.py
│   │           │   ├── __pycache__
│   │           │   │   ├── codec.cpython-312.pyc
│   │           │   │   ├── compat.cpython-312.pyc
│   │           │   │   ├── core.cpython-312.pyc
│   │           │   │   ├── idnadata.cpython-312.pyc
│   │           │   │   ├── __init__.cpython-312.pyc
│   │           │   │   ├── intranges.cpython-312.pyc
│   │           │   │   ├── package_data.cpython-312.pyc
│   │           │   │   └── uts46data.cpython-312.pyc
│   │           │   ├── py.typed
│   │           │   └── uts46data.py
│   │           ├── idna-3.10.dist-info
│   │           │   ├── INSTALLER
│   │           │   ├── LICENSE.md
│   │           │   ├── METADATA
│   │           │   ├── RECORD
│   │           │   └── WHEEL
│   │           ├── path
│   │           │   ├── classes.py
│   │           │   ├── compat
│   │           │   │   ├── py38.py
│   │           │   │   └── __pycache__
│   │           │   │       └── py38.cpython-312.pyc
│   │           │   ├── __init__.py
│   │           │   ├── masks.py
│   │           │   ├── matchers.py
│   │           │   ├── __pycache__
│   │           │   │   ├── classes.cpython-312.pyc
│   │           │   │   ├── __init__.cpython-312.pyc
│   │           │   │   ├── masks.cpython-312.pyc
│   │           │   │   └── matchers.cpython-312.pyc
│   │           │   └── py.typed
│   │           ├── path-17.1.1.dist-info
│   │           │   ├── INSTALLER
│   │           │   ├── licenses
│   │           │   │   └── LICENSE
│   │           │   ├── METADATA
│   │           │   ├── RECORD
│   │           │   ├── top_level.txt
│   │           │   └── WHEEL
│   │           ├── pip
│   │           │   ├── __init__.py
│   │           │   ├── _internal
│   │           │   │   ├── build_env.py
│   │           │   │   ├── cache.py
│   │           │   │   ├── cli
│   │           │   │   │   ├── autocompletion.py
│   │           │   │   │   ├── base_command.py
│   │           │   │   │   ├── cmdoptions.py
│   │           │   │   │   ├── command_context.py
│   │           │   │   │   ├── __init__.py
│   │           │   │   │   ├── main_parser.py
│   │           │   │   │   ├── main.py
│   │           │   │   │   ├── parser.py
│   │           │   │   │   ├── progress_bars.py
│   │           │   │   │   ├── __pycache__
│   │           │   │   │   │   ├── autocompletion.cpython-312.pyc
│   │           │   │   │   │   ├── base_command.cpython-312.pyc
│   │           │   │   │   │   ├── cmdoptions.cpython-312.pyc
│   │           │   │   │   │   ├── command_context.cpython-312.pyc
│   │           │   │   │   │   ├── __init__.cpython-312.pyc
│   │           │   │   │   │   ├── main.cpython-312.pyc
│   │           │   │   │   │   ├── main_parser.cpython-312.pyc
│   │           │   │   │   │   ├── parser.cpython-312.pyc
│   │           │   │   │   │   ├── progress_bars.cpython-312.pyc
│   │           │   │   │   │   ├── req_command.cpython-312.pyc
│   │           │   │   │   │   ├── spinners.cpython-312.pyc
│   │           │   │   │   │   └── status_codes.cpython-312.pyc
│   │           │   │   │   ├── req_command.py
│   │           │   │   │   ├── spinners.py
│   │           │   │   │   └── status_codes.py
│   │           │   │   ├── commands
│   │           │   │   │   ├── cache.py
│   │           │   │   │   ├── check.py
│   │           │   │   │   ├── completion.py
│   │           │   │   │   ├── configuration.py
│   │           │   │   │   ├── debug.py
│   │           │   │   │   ├── download.py
│   │           │   │   │   ├── freeze.py
│   │           │   │   │   ├── hash.py
│   │           │   │   │   ├── help.py
│   │           │   │   │   ├── index.py
│   │           │   │   │   ├── __init__.py
│   │           │   │   │   ├── inspect.py
│   │           │   │   │   ├── install.py
│   │           │   │   │   ├── list.py
│   │           │   │   │   ├── __pycache__
│   │           │   │   │   │   ├── cache.cpython-312.pyc
│   │           │   │   │   │   ├── check.cpython-312.pyc
│   │           │   │   │   │   ├── completion.cpython-312.pyc
│   │           │   │   │   │   ├── configuration.cpython-312.pyc
│   │           │   │   │   │   ├── debug.cpython-312.pyc
│   │           │   │   │   │   ├── download.cpython-312.pyc
│   │           │   │   │   │   ├── freeze.cpython-312.pyc
│   │           │   │   │   │   ├── hash.cpython-312.pyc
│   │           │   │   │   │   ├── help.cpython-312.pyc
│   │           │   │   │   │   ├── index.cpython-312.pyc
│   │           │   │   │   │   ├── __init__.cpython-312.pyc
│   │           │   │   │   │   ├── inspect.cpython-312.pyc
│   │           │   │   │   │   ├── install.cpython-312.pyc
│   │           │   │   │   │   ├── list.cpython-312.pyc
│   │           │   │   │   │   ├── search.cpython-312.pyc
│   │           │   │   │   │   ├── show.cpython-312.pyc
│   │           │   │   │   │   ├── uninstall.cpython-312.pyc
│   │           │   │   │   │   └── wheel.cpython-312.pyc
│   │           │   │   │   ├── search.py
│   │           │   │   │   ├── show.py
│   │           │   │   │   ├── uninstall.py
│   │           │   │   │   └── wheel.py
│   │           │   │   ├── configuration.py
│   │           │   │   ├── distributions
│   │           │   │   │   ├── base.py
│   │           │   │   │   ├── __init__.py
│   │           │   │   │   ├── installed.py
│   │           │   │   │   ├── __pycache__
│   │           │   │   │   │   ├── base.cpython-312.pyc
│   │           │   │   │   │   ├── __init__.cpython-312.pyc
│   │           │   │   │   │   ├── installed.cpython-312.pyc
│   │           │   │   │   │   ├── sdist.cpython-312.pyc
│   │           │   │   │   │   └── wheel.cpython-312.pyc
│   │           │   │   │   ├── sdist.py
│   │           │   │   │   └── wheel.py
│   │           │   │   ├── exceptions.py
│   │           │   │   ├── index
│   │           │   │   │   ├── collector.py
│   │           │   │   │   ├── __init__.py
│   │           │   │   │   ├── package_finder.py
│   │           │   │   │   ├── __pycache__
│   │           │   │   │   │   ├── collector.cpython-312.pyc
│   │           │   │   │   │   ├── __init__.cpython-312.pyc
│   │           │   │   │   │   ├── package_finder.cpython-312.pyc
│   │           │   │   │   │   └── sources.cpython-312.pyc
│   │           │   │   │   └── sources.py
│   │           │   │   ├── __init__.py
│   │           │   │   ├── locations
│   │           │   │   │   ├── base.py
│   │           │   │   │   ├── _distutils.py
│   │           │   │   │   ├── __init__.py
│   │           │   │   │   ├── __pycache__
│   │           │   │   │   │   ├── base.cpython-312.pyc
│   │           │   │   │   │   ├── _distutils.cpython-312.pyc
│   │           │   │   │   │   ├── __init__.cpython-312.pyc
│   │           │   │   │   │   └── _sysconfig.cpython-312.pyc
│   │           │   │   │   └── _sysconfig.py
│   │           │   │   ├── main.py
│   │           │   │   ├── metadata
│   │           │   │   │   ├── base.py
│   │           │   │   │   ├── importlib
│   │           │   │   │   │   ├── _compat.py
│   │           │   │   │   │   ├── _dists.py
│   │           │   │   │   │   ├── _envs.py
│   │           │   │   │   │   ├── __init__.py
│   │           │   │   │   │   └── __pycache__
│   │           │   │   │   │       ├── _compat.cpython-312.pyc
│   │           │   │   │   │       ├── _dists.cpython-312.pyc
│   │           │   │   │   │       ├── _envs.cpython-312.pyc
│   │           │   │   │   │       └── __init__.cpython-312.pyc
│   │           │   │   │   ├── __init__.py
│   │           │   │   │   ├── _json.py
│   │           │   │   │   ├── pkg_resources.py
│   │           │   │   │   └── __pycache__
│   │           │   │   │       ├── base.cpython-312.pyc
│   │           │   │   │       ├── __init__.cpython-312.pyc
│   │           │   │   │       ├── _json.cpython-312.pyc
│   │           │   │   │       └── pkg_resources.cpython-312.pyc
│   │           │   │   ├── models
│   │           │   │   │   ├── candidate.py
│   │           │   │   │   ├── direct_url.py
│   │           │   │   │   ├── format_control.py
│   │           │   │   │   ├── index.py
│   │           │   │   │   ├── __init__.py
│   │           │   │   │   ├── installation_report.py
│   │           │   │   │   ├── link.py
│   │           │   │   │   ├── __pycache__
│   │           │   │   │   │   ├── candidate.cpython-312.pyc
│   │           │   │   │   │   ├── direct_url.cpython-312.pyc
│   │           │   │   │   │   ├── format_control.cpython-312.pyc
│   │           │   │   │   │   ├── index.cpython-312.pyc
│   │           │   │   │   │   ├── __init__.cpython-312.pyc
│   │           │   │   │   │   ├── installation_report.cpython-312.pyc
│   │           │   │   │   │   ├── link.cpython-312.pyc
│   │           │   │   │   │   ├── scheme.cpython-312.pyc
│   │           │   │   │   │   ├── search_scope.cpython-312.pyc
│   │           │   │   │   │   ├── selection_prefs.cpython-312.pyc
│   │           │   │   │   │   ├── target_python.cpython-312.pyc
│   │           │   │   │   │   └── wheel.cpython-312.pyc
│   │           │   │   │   ├── scheme.py
│   │           │   │   │   ├── search_scope.py
│   │           │   │   │   ├── selection_prefs.py
│   │           │   │   │   ├── target_python.py
│   │           │   │   │   └── wheel.py
│   │           │   │   ├── network
│   │           │   │   │   ├── auth.py
│   │           │   │   │   ├── cache.py
│   │           │   │   │   ├── download.py
│   │           │   │   │   ├── __init__.py
│   │           │   │   │   ├── lazy_wheel.py
│   │           │   │   │   ├── __pycache__
│   │           │   │   │   │   ├── auth.cpython-312.pyc
│   │           │   │   │   │   ├── cache.cpython-312.pyc
│   │           │   │   │   │   ├── download.cpython-312.pyc
│   │           │   │   │   │   ├── __init__.cpython-312.pyc
│   │           │   │   │   │   ├── lazy_wheel.cpython-312.pyc
│   │           │   │   │   │   ├── session.cpython-312.pyc
│   │           │   │   │   │   ├── utils.cpython-312.pyc
│   │           │   │   │   │   └── xmlrpc.cpython-312.pyc
│   │           │   │   │   ├── session.py
│   │           │   │   │   ├── utils.py
│   │           │   │   │   └── xmlrpc.py
│   │           │   │   ├── operations
│   │           │   │   │   ├── build
│   │           │   │   │   │   ├── build_tracker.py
│   │           │   │   │   │   ├── __init__.py
│   │           │   │   │   │   ├── metadata_editable.py
│   │           │   │   │   │   ├── metadata_legacy.py
│   │           │   │   │   │   ├── metadata.py
│   │           │   │   │   │   ├── __pycache__
│   │           │   │   │   │   │   ├── build_tracker.cpython-312.pyc
│   │           │   │   │   │   │   ├── __init__.cpython-312.pyc
│   │           │   │   │   │   │   ├── metadata.cpython-312.pyc
│   │           │   │   │   │   │   ├── metadata_editable.cpython-312.pyc
│   │           │   │   │   │   │   ├── metadata_legacy.cpython-312.pyc
│   │           │   │   │   │   │   ├── wheel.cpython-312.pyc
│   │           │   │   │   │   │   ├── wheel_editable.cpython-312.pyc
│   │           │   │   │   │   │   └── wheel_legacy.cpython-312.pyc
│   │           │   │   │   │   ├── wheel_editable.py
│   │           │   │   │   │   ├── wheel_legacy.py
│   │           │   │   │   │   └── wheel.py
│   │           │   │   │   ├── check.py
│   │           │   │   │   ├── freeze.py
│   │           │   │   │   ├── __init__.py
│   │           │   │   │   ├── install
│   │           │   │   │   │   ├── editable_legacy.py
│   │           │   │   │   │   ├── __init__.py
│   │           │   │   │   │   ├── __pycache__
│   │           │   │   │   │   │   ├── editable_legacy.cpython-312.pyc
│   │           │   │   │   │   │   ├── __init__.cpython-312.pyc
│   │           │   │   │   │   │   └── wheel.cpython-312.pyc
│   │           │   │   │   │   └── wheel.py
│   │           │   │   │   ├── prepare.py
│   │           │   │   │   └── __pycache__
│   │           │   │   │       ├── check.cpython-312.pyc
│   │           │   │   │       ├── freeze.cpython-312.pyc
│   │           │   │   │       ├── __init__.cpython-312.pyc
│   │           │   │   │       └── prepare.cpython-312.pyc
│   │           │   │   ├── __pycache__
│   │           │   │   │   ├── build_env.cpython-312.pyc
│   │           │   │   │   ├── cache.cpython-312.pyc
│   │           │   │   │   ├── configuration.cpython-312.pyc
│   │           │   │   │   ├── exceptions.cpython-312.pyc
│   │           │   │   │   ├── __init__.cpython-312.pyc
│   │           │   │   │   ├── main.cpython-312.pyc
│   │           │   │   │   ├── pyproject.cpython-312.pyc
│   │           │   │   │   ├── self_outdated_check.cpython-312.pyc
│   │           │   │   │   └── wheel_builder.cpython-312.pyc
│   │           │   │   ├── pyproject.py
│   │           │   │   ├── req
│   │           │   │   │   ├── constructors.py
│   │           │   │   │   ├── __init__.py
│   │           │   │   │   ├── __pycache__
│   │           │   │   │   │   ├── constructors.cpython-312.pyc
│   │           │   │   │   │   ├── __init__.cpython-312.pyc
│   │           │   │   │   │   ├── req_file.cpython-312.pyc
│   │           │   │   │   │   ├── req_install.cpython-312.pyc
│   │           │   │   │   │   ├── req_set.cpython-312.pyc
│   │           │   │   │   │   └── req_uninstall.cpython-312.pyc
│   │           │   │   │   ├── req_file.py
│   │           │   │   │   ├── req_install.py
│   │           │   │   │   ├── req_set.py
│   │           │   │   │   └── req_uninstall.py
│   │           │   │   ├── resolution
│   │           │   │   │   ├── base.py
│   │           │   │   │   ├── __init__.py
│   │           │   │   │   ├── legacy
│   │           │   │   │   │   ├── __init__.py
│   │           │   │   │   │   ├── __pycache__
│   │           │   │   │   │   │   ├── __init__.cpython-312.pyc
│   │           │   │   │   │   │   └── resolver.cpython-312.pyc
│   │           │   │   │   │   └── resolver.py
│   │           │   │   │   ├── __pycache__
│   │           │   │   │   │   ├── base.cpython-312.pyc
│   │           │   │   │   │   └── __init__.cpython-312.pyc
│   │           │   │   │   └── resolvelib
│   │           │   │   │       ├── base.py
│   │           │   │   │       ├── candidates.py
│   │           │   │   │       ├── factory.py
│   │           │   │   │       ├── found_candidates.py
│   │           │   │   │       ├── __init__.py
│   │           │   │   │       ├── provider.py
│   │           │   │   │       ├── __pycache__
│   │           │   │   │       │   ├── base.cpython-312.pyc
│   │           │   │   │       │   ├── candidates.cpython-312.pyc
│   │           │   │   │       │   ├── factory.cpython-312.pyc
│   │           │   │   │       │   ├── found_candidates.cpython-312.pyc
│   │           │   │   │       │   ├── __init__.cpython-312.pyc
│   │           │   │   │       │   ├── provider.cpython-312.pyc
│   │           │   │   │       │   ├── reporter.cpython-312.pyc
│   │           │   │   │       │   ├── requirements.cpython-312.pyc
│   │           │   │   │       │   └── resolver.cpython-312.pyc
│   │           │   │   │       ├── reporter.py
│   │           │   │   │       ├── requirements.py
│   │           │   │   │       └── resolver.py
│   │           │   │   ├── self_outdated_check.py
│   │           │   │   ├── utils
│   │           │   │   │   ├── appdirs.py
│   │           │   │   │   ├── compatibility_tags.py
│   │           │   │   │   ├── compat.py
│   │           │   │   │   ├── datetime.py
│   │           │   │   │   ├── deprecation.py
│   │           │   │   │   ├── direct_url_helpers.py
│   │           │   │   │   ├── egg_link.py
│   │           │   │   │   ├── encoding.py
│   │           │   │   │   ├── entrypoints.py
│   │           │   │   │   ├── filesystem.py
│   │           │   │   │   ├── filetypes.py
│   │           │   │   │   ├── glibc.py
│   │           │   │   │   ├── hashes.py
│   │           │   │   │   ├── __init__.py
│   │           │   │   │   ├── _jaraco_text.py
│   │           │   │   │   ├── logging.py
│   │           │   │   │   ├── _log.py
│   │           │   │   │   ├── misc.py
│   │           │   │   │   ├── models.py
│   │           │   │   │   ├── packaging.py
│   │           │   │   │   ├── __pycache__
│   │           │   │   │   │   ├── appdirs.cpython-312.pyc
│   │           │   │   │   │   ├── compat.cpython-312.pyc
│   │           │   │   │   │   ├── compatibility_tags.cpython-312.pyc
│   │           │   │   │   │   ├── datetime.cpython-312.pyc
│   │           │   │   │   │   ├── deprecation.cpython-312.pyc
│   │           │   │   │   │   ├── direct_url_helpers.cpython-312.pyc
│   │           │   │   │   │   ├── egg_link.cpython-312.pyc
│   │           │   │   │   │   ├── encoding.cpython-312.pyc
│   │           │   │   │   │   ├── entrypoints.cpython-312.pyc
│   │           │   │   │   │   ├── filesystem.cpython-312.pyc
│   │           │   │   │   │   ├── filetypes.cpython-312.pyc
│   │           │   │   │   │   ├── glibc.cpython-312.pyc
│   │           │   │   │   │   ├── hashes.cpython-312.pyc
│   │           │   │   │   │   ├── __init__.cpython-312.pyc
│   │           │   │   │   │   ├── _jaraco_text.cpython-312.pyc
│   │           │   │   │   │   ├── _log.cpython-312.pyc
│   │           │   │   │   │   ├── logging.cpython-312.pyc
│   │           │   │   │   │   ├── misc.cpython-312.pyc
│   │           │   │   │   │   ├── models.cpython-312.pyc
│   │           │   │   │   │   ├── packaging.cpython-312.pyc
│   │           │   │   │   │   ├── setuptools_build.cpython-312.pyc
│   │           │   │   │   │   ├── subprocess.cpython-312.pyc
│   │           │   │   │   │   ├── temp_dir.cpython-312.pyc
│   │           │   │   │   │   ├── unpacking.cpython-312.pyc
│   │           │   │   │   │   ├── urls.cpython-312.pyc
│   │           │   │   │   │   ├── virtualenv.cpython-312.pyc
│   │           │   │   │   │   └── wheel.cpython-312.pyc
│   │           │   │   │   ├── setuptools_build.py
│   │           │   │   │   ├── subprocess.py
│   │           │   │   │   ├── temp_dir.py
│   │           │   │   │   ├── unpacking.py
│   │           │   │   │   ├── urls.py
│   │           │   │   │   ├── virtualenv.py
│   │           │   │   │   └── wheel.py
│   │           │   │   ├── vcs
│   │           │   │   │   ├── bazaar.py
│   │           │   │   │   ├── git.py
│   │           │   │   │   ├── __init__.py
│   │           │   │   │   ├── mercurial.py
│   │           │   │   │   ├── __pycache__
│   │           │   │   │   │   ├── bazaar.cpython-312.pyc
│   │           │   │   │   │   ├── git.cpython-312.pyc
│   │           │   │   │   │   ├── __init__.cpython-312.pyc
│   │           │   │   │   │   ├── mercurial.cpython-312.pyc
│   │           │   │   │   │   ├── subversion.cpython-312.pyc
│   │           │   │   │   │   └── versioncontrol.cpython-312.pyc
│   │           │   │   │   ├── subversion.py
│   │           │   │   │   └── versioncontrol.py
│   │           │   │   └── wheel_builder.py
│   │           │   ├── __main__.py
│   │           │   ├── __pip-runner__.py
│   │           │   ├── __pycache__
│   │           │   │   ├── __init__.cpython-312.pyc
│   │           │   │   ├── __main__.cpython-312.pyc
│   │           │   │   └── __pip-runner__.cpython-312.pyc
│   │           │   ├── py.typed
│   │           │   └── _vendor
│   │           │       ├── cachecontrol
│   │           │       │   ├── adapter.py
│   │           │       │   ├── cache.py
│   │           │       │   ├── caches
│   │           │       │   │   ├── file_cache.py
│   │           │       │   │   ├── __init__.py
│   │           │       │   │   ├── __pycache__
│   │           │       │   │   │   ├── file_cache.cpython-312.pyc
│   │           │       │   │   │   ├── __init__.cpython-312.pyc
│   │           │       │   │   │   └── redis_cache.cpython-312.pyc
│   │           │       │   │   └── redis_cache.py
│   │           │       │   ├── _cmd.py
│   │           │       │   ├── controller.py
│   │           │       │   ├── filewrapper.py
│   │           │       │   ├── heuristics.py
│   │           │       │   ├── __init__.py
│   │           │       │   ├── __pycache__
│   │           │       │   │   ├── adapter.cpython-312.pyc
│   │           │       │   │   ├── cache.cpython-312.pyc
│   │           │       │   │   ├── _cmd.cpython-312.pyc
│   │           │       │   │   ├── controller.cpython-312.pyc
│   │           │       │   │   ├── filewrapper.cpython-312.pyc
│   │           │       │   │   ├── heuristics.cpython-312.pyc
│   │           │       │   │   ├── __init__.cpython-312.pyc
│   │           │       │   │   ├── serialize.cpython-312.pyc
│   │           │       │   │   └── wrapper.cpython-312.pyc
│   │           │       │   ├── serialize.py
│   │           │       │   └── wrapper.py
│   │           │       ├── certifi
│   │           │       │   ├── cacert.pem
│   │           │       │   ├── core.py
│   │           │       │   ├── __init__.py
│   │           │       │   ├── __main__.py
│   │           │       │   └── __pycache__
│   │           │       │       ├── core.cpython-312.pyc
│   │           │       │       ├── __init__.cpython-312.pyc
│   │           │       │       └── __main__.cpython-312.pyc
│   │           │       ├── chardet
│   │           │       │   ├── big5freq.py
│   │           │       │   ├── big5prober.py
│   │           │       │   ├── chardistribution.py
│   │           │       │   ├── charsetgroupprober.py
│   │           │       │   ├── charsetprober.py
│   │           │       │   ├── cli
│   │           │       │   │   ├── chardetect.py
│   │           │       │   │   ├── __init__.py
│   │           │       │   │   └── __pycache__
│   │           │       │   │       ├── chardetect.cpython-312.pyc
│   │           │       │   │       └── __init__.cpython-312.pyc
│   │           │       │   ├── codingstatemachinedict.py
│   │           │       │   ├── codingstatemachine.py
│   │           │       │   ├── cp949prober.py
│   │           │       │   ├── enums.py
│   │           │       │   ├── escprober.py
│   │           │       │   ├── escsm.py
│   │           │       │   ├── eucjpprober.py
│   │           │       │   ├── euckrfreq.py
│   │           │       │   ├── euckrprober.py
│   │           │       │   ├── euctwfreq.py
│   │           │       │   ├── euctwprober.py
│   │           │       │   ├── gb2312freq.py
│   │           │       │   ├── gb2312prober.py
│   │           │       │   ├── hebrewprober.py
│   │           │       │   ├── __init__.py
│   │           │       │   ├── jisfreq.py
│   │           │       │   ├── johabfreq.py
│   │           │       │   ├── johabprober.py
│   │           │       │   ├── jpcntx.py
│   │           │       │   ├── langbulgarianmodel.py
│   │           │       │   ├── langgreekmodel.py
│   │           │       │   ├── langhebrewmodel.py
│   │           │       │   ├── langhungarianmodel.py
│   │           │       │   ├── langrussianmodel.py
│   │           │       │   ├── langthaimodel.py
│   │           │       │   ├── langturkishmodel.py
│   │           │       │   ├── latin1prober.py
│   │           │       │   ├── macromanprober.py
│   │           │       │   ├── mbcharsetprober.py
│   │           │       │   ├── mbcsgroupprober.py
│   │           │       │   ├── mbcssm.py
│   │           │       │   ├── metadata
│   │           │       │   │   ├── __init__.py
│   │           │       │   │   ├── languages.py
│   │           │       │   │   └── __pycache__
│   │           │       │   │       ├── __init__.cpython-312.pyc
│   │           │       │   │       └── languages.cpython-312.pyc
│   │           │       │   ├── __pycache__
│   │           │       │   │   ├── big5freq.cpython-312.pyc
│   │           │       │   │   ├── big5prober.cpython-312.pyc
│   │           │       │   │   ├── chardistribution.cpython-312.pyc
│   │           │       │   │   ├── charsetgroupprober.cpython-312.pyc
│   │           │       │   │   ├── charsetprober.cpython-312.pyc
│   │           │       │   │   ├── codingstatemachine.cpython-312.pyc
│   │           │       │   │   ├── codingstatemachinedict.cpython-312.pyc
│   │           │       │   │   ├── cp949prober.cpython-312.pyc
│   │           │       │   │   ├── enums.cpython-312.pyc
│   │           │       │   │   ├── escprober.cpython-312.pyc
│   │           │       │   │   ├── escsm.cpython-312.pyc
│   │           │       │   │   ├── eucjpprober.cpython-312.pyc
│   │           │       │   │   ├── euckrfreq.cpython-312.pyc
│   │           │       │   │   ├── euckrprober.cpython-312.pyc
│   │           │       │   │   ├── euctwfreq.cpython-312.pyc
│   │           │       │   │   ├── euctwprober.cpython-312.pyc
│   │           │       │   │   ├── gb2312freq.cpython-312.pyc
│   │           │       │   │   ├── gb2312prober.cpython-312.pyc
│   │           │       │   │   ├── hebrewprober.cpython-312.pyc
│   │           │       │   │   ├── __init__.cpython-312.pyc
│   │           │       │   │   ├── jisfreq.cpython-312.pyc
│   │           │       │   │   ├── johabfreq.cpython-312.pyc
│   │           │       │   │   ├── johabprober.cpython-312.pyc
│   │           │       │   │   ├── jpcntx.cpython-312.pyc
│   │           │       │   │   ├── langbulgarianmodel.cpython-312.pyc
│   │           │       │   │   ├── langgreekmodel.cpython-312.pyc
│   │           │       │   │   ├── langhebrewmodel.cpython-312.pyc
│   │           │       │   │   ├── langhungarianmodel.cpython-312.pyc
│   │           │       │   │   ├── langrussianmodel.cpython-312.pyc
│   │           │       │   │   ├── langthaimodel.cpython-312.pyc
│   │           │       │   │   ├── langturkishmodel.cpython-312.pyc
│   │           │       │   │   ├── latin1prober.cpython-312.pyc
│   │           │       │   │   ├── macromanprober.cpython-312.pyc
│   │           │       │   │   ├── mbcharsetprober.cpython-312.pyc
│   │           │       │   │   ├── mbcsgroupprober.cpython-312.pyc
│   │           │       │   │   ├── mbcssm.cpython-312.pyc
│   │           │       │   │   ├── resultdict.cpython-312.pyc
│   │           │       │   │   ├── sbcharsetprober.cpython-312.pyc
│   │           │       │   │   ├── sbcsgroupprober.cpython-312.pyc
│   │           │       │   │   ├── sjisprober.cpython-312.pyc
│   │           │       │   │   ├── universaldetector.cpython-312.pyc
│   │           │       │   │   ├── utf1632prober.cpython-312.pyc
│   │           │       │   │   ├── utf8prober.cpython-312.pyc
│   │           │       │   │   └── version.cpython-312.pyc
│   │           │       │   ├── resultdict.py
│   │           │       │   ├── sbcharsetprober.py
│   │           │       │   ├── sbcsgroupprober.py
│   │           │       │   ├── sjisprober.py
│   │           │       │   ├── universaldetector.py
│   │           │       │   ├── utf1632prober.py
│   │           │       │   ├── utf8prober.py
│   │           │       │   └── version.py
│   │           │       ├── colorama
│   │           │       │   ├── ansi.py
│   │           │       │   ├── ansitowin32.py
│   │           │       │   ├── initialise.py
│   │           │       │   ├── __init__.py
│   │           │       │   ├── __pycache__
│   │           │       │   │   ├── ansi.cpython-312.pyc
│   │           │       │   │   ├── ansitowin32.cpython-312.pyc
│   │           │       │   │   ├── __init__.cpython-312.pyc
│   │           │       │   │   ├── initialise.cpython-312.pyc
│   │           │       │   │   ├── win32.cpython-312.pyc
│   │           │       │   │   └── winterm.cpython-312.pyc
│   │           │       │   ├── tests
│   │           │       │   │   ├── ansi_test.py
│   │           │       │   │   ├── ansitowin32_test.py
│   │           │       │   │   ├── initialise_test.py
│   │           │       │   │   ├── __init__.py
│   │           │       │   │   ├── isatty_test.py
│   │           │       │   │   ├── __pycache__
│   │           │       │   │   │   ├── ansi_test.cpython-312.pyc
│   │           │       │   │   │   ├── ansitowin32_test.cpython-312.pyc
│   │           │       │   │   │   ├── __init__.cpython-312.pyc
│   │           │       │   │   │   ├── initialise_test.cpython-312.pyc
│   │           │       │   │   │   ├── isatty_test.cpython-312.pyc
│   │           │       │   │   │   ├── utils.cpython-312.pyc
│   │           │       │   │   │   └── winterm_test.cpython-312.pyc
│   │           │       │   │   ├── utils.py
│   │           │       │   │   └── winterm_test.py
│   │           │       │   ├── win32.py
│   │           │       │   └── winterm.py
│   │           │       ├── distlib
│   │           │       │   ├── compat.py
│   │           │       │   ├── database.py
│   │           │       │   ├── index.py
│   │           │       │   ├── __init__.py
│   │           │       │   ├── locators.py
│   │           │       │   ├── manifest.py
│   │           │       │   ├── markers.py
│   │           │       │   ├── metadata.py
│   │           │       │   ├── __pycache__
│   │           │       │   │   ├── compat.cpython-312.pyc
│   │           │       │   │   ├── database.cpython-312.pyc
│   │           │       │   │   ├── index.cpython-312.pyc
│   │           │       │   │   ├── __init__.cpython-312.pyc
│   │           │       │   │   ├── locators.cpython-312.pyc
│   │           │       │   │   ├── manifest.cpython-312.pyc
│   │           │       │   │   ├── markers.cpython-312.pyc
│   │           │       │   │   ├── metadata.cpython-312.pyc
│   │           │       │   │   ├── resources.cpython-312.pyc
│   │           │       │   │   ├── scripts.cpython-312.pyc
│   │           │       │   │   ├── util.cpython-312.pyc
│   │           │       │   │   ├── version.cpython-312.pyc
│   │           │       │   │   └── wheel.cpython-312.pyc
│   │           │       │   ├── resources.py
│   │           │       │   ├── scripts.py
│   │           │       │   ├── util.py
│   │           │       │   ├── version.py
│   │           │       │   └── wheel.py
│   │           │       ├── distro
│   │           │       │   ├── distro.py
│   │           │       │   ├── __init__.py
│   │           │       │   ├── __main__.py
│   │           │       │   └── __pycache__
│   │           │       │       ├── distro.cpython-312.pyc
│   │           │       │       ├── __init__.cpython-312.pyc
│   │           │       │       └── __main__.cpython-312.pyc
│   │           │       ├── idna
│   │           │       │   ├── codec.py
│   │           │       │   ├── compat.py
│   │           │       │   ├── core.py
│   │           │       │   ├── idnadata.py
│   │           │       │   ├── __init__.py
│   │           │       │   ├── intranges.py
│   │           │       │   ├── package_data.py
│   │           │       │   ├── __pycache__
│   │           │       │   │   ├── codec.cpython-312.pyc
│   │           │       │   │   ├── compat.cpython-312.pyc
│   │           │       │   │   ├── core.cpython-312.pyc
│   │           │       │   │   ├── idnadata.cpython-312.pyc
│   │           │       │   │   ├── __init__.cpython-312.pyc
│   │           │       │   │   ├── intranges.cpython-312.pyc
│   │           │       │   │   ├── package_data.cpython-312.pyc
│   │           │       │   │   └── uts46data.cpython-312.pyc
│   │           │       │   └── uts46data.py
│   │           │       ├── __init__.py
│   │           │       ├── msgpack
│   │           │       │   ├── exceptions.py
│   │           │       │   ├── ext.py
│   │           │       │   ├── fallback.py
│   │           │       │   ├── __init__.py
│   │           │       │   └── __pycache__
│   │           │       │       ├── exceptions.cpython-312.pyc
│   │           │       │       ├── ext.cpython-312.pyc
│   │           │       │       ├── fallback.cpython-312.pyc
│   │           │       │       └── __init__.cpython-312.pyc
│   │           │       ├── packaging
│   │           │       │   ├── __about__.py
│   │           │       │   ├── __init__.py
│   │           │       │   ├── _manylinux.py
│   │           │       │   ├── markers.py
│   │           │       │   ├── _musllinux.py
│   │           │       │   ├── __pycache__
│   │           │       │   │   ├── __about__.cpython-312.pyc
│   │           │       │   │   ├── __init__.cpython-312.pyc
│   │           │       │   │   ├── _manylinux.cpython-312.pyc
│   │           │       │   │   ├── markers.cpython-312.pyc
│   │           │       │   │   ├── _musllinux.cpython-312.pyc
│   │           │       │   │   ├── requirements.cpython-312.pyc
│   │           │       │   │   ├── specifiers.cpython-312.pyc
│   │           │       │   │   ├── _structures.cpython-312.pyc
│   │           │       │   │   ├── tags.cpython-312.pyc
│   │           │       │   │   ├── utils.cpython-312.pyc
│   │           │       │   │   └── version.cpython-312.pyc
│   │           │       │   ├── requirements.py
│   │           │       │   ├── specifiers.py
│   │           │       │   ├── _structures.py
│   │           │       │   ├── tags.py
│   │           │       │   ├── utils.py
│   │           │       │   └── version.py
│   │           │       ├── pkg_resources
│   │           │       │   ├── __init__.py
│   │           │       │   └── __pycache__
│   │           │       │       └── __init__.cpython-312.pyc
│   │           │       ├── platformdirs
│   │           │       │   ├── android.py
│   │           │       │   ├── api.py
│   │           │       │   ├── __init__.py
│   │           │       │   ├── macos.py
│   │           │       │   ├── __main__.py
│   │           │       │   ├── __pycache__
│   │           │       │   │   ├── android.cpython-312.pyc
│   │           │       │   │   ├── api.cpython-312.pyc
│   │           │       │   │   ├── __init__.cpython-312.pyc
│   │           │       │   │   ├── macos.cpython-312.pyc
│   │           │       │   │   ├── __main__.cpython-312.pyc
│   │           │       │   │   ├── unix.cpython-312.pyc
│   │           │       │   │   ├── version.cpython-312.pyc
│   │           │       │   │   └── windows.cpython-312.pyc
│   │           │       │   ├── unix.py
│   │           │       │   ├── version.py
│   │           │       │   └── windows.py
│   │           │       ├── __pycache__
│   │           │       │   ├── __init__.cpython-312.pyc
│   │           │       │   ├── six.cpython-312.pyc
│   │           │       │   └── typing_extensions.cpython-312.pyc
│   │           │       ├── pygments
│   │           │       │   ├── cmdline.py
│   │           │       │   ├── console.py
│   │           │       │   ├── filter.py
│   │           │       │   ├── filters
│   │           │       │   │   ├── __init__.py
│   │           │       │   │   └── __pycache__
│   │           │       │   │       └── __init__.cpython-312.pyc
│   │           │       │   ├── formatter.py
│   │           │       │   ├── formatters
│   │           │       │   │   ├── bbcode.py
│   │           │       │   │   ├── groff.py
│   │           │       │   │   ├── html.py
│   │           │       │   │   ├── img.py
│   │           │       │   │   ├── __init__.py
│   │           │       │   │   ├── irc.py
│   │           │       │   │   ├── latex.py
│   │           │       │   │   ├── _mapping.py
│   │           │       │   │   ├── other.py
│   │           │       │   │   ├── pangomarkup.py
│   │           │       │   │   ├── __pycache__
│   │           │       │   │   │   ├── bbcode.cpython-312.pyc
│   │           │       │   │   │   ├── groff.cpython-312.pyc
│   │           │       │   │   │   ├── html.cpython-312.pyc
│   │           │       │   │   │   ├── img.cpython-312.pyc
│   │           │       │   │   │   ├── __init__.cpython-312.pyc
│   │           │       │   │   │   ├── irc.cpython-312.pyc
│   │           │       │   │   │   ├── latex.cpython-312.pyc
│   │           │       │   │   │   ├── _mapping.cpython-312.pyc
│   │           │       │   │   │   ├── other.cpython-312.pyc
│   │           │       │   │   │   ├── pangomarkup.cpython-312.pyc
│   │           │       │   │   │   ├── rtf.cpython-312.pyc
│   │           │       │   │   │   ├── svg.cpython-312.pyc
│   │           │       │   │   │   ├── terminal256.cpython-312.pyc
│   │           │       │   │   │   └── terminal.cpython-312.pyc
│   │           │       │   │   ├── rtf.py
│   │           │       │   │   ├── svg.py
│   │           │       │   │   ├── terminal256.py
│   │           │       │   │   └── terminal.py
│   │           │       │   ├── __init__.py
│   │           │       │   ├── lexer.py
│   │           │       │   ├── lexers
│   │           │       │   │   ├── __init__.py
│   │           │       │   │   ├── _mapping.py
│   │           │       │   │   ├── __pycache__
│   │           │       │   │   │   ├── __init__.cpython-312.pyc
│   │           │       │   │   │   ├── _mapping.cpython-312.pyc
│   │           │       │   │   │   └── python.cpython-312.pyc
│   │           │       │   │   └── python.py
│   │           │       │   ├── __main__.py
│   │           │       │   ├── modeline.py
│   │           │       │   ├── plugin.py
│   │           │       │   ├── __pycache__
│   │           │       │   │   ├── cmdline.cpython-312.pyc
│   │           │       │   │   ├── console.cpython-312.pyc
│   │           │       │   │   ├── filter.cpython-312.pyc
│   │           │       │   │   ├── formatter.cpython-312.pyc
│   │           │       │   │   ├── __init__.cpython-312.pyc
│   │           │       │   │   ├── lexer.cpython-312.pyc
│   │           │       │   │   ├── __main__.cpython-312.pyc
│   │           │       │   │   ├── modeline.cpython-312.pyc
│   │           │       │   │   ├── plugin.cpython-312.pyc
│   │           │       │   │   ├── regexopt.cpython-312.pyc
│   │           │       │   │   ├── scanner.cpython-312.pyc
│   │           │       │   │   ├── sphinxext.cpython-312.pyc
│   │           │       │   │   ├── style.cpython-312.pyc
│   │           │       │   │   ├── token.cpython-312.pyc
│   │           │       │   │   ├── unistring.cpython-312.pyc
│   │           │       │   │   └── util.cpython-312.pyc
│   │           │       │   ├── regexopt.py
│   │           │       │   ├── scanner.py
│   │           │       │   ├── sphinxext.py
│   │           │       │   ├── style.py
│   │           │       │   ├── styles
│   │           │       │   │   ├── __init__.py
│   │           │       │   │   └── __pycache__
│   │           │       │   │       └── __init__.cpython-312.pyc
│   │           │       │   ├── token.py
│   │           │       │   ├── unistring.py
│   │           │       │   └── util.py
│   │           │       ├── pyparsing
│   │           │       │   ├── actions.py
│   │           │       │   ├── common.py
│   │           │       │   ├── core.py
│   │           │       │   ├── diagram
│   │           │       │   │   ├── __init__.py
│   │           │       │   │   └── __pycache__
│   │           │       │   │       └── __init__.cpython-312.pyc
│   │           │       │   ├── exceptions.py
│   │           │       │   ├── helpers.py
│   │           │       │   ├── __init__.py
│   │           │       │   ├── __pycache__
│   │           │       │   │   ├── actions.cpython-312.pyc
│   │           │       │   │   ├── common.cpython-312.pyc
│   │           │       │   │   ├── core.cpython-312.pyc
│   │           │       │   │   ├── exceptions.cpython-312.pyc
│   │           │       │   │   ├── helpers.cpython-312.pyc
│   │           │       │   │   ├── __init__.cpython-312.pyc
│   │           │       │   │   ├── results.cpython-312.pyc
│   │           │       │   │   ├── testing.cpython-312.pyc
│   │           │       │   │   ├── unicode.cpython-312.pyc
│   │           │       │   │   └── util.cpython-312.pyc
│   │           │       │   ├── results.py
│   │           │       │   ├── testing.py
│   │           │       │   ├── unicode.py
│   │           │       │   └── util.py
│   │           │       ├── pyproject_hooks
│   │           │       │   ├── _compat.py
│   │           │       │   ├── _impl.py
│   │           │       │   ├── __init__.py
│   │           │       │   ├── _in_process
│   │           │       │   │   ├── __init__.py
│   │           │       │   │   ├── _in_process.py
│   │           │       │   │   └── __pycache__
│   │           │       │   │       ├── __init__.cpython-312.pyc
│   │           │       │   │       └── _in_process.cpython-312.pyc
│   │           │       │   └── __pycache__
│   │           │       │       ├── _compat.cpython-312.pyc
│   │           │       │       ├── _impl.cpython-312.pyc
│   │           │       │       └── __init__.cpython-312.pyc
│   │           │       ├── requests
│   │           │       │   ├── adapters.py
│   │           │       │   ├── api.py
│   │           │       │   ├── auth.py
│   │           │       │   ├── certs.py
│   │           │       │   ├── compat.py
│   │           │       │   ├── cookies.py
│   │           │       │   ├── exceptions.py
│   │           │       │   ├── help.py
│   │           │       │   ├── hooks.py
│   │           │       │   ├── __init__.py
│   │           │       │   ├── _internal_utils.py
│   │           │       │   ├── models.py
│   │           │       │   ├── packages.py
│   │           │       │   ├── __pycache__
│   │           │       │   │   ├── adapters.cpython-312.pyc
│   │           │       │   │   ├── api.cpython-312.pyc
│   │           │       │   │   ├── auth.cpython-312.pyc
│   │           │       │   │   ├── certs.cpython-312.pyc
│   │           │       │   │   ├── compat.cpython-312.pyc
│   │           │       │   │   ├── cookies.cpython-312.pyc
│   │           │       │   │   ├── exceptions.cpython-312.pyc
│   │           │       │   │   ├── help.cpython-312.pyc
│   │           │       │   │   ├── hooks.cpython-312.pyc
│   │           │       │   │   ├── __init__.cpython-312.pyc
│   │           │       │   │   ├── _internal_utils.cpython-312.pyc
│   │           │       │   │   ├── models.cpython-312.pyc
│   │           │       │   │   ├── packages.cpython-312.pyc
│   │           │       │   │   ├── sessions.cpython-312.pyc
│   │           │       │   │   ├── status_codes.cpython-312.pyc
│   │           │       │   │   ├── structures.cpython-312.pyc
│   │           │       │   │   ├── utils.cpython-312.pyc
│   │           │       │   │   └── __version__.cpython-312.pyc
│   │           │       │   ├── sessions.py
│   │           │       │   ├── status_codes.py
│   │           │       │   ├── structures.py
│   │           │       │   ├── utils.py
│   │           │       │   └── __version__.py
│   │           │       ├── resolvelib
│   │           │       │   ├── compat
│   │           │       │   │   ├── collections_abc.py
│   │           │       │   │   ├── __init__.py
│   │           │       │   │   └── __pycache__
│   │           │       │   │       ├── collections_abc.cpython-312.pyc
│   │           │       │   │       └── __init__.cpython-312.pyc
│   │           │       │   ├── __init__.py
│   │           │       │   ├── providers.py
│   │           │       │   ├── __pycache__
│   │           │       │   │   ├── __init__.cpython-312.pyc
│   │           │       │   │   ├── providers.cpython-312.pyc
│   │           │       │   │   ├── reporters.cpython-312.pyc
│   │           │       │   │   ├── resolvers.cpython-312.pyc
│   │           │       │   │   └── structs.cpython-312.pyc
│   │           │       │   ├── reporters.py
│   │           │       │   ├── resolvers.py
│   │           │       │   └── structs.py
│   │           │       ├── rich
│   │           │       │   ├── abc.py
│   │           │       │   ├── align.py
│   │           │       │   ├── ansi.py
│   │           │       │   ├── bar.py
│   │           │       │   ├── box.py
│   │           │       │   ├── cells.py
│   │           │       │   ├── _cell_widths.py
│   │           │       │   ├── color.py
│   │           │       │   ├── color_triplet.py
│   │           │       │   ├── columns.py
│   │           │       │   ├── console.py
│   │           │       │   ├── constrain.py
│   │           │       │   ├── containers.py
│   │           │       │   ├── control.py
│   │           │       │   ├── default_styles.py
│   │           │       │   ├── diagnose.py
│   │           │       │   ├── _emoji_codes.py
│   │           │       │   ├── emoji.py
│   │           │       │   ├── _emoji_replace.py
│   │           │       │   ├── errors.py
│   │           │       │   ├── _export_format.py
│   │           │       │   ├── _extension.py
│   │           │       │   ├── _fileno.py
│   │           │       │   ├── file_proxy.py
│   │           │       │   ├── filesize.py
│   │           │       │   ├── highlighter.py
│   │           │       │   ├── __init__.py
│   │           │       │   ├── _inspect.py
│   │           │       │   ├── json.py
│   │           │       │   ├── jupyter.py
│   │           │       │   ├── layout.py
│   │           │       │   ├── live.py
│   │           │       │   ├── live_render.py
│   │           │       │   ├── logging.py
│   │           │       │   ├── _log_render.py
│   │           │       │   ├── _loop.py
│   │           │       │   ├── __main__.py
│   │           │       │   ├── markup.py
│   │           │       │   ├── measure.py
│   │           │       │   ├── _null_file.py
│   │           │       │   ├── padding.py
│   │           │       │   ├── pager.py
│   │           │       │   ├── palette.py
│   │           │       │   ├── _palettes.py
│   │           │       │   ├── panel.py
│   │           │       │   ├── _pick.py
│   │           │       │   ├── pretty.py
│   │           │       │   ├── progress_bar.py
│   │           │       │   ├── progress.py
│   │           │       │   ├── prompt.py
│   │           │       │   ├── protocol.py
│   │           │       │   ├── __pycache__
│   │           │       │   │   ├── abc.cpython-312.pyc
│   │           │       │   │   ├── align.cpython-312.pyc
│   │           │       │   │   ├── ansi.cpython-312.pyc
│   │           │       │   │   ├── bar.cpython-312.pyc
│   │           │       │   │   ├── box.cpython-312.pyc
│   │           │       │   │   ├── cells.cpython-312.pyc
│   │           │       │   │   ├── _cell_widths.cpython-312.pyc
│   │           │       │   │   ├── color.cpython-312.pyc
│   │           │       │   │   ├── color_triplet.cpython-312.pyc
│   │           │       │   │   ├── columns.cpython-312.pyc
│   │           │       │   │   ├── console.cpython-312.pyc
│   │           │       │   │   ├── constrain.cpython-312.pyc
│   │           │       │   │   ├── containers.cpython-312.pyc
│   │           │       │   │   ├── control.cpython-312.pyc
│   │           │       │   │   ├── default_styles.cpython-312.pyc
│   │           │       │   │   ├── diagnose.cpython-312.pyc
│   │           │       │   │   ├── _emoji_codes.cpython-312.pyc
│   │           │       │   │   ├── emoji.cpython-312.pyc
│   │           │       │   │   ├── _emoji_replace.cpython-312.pyc
│   │           │       │   │   ├── errors.cpython-312.pyc
│   │           │       │   │   ├── _export_format.cpython-312.pyc
│   │           │       │   │   ├── _extension.cpython-312.pyc
│   │           │       │   │   ├── _fileno.cpython-312.pyc
│   │           │       │   │   ├── file_proxy.cpython-312.pyc
│   │           │       │   │   ├── filesize.cpython-312.pyc
│   │           │       │   │   ├── highlighter.cpython-312.pyc
│   │           │       │   │   ├── __init__.cpython-312.pyc
│   │           │       │   │   ├── _inspect.cpython-312.pyc
│   │           │       │   │   ├── json.cpython-312.pyc
│   │           │       │   │   ├── jupyter.cpython-312.pyc
│   │           │       │   │   ├── layout.cpython-312.pyc
│   │           │       │   │   ├── live.cpython-312.pyc
│   │           │       │   │   ├── live_render.cpython-312.pyc
│   │           │       │   │   ├── logging.cpython-312.pyc
│   │           │       │   │   ├── _log_render.cpython-312.pyc
│   │           │       │   │   ├── _loop.cpython-312.pyc
│   │           │       │   │   ├── __main__.cpython-312.pyc
│   │           │       │   │   ├── markup.cpython-312.pyc
│   │           │       │   │   ├── measure.cpython-312.pyc
│   │           │       │   │   ├── _null_file.cpython-312.pyc
│   │           │       │   │   ├── padding.cpython-312.pyc
│   │           │       │   │   ├── pager.cpython-312.pyc
│   │           │       │   │   ├── palette.cpython-312.pyc
│   │           │       │   │   ├── _palettes.cpython-312.pyc
│   │           │       │   │   ├── panel.cpython-312.pyc
│   │           │       │   │   ├── _pick.cpython-312.pyc
│   │           │       │   │   ├── pretty.cpython-312.pyc
│   │           │       │   │   ├── progress_bar.cpython-312.pyc
│   │           │       │   │   ├── progress.cpython-312.pyc
│   │           │       │   │   ├── prompt.cpython-312.pyc
│   │           │       │   │   ├── protocol.cpython-312.pyc
│   │           │       │   │   ├── _ratio.cpython-312.pyc
│   │           │       │   │   ├── region.cpython-312.pyc
│   │           │       │   │   ├── repr.cpython-312.pyc
│   │           │       │   │   ├── rule.cpython-312.pyc
│   │           │       │   │   ├── scope.cpython-312.pyc
│   │           │       │   │   ├── screen.cpython-312.pyc
│   │           │       │   │   ├── segment.cpython-312.pyc
│   │           │       │   │   ├── spinner.cpython-312.pyc
│   │           │       │   │   ├── _spinners.cpython-312.pyc
│   │           │       │   │   ├── _stack.cpython-312.pyc
│   │           │       │   │   ├── status.cpython-312.pyc
│   │           │       │   │   ├── style.cpython-312.pyc
│   │           │       │   │   ├── styled.cpython-312.pyc
│   │           │       │   │   ├── syntax.cpython-312.pyc
│   │           │       │   │   ├── table.cpython-312.pyc
│   │           │       │   │   ├── terminal_theme.cpython-312.pyc
│   │           │       │   │   ├── text.cpython-312.pyc
│   │           │       │   │   ├── theme.cpython-312.pyc
│   │           │       │   │   ├── themes.cpython-312.pyc
│   │           │       │   │   ├── _timer.cpython-312.pyc
│   │           │       │   │   ├── traceback.cpython-312.pyc
│   │           │       │   │   ├── tree.cpython-312.pyc
│   │           │       │   │   ├── _win32_console.cpython-312.pyc
│   │           │       │   │   ├── _windows.cpython-312.pyc
│   │           │       │   │   ├── _windows_renderer.cpython-312.pyc
│   │           │       │   │   └── _wrap.cpython-312.pyc
│   │           │       │   ├── _ratio.py
│   │           │       │   ├── region.py
│   │           │       │   ├── repr.py
│   │           │       │   ├── rule.py
│   │           │       │   ├── scope.py
│   │           │       │   ├── screen.py
│   │           │       │   ├── segment.py
│   │           │       │   ├── spinner.py
│   │           │       │   ├── _spinners.py
│   │           │       │   ├── _stack.py
│   │           │       │   ├── status.py
│   │           │       │   ├── styled.py
│   │           │       │   ├── style.py
│   │           │       │   ├── syntax.py
│   │           │       │   ├── table.py
│   │           │       │   ├── terminal_theme.py
│   │           │       │   ├── text.py
│   │           │       │   ├── theme.py
│   │           │       │   ├── themes.py
│   │           │       │   ├── _timer.py
│   │           │       │   ├── traceback.py
│   │           │       │   ├── tree.py
│   │           │       │   ├── _win32_console.py
│   │           │       │   ├── _windows.py
│   │           │       │   ├── _windows_renderer.py
│   │           │       │   └── _wrap.py
│   │           │       ├── six.py
│   │           │       ├── tenacity
│   │           │       │   ├── after.py
│   │           │       │   ├── _asyncio.py
│   │           │       │   ├── before.py
│   │           │       │   ├── before_sleep.py
│   │           │       │   ├── __init__.py
│   │           │       │   ├── nap.py
│   │           │       │   ├── __pycache__
│   │           │       │   │   ├── after.cpython-312.pyc
│   │           │       │   │   ├── _asyncio.cpython-312.pyc
│   │           │       │   │   ├── before.cpython-312.pyc
│   │           │       │   │   ├── before_sleep.cpython-312.pyc
│   │           │       │   │   ├── __init__.cpython-312.pyc
│   │           │       │   │   ├── nap.cpython-312.pyc
│   │           │       │   │   ├── retry.cpython-312.pyc
│   │           │       │   │   ├── stop.cpython-312.pyc
│   │           │       │   │   ├── tornadoweb.cpython-312.pyc
│   │           │       │   │   ├── _utils.cpython-312.pyc
│   │           │       │   │   └── wait.cpython-312.pyc
│   │           │       │   ├── retry.py
│   │           │       │   ├── stop.py
│   │           │       │   ├── tornadoweb.py
│   │           │       │   ├── _utils.py
│   │           │       │   └── wait.py
│   │           │       ├── tomli
│   │           │       │   ├── __init__.py
│   │           │       │   ├── _parser.py
│   │           │       │   ├── __pycache__
│   │           │       │   │   ├── __init__.cpython-312.pyc
│   │           │       │   │   ├── _parser.cpython-312.pyc
│   │           │       │   │   ├── _re.cpython-312.pyc
│   │           │       │   │   └── _types.cpython-312.pyc
│   │           │       │   ├── _re.py
│   │           │       │   └── _types.py
│   │           │       ├── truststore
│   │           │       │   ├── _api.py
│   │           │       │   ├── __init__.py
│   │           │       │   ├── _macos.py
│   │           │       │   ├── _openssl.py
│   │           │       │   ├── __pycache__
│   │           │       │   │   ├── _api.cpython-312.pyc
│   │           │       │   │   ├── __init__.cpython-312.pyc
│   │           │       │   │   ├── _macos.cpython-312.pyc
│   │           │       │   │   ├── _openssl.cpython-312.pyc
│   │           │       │   │   ├── _ssl_constants.cpython-312.pyc
│   │           │       │   │   └── _windows.cpython-312.pyc
│   │           │       │   ├── _ssl_constants.py
│   │           │       │   └── _windows.py
│   │           │       ├── typing_extensions.py
│   │           │       ├── urllib3
│   │           │       │   ├── _collections.py
│   │           │       │   ├── connectionpool.py
│   │           │       │   ├── connection.py
│   │           │       │   ├── contrib
│   │           │       │   │   ├── _appengine_environ.py
│   │           │       │   │   ├── appengine.py
│   │           │       │   │   ├── __init__.py
│   │           │       │   │   ├── ntlmpool.py
│   │           │       │   │   ├── __pycache__
│   │           │       │   │   │   ├── appengine.cpython-312.pyc
│   │           │       │   │   │   ├── _appengine_environ.cpython-312.pyc
│   │           │       │   │   │   ├── __init__.cpython-312.pyc
│   │           │       │   │   │   ├── ntlmpool.cpython-312.pyc
│   │           │       │   │   │   ├── pyopenssl.cpython-312.pyc
│   │           │       │   │   │   ├── securetransport.cpython-312.pyc
│   │           │       │   │   │   └── socks.cpython-312.pyc
│   │           │       │   │   ├── pyopenssl.py
│   │           │       │   │   ├── _securetransport
│   │           │       │   │   │   ├── bindings.py
│   │           │       │   │   │   ├── __init__.py
│   │           │       │   │   │   ├── low_level.py
│   │           │       │   │   │   └── __pycache__
│   │           │       │   │   │       ├── bindings.cpython-312.pyc
│   │           │       │   │   │       ├── __init__.cpython-312.pyc
│   │           │       │   │   │       └── low_level.cpython-312.pyc
│   │           │       │   │   ├── securetransport.py
│   │           │       │   │   └── socks.py
│   │           │       │   ├── exceptions.py
│   │           │       │   ├── fields.py
│   │           │       │   ├── filepost.py
│   │           │       │   ├── __init__.py
│   │           │       │   ├── packages
│   │           │       │   │   ├── backports
│   │           │       │   │   │   ├── __init__.py
│   │           │       │   │   │   ├── makefile.py
│   │           │       │   │   │   ├── __pycache__
│   │           │       │   │   │   │   ├── __init__.cpython-312.pyc
│   │           │       │   │   │   │   ├── makefile.cpython-312.pyc
│   │           │       │   │   │   │   └── weakref_finalize.cpython-312.pyc
│   │           │       │   │   │   └── weakref_finalize.py
│   │           │       │   │   ├── __init__.py
│   │           │       │   │   ├── __pycache__
│   │           │       │   │   │   ├── __init__.cpython-312.pyc
│   │           │       │   │   │   └── six.cpython-312.pyc
│   │           │       │   │   └── six.py
│   │           │       │   ├── poolmanager.py
│   │           │       │   ├── __pycache__
│   │           │       │   │   ├── _collections.cpython-312.pyc
│   │           │       │   │   ├── connection.cpython-312.pyc
│   │           │       │   │   ├── connectionpool.cpython-312.pyc
│   │           │       │   │   ├── exceptions.cpython-312.pyc
│   │           │       │   │   ├── fields.cpython-312.pyc
│   │           │       │   │   ├── filepost.cpython-312.pyc
│   │           │       │   │   ├── __init__.cpython-312.pyc
│   │           │       │   │   ├── poolmanager.cpython-312.pyc
│   │           │       │   │   ├── request.cpython-312.pyc
│   │           │       │   │   ├── response.cpython-312.pyc
│   │           │       │   │   └── _version.cpython-312.pyc
│   │           │       │   ├── request.py
│   │           │       │   ├── response.py
│   │           │       │   ├── util
│   │           │       │   │   ├── connection.py
│   │           │       │   │   ├── __init__.py
│   │           │       │   │   ├── proxy.py
│   │           │       │   │   ├── __pycache__
│   │           │       │   │   │   ├── connection.cpython-312.pyc
│   │           │       │   │   │   ├── __init__.cpython-312.pyc
│   │           │       │   │   │   ├── proxy.cpython-312.pyc
│   │           │       │   │   │   ├── queue.cpython-312.pyc
│   │           │       │   │   │   ├── request.cpython-312.pyc
│   │           │       │   │   │   ├── response.cpython-312.pyc
│   │           │       │   │   │   ├── retry.cpython-312.pyc
│   │           │       │   │   │   ├── ssl_.cpython-312.pyc
│   │           │       │   │   │   ├── ssl_match_hostname.cpython-312.pyc
│   │           │       │   │   │   ├── ssltransport.cpython-312.pyc
│   │           │       │   │   │   ├── timeout.cpython-312.pyc
│   │           │       │   │   │   ├── url.cpython-312.pyc
│   │           │       │   │   │   └── wait.cpython-312.pyc
│   │           │       │   │   ├── queue.py
│   │           │       │   │   ├── request.py
│   │           │       │   │   ├── response.py
│   │           │       │   │   ├── retry.py
│   │           │       │   │   ├── ssl_match_hostname.py
│   │           │       │   │   ├── ssl_.py
│   │           │       │   │   ├── ssltransport.py
│   │           │       │   │   ├── timeout.py
│   │           │       │   │   ├── url.py
│   │           │       │   │   └── wait.py
│   │           │       │   └── _version.py
│   │           │       ├── vendor.txt
│   │           │       └── webencodings
│   │           │           ├── __init__.py
│   │           │           ├── labels.py
│   │           │           ├── mklabels.py
│   │           │           ├── __pycache__
│   │           │           │   ├── __init__.cpython-312.pyc
│   │           │           │   ├── labels.cpython-312.pyc
│   │           │           │   ├── mklabels.cpython-312.pyc
│   │           │           │   ├── tests.cpython-312.pyc
│   │           │           │   └── x_user_defined.cpython-312.pyc
│   │           │           ├── tests.py
│   │           │           └── x_user_defined.py
│   │           ├── pip-24.0.dist-info
│   │           │   ├── AUTHORS.txt
│   │           │   ├── entry_points.txt
│   │           │   ├── INSTALLER
│   │           │   ├── LICENSE.txt
│   │           │   ├── METADATA
│   │           │   ├── RECORD
│   │           │   ├── REQUESTED
│   │           │   ├── top_level.txt
│   │           │   └── WHEEL
│   │           ├── __pycache__
│   │           │   └── argparse.cpython-312.pyc
│   │           ├── pyyaml-6.0.3.dist-info
│   │           │   ├── INSTALLER
│   │           │   ├── licenses
│   │           │   │   └── LICENSE
│   │           │   ├── METADATA
│   │           │   ├── RECORD
│   │           │   ├── REQUESTED
│   │           │   ├── top_level.txt
│   │           │   └── WHEEL
│   │           ├── requests
│   │           │   ├── adapters.py
│   │           │   ├── api.py
│   │           │   ├── auth.py
│   │           │   ├── certs.py
│   │           │   ├── compat.py
│   │           │   ├── cookies.py
│   │           │   ├── exceptions.py
│   │           │   ├── help.py
│   │           │   ├── hooks.py
│   │           │   ├── __init__.py
│   │           │   ├── _internal_utils.py
│   │           │   ├── models.py
│   │           │   ├── packages.py
│   │           │   ├── __pycache__
│   │           │   │   ├── adapters.cpython-312.pyc
│   │           │   │   ├── api.cpython-312.pyc
│   │           │   │   ├── auth.cpython-312.pyc
│   │           │   │   ├── certs.cpython-312.pyc
│   │           │   │   ├── compat.cpython-312.pyc
│   │           │   │   ├── cookies.cpython-312.pyc
│   │           │   │   ├── exceptions.cpython-312.pyc
│   │           │   │   ├── help.cpython-312.pyc
│   │           │   │   ├── hooks.cpython-312.pyc
│   │           │   │   ├── __init__.cpython-312.pyc
│   │           │   │   ├── _internal_utils.cpython-312.pyc
│   │           │   │   ├── models.cpython-312.pyc
│   │           │   │   ├── packages.cpython-312.pyc
│   │           │   │   ├── sessions.cpython-312.pyc
│   │           │   │   ├── status_codes.cpython-312.pyc
│   │           │   │   ├── structures.cpython-312.pyc
│   │           │   │   ├── utils.cpython-312.pyc
│   │           │   │   └── __version__.cpython-312.pyc
│   │           │   ├── sessions.py
│   │           │   ├── status_codes.py
│   │           │   ├── structures.py
│   │           │   ├── utils.py
│   │           │   └── __version__.py
│   │           ├── requests-2.32.5.dist-info
│   │           │   ├── INSTALLER
│   │           │   ├── licenses
│   │           │   │   └── LICENSE
│   │           │   ├── METADATA
│   │           │   ├── RECORD
│   │           │   ├── top_level.txt
│   │           │   └── WHEEL
│   │           ├── sandpiper
│   │           │   ├── __init__.py
│   │           │   ├── __main__.py
│   │           │   └── __pycache__
│   │           │       ├── __init__.cpython-312.pyc
│   │           │       └── __main__.cpython-312.pyc
│   │           ├── sandpiper_saas-1.1.0.dist-info
│   │           │   ├── entry_points.txt
│   │           │   ├── INSTALLER
│   │           │   ├── LICENSE.md
│   │           │   ├── METADATA
│   │           │   ├── RECORD
│   │           │   ├── REQUESTED
│   │           │   ├── top_level.txt
│   │           │   └── WHEEL
│   │           ├── urllib3
│   │           │   ├── _base_connection.py
│   │           │   ├── _collections.py
│   │           │   ├── connectionpool.py
│   │           │   ├── connection.py
│   │           │   ├── contrib
│   │           │   │   ├── emscripten
│   │           │   │   │   ├── connection.py
│   │           │   │   │   ├── emscripten_fetch_worker.js
│   │           │   │   │   ├── fetch.py
│   │           │   │   │   ├── __init__.py
│   │           │   │   │   ├── __pycache__
│   │           │   │   │   │   ├── connection.cpython-312.pyc
│   │           │   │   │   │   ├── fetch.cpython-312.pyc
│   │           │   │   │   │   ├── __init__.cpython-312.pyc
│   │           │   │   │   │   ├── request.cpython-312.pyc
│   │           │   │   │   │   └── response.cpython-312.pyc
│   │           │   │   │   ├── request.py
│   │           │   │   │   └── response.py
│   │           │   │   ├── __init__.py
│   │           │   │   ├── __pycache__
│   │           │   │   │   ├── __init__.cpython-312.pyc
│   │           │   │   │   ├── pyopenssl.cpython-312.pyc
│   │           │   │   │   └── socks.cpython-312.pyc
│   │           │   │   ├── pyopenssl.py
│   │           │   │   └── socks.py
│   │           │   ├── exceptions.py
│   │           │   ├── fields.py
│   │           │   ├── filepost.py
│   │           │   ├── http2
│   │           │   │   ├── connection.py
│   │           │   │   ├── __init__.py
│   │           │   │   ├── probe.py
│   │           │   │   └── __pycache__
│   │           │   │       ├── connection.cpython-312.pyc
│   │           │   │       ├── __init__.cpython-312.pyc
│   │           │   │       └── probe.cpython-312.pyc
│   │           │   ├── __init__.py
│   │           │   ├── poolmanager.py
│   │           │   ├── __pycache__
│   │           │   │   ├── _base_connection.cpython-312.pyc
│   │           │   │   ├── _collections.cpython-312.pyc
│   │           │   │   ├── connection.cpython-312.pyc
│   │           │   │   ├── connectionpool.cpython-312.pyc
│   │           │   │   ├── exceptions.cpython-312.pyc
│   │           │   │   ├── fields.cpython-312.pyc
│   │           │   │   ├── filepost.cpython-312.pyc
│   │           │   │   ├── __init__.cpython-312.pyc
│   │           │   │   ├── poolmanager.cpython-312.pyc
│   │           │   │   ├── _request_methods.cpython-312.pyc
│   │           │   │   ├── response.cpython-312.pyc
│   │           │   │   └── _version.cpython-312.pyc
│   │           │   ├── py.typed
│   │           │   ├── _request_methods.py
│   │           │   ├── response.py
│   │           │   ├── util
│   │           │   │   ├── connection.py
│   │           │   │   ├── __init__.py
│   │           │   │   ├── proxy.py
│   │           │   │   ├── __pycache__
│   │           │   │   │   ├── connection.cpython-312.pyc
│   │           │   │   │   ├── __init__.cpython-312.pyc
│   │           │   │   │   ├── proxy.cpython-312.pyc
│   │           │   │   │   ├── request.cpython-312.pyc
│   │           │   │   │   ├── response.cpython-312.pyc
│   │           │   │   │   ├── retry.cpython-312.pyc
│   │           │   │   │   ├── ssl_.cpython-312.pyc
│   │           │   │   │   ├── ssl_match_hostname.cpython-312.pyc
│   │           │   │   │   ├── ssltransport.cpython-312.pyc
│   │           │   │   │   ├── timeout.cpython-312.pyc
│   │           │   │   │   ├── url.cpython-312.pyc
│   │           │   │   │   ├── util.cpython-312.pyc
│   │           │   │   │   └── wait.cpython-312.pyc
│   │           │   │   ├── request.py
│   │           │   │   ├── response.py
│   │           │   │   ├── retry.py
│   │           │   │   ├── ssl_match_hostname.py
│   │           │   │   ├── ssl_.py
│   │           │   │   ├── ssltransport.py
│   │           │   │   ├── timeout.py
│   │           │   │   ├── url.py
│   │           │   │   ├── util.py
│   │           │   │   └── wait.py
│   │           │   └── _version.py
│   │           ├── urllib3-2.5.0.dist-info
│   │           │   ├── INSTALLER
│   │           │   ├── licenses
│   │           │   │   └── LICENSE.txt
│   │           │   ├── METADATA
│   │           │   ├── RECORD
│   │           │   └── WHEEL
│   │           ├── _yaml
│   │           │   ├── __init__.py
│   │           │   └── __pycache__
│   │           │       └── __init__.cpython-312.pyc
│   │           └── yaml
│   │               ├── composer.py
│   │               ├── constructor.py
│   │               ├── cyaml.py
│   │               ├── dumper.py
│   │               ├── emitter.py
│   │               ├── error.py
│   │               ├── events.py
│   │               ├── __init__.py
│   │               ├── loader.py
│   │               ├── nodes.py
│   │               ├── parser.py
│   │               ├── __pycache__
│   │               │   ├── composer.cpython-312.pyc
│   │               │   ├── constructor.cpython-312.pyc
│   │               │   ├── cyaml.cpython-312.pyc
│   │               │   ├── dumper.cpython-312.pyc
│   │               │   ├── emitter.cpython-312.pyc
│   │               │   ├── error.cpython-312.pyc
│   │               │   ├── events.cpython-312.pyc
│   │               │   ├── __init__.cpython-312.pyc
│   │               │   ├── loader.cpython-312.pyc
│   │               │   ├── nodes.cpython-312.pyc
│   │               │   ├── parser.cpython-312.pyc
│   │               │   ├── reader.cpython-312.pyc
│   │               │   ├── representer.cpython-312.pyc
│   │               │   ├── resolver.cpython-312.pyc
│   │               │   ├── scanner.cpython-312.pyc
│   │               │   ├── serializer.cpython-312.pyc
│   │               │   └── tokens.cpython-312.pyc
│   │               ├── reader.py
│   │               ├── representer.py
│   │               ├── resolver.py
│   │               ├── scanner.py
│   │               ├── serializer.py
│   │               ├── tokens.py
│   │               └── _yaml.cpython-312-x86_64-linux-gnu.so
│   ├── lib64 -> lib
│   └── pyvenv.cfg
├── src
│   ├── gds
│   │   ├── avsddac.gds
│   │   └── avsdpll.gds
│   ├── gls_model
│   │   ├── primitives.v
│   │   └── sky130_fd_sc_hd.v
│   ├── include
│   │   ├── sandpiper_gen.vh
│   │   ├── sandpiper.vh
│   │   ├── sp_default.vh
│   │   └── sp_verilog.vh
│   ├── layout_conf
│   │   ├── rvmyth
│   │   │   ├── config.tcl
│   │   │   └── pin_order.cfg
│   │   └── vsdbabysoc
│   │       ├── config.tcl
│   │       ├── macro.cfg
│   │       └── pin_order.cfg
│   ├── lef
│   │   ├── avsddac.lef
│   │   └── avsdpll.lef
│   ├── lib
│   │   ├── avsddac.lib
│   │   ├── avsdpll.lib
│   │   └── sky130_fd_sc_hd__tt_025C_1v80.lib
│   ├── module
│   │   ├── avsddac.v
│   │   ├── avsdpll.v
│   │   ├── clk_gate.v
│   │   ├── primitives.v
│   │   ├── pseudo_rand_gen.sv
│   │   ├── pseudo_rand.sv
│   │   ├── rvmyth_gen.v
│   │   ├── rvmyth.tlv
│   │   ├── rvmyth.v
│   │   ├── sky130_fd_sc_hd.v
│   │   ├── testbench.rvmyth.post-routing.v
│   │   ├── testbench.v
│   │   ├── vsdbabysoc.synth.v
│   │   └── vsdbabysoc.v
│   ├── script
│   │   ├── sta.conf
│   │   ├── verilog_to_lib.pl
│   │   └── yosys.ys
│   └── sdc
│       ├── vsdbabysoc_layout.sdc
│       └── vsdbabysoc_synthesis.sdc
└── vsdbabysoc.synth.v

194 directories, 1387 files
```
Here the structure might look lengthy but most of the files are part of `` sp_env`` directory which we will be creating and defining soon.
The simplified or minimized form of the structure is as follows

<img width="2829" height="2498" alt="project_structure" src="https://github.com/user-attachments/assets/27e2d8de-d58f-43d0-a214-b057d8dc6884" />

to get this structure or for the rtl files of ``VSDBabySoc`` use the following command in linux terminal

```
git clone https://github.com/manili/VSDBabySoC.git
```
now lets go into the directory and check if the files are available

<img width="1231" height="269" alt="rtl_files" src="https://github.com/user-attachments/assets/624e6c98-0ee9-40f7-96ed-64904a6268ac" />

these are the rtl files or modules of the SoC, we will  simulate and check their functionality

But we see that the ``rvmyth.tlv`` file is in ``.tlv`` format, but for synthesis ``.tlv`` format isnt supported. 

# TLV to Verilog Conversion for RVMYTH

Initially, you will see only the rvmyth.tlv file inside src/module/, since the RVMYTH core is written in TL-Verilog. To convert it into a .v file for simulation, follow the steps below:

```
# Step 1: Install python3-venv 

sudo apt update
sudo apt install python3-venv python3-pip

# Step 2: Create and activate a virtual environment

cd VSDBabySoC/
python3 -m venv sp_env
source sp_env/bin/activate

# Step 3: Install SandPiper-SaaS inside the virtual environment

pip install pyyaml click sandpiper-saas

# Step 4: Convert rvmyth.tlv to Verilog

sandpiper-saas -i ./src/module/*.tlv -o rvmyth.v --bestsv --noline -p verilog --outdir ./src/module/
```

Now that we got the verilog file for this too, lets proceed with simulations

# Pre-Synthesis Simulation Flow

- We will simulate the RTL using Iverilog and verify its functionality before synthesis
- Macro: -DPRE_SYNTH_SIM
- the outputs of the simulation are stored in the ``output/pre_synth_sim/pre_Synth_sim.out`` and ``output/pre_synth_sim/pre_Synth_sim.vcd``
- ``.out`` is used to finish simulation and write the output into ``.vcd`` file
- ``.vcd`` file is used to check the output waveform in ``gtkwave`` tool

The following command is used to run simulation of the code with all the modules included 

``
iverilog -o output/pre_synth_sim/pre_synth_sim.out   -DPRE_SYNTH_SIM   -I src/include   -I src/module   src/module/testbench.v
``

the following is the terminal log of the entire simulation flow

<img width="3568" height="984" alt="presynthesis_log" src="https://github.com/user-attachments/assets/8ee8b8a3-bb3c-4de2-a919-dc6fbee1b801" />

Now by looking at the waveform , we see the following 

<img width="3974" height="1755" alt="pre_synth_sim" src="https://github.com/user-attachments/assets/5801ae0c-7e9c-4b76-ab09-ab0f9121b4c2" />

<img width="3974" height="1755" alt="pre_synth_sim_zoom" src="https://github.com/user-attachments/assets/4cd75758-06ed-460f-8aba-a528f8980498" />

the above plotted waveform is on clk , reset , output of design , DAC signal and output of DAC.

this waveform can be used as reference behaviour of the RTL of ``VSDBABYSOC``

# Synthesis Flow

Now that we have the Pre-synthesis simulation done , we are going to use ``Yosys`` tool to perform synthesis

The purpose of doing synthesis is to get the netlist of the design and perform GLS or post synthesis Simulation to check if the design is having good behaviour even after synthesis

The following are the steps to perform synthesis for this Soc Design

- To invoke Yosys tool

```
Yosys
```

- To read the library files

```
read_liberty -lib /home/jitesh/SOC/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_liberty -lib src/lib/avsddac.lib
 
read_liberty -lib src/lib/avsdpll.lib

read_liberty -lib src/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

- To read the verilog files
  
```
read_verilog  -sv -I src/include/ -I src/module/ src/module/vsdbabysoc.v src/module/clk_gate.v src/module/rvmyth.v
```

- To define the top module for synthesis

```
synth -top vsdbabysoc
```

<img width="1685" height="1810" alt="Screenshot from 2025-10-04 12-20-50" src="https://github.com/user-attachments/assets/27c433e2-2692-426c-8d46-a10cd10820ed" />

<img width="2109" height="2247" alt="Screenshot from 2025-10-04 12-21-04" src="https://github.com/user-attachments/assets/62f3937e-05b5-44d9-9085-d624d774b9d3" />

- To map the dff with the library file

```
dfflibmap -liberty /home/jitesh/SOC/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

- To run the synthesis

```
abc -liberty src/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

- To write the netlist

```
write_verilog vsdbabysoc.synth.v
```

- To check the visual representation of the netlist

```
show vsdbabysoc
```

<img width="3963" height="752" alt="vsdbabysoc_show" src="https://github.com/user-attachments/assets/838f951b-6a95-4743-a0cc-5d559ff58391" />


Now that the synthesis is done , lets verify if its functionality is same for post synthesis simulation too.

# Post Synthesis Simulation Flow

- Here we will compile the netlist along with the standard cells from the library with Iverilog
- Macro: -DPOST_SYNTH_SIM
- the outputs of the simulation are stored in the ``output/post_synth_sim/post_Synth_sim.out`` and ``output/post_synth_sim/post_Synth_sim.vcd``
- ``.out`` is used to finish simulation and write the output into ``.vcd`` file
- ``.vcd`` file is used to check the output waveform in ``gtkwave`` tool

Before we start this simulation, to avoid errors , we have to copy and paste ``primitives.v`` and  ``sky130_fd_sc_hd.v`` in ``src/module`` . This is so because the testbench file will include these two files into simulation here , if these files are not placed here , the compiler might throw errors of misssing or file found when simulation is done.

Now the command to run post synthesis simulation is as follows

```
iverilog -o output/post_synth_sim/post_synth_sim.out -DPOST_SYNTH_SIM  -I src/include/ -I src/module/ src/module/testbench.v
```

The following is terminal log of the simulation flow done 

<img width="3573" height="774" alt="post_synth_log" src="https://github.com/user-attachments/assets/2bb5c780-f1d7-4045-9baa-77c8da96ab66" />

By looking at the waveform , same signals are plotted here too as we did with pre-synthesis simulation and we notce that the behaviour is same.

<img width="3973" height="1452" alt="post_synthesis_gtkwave" src="https://github.com/user-attachments/assets/563f7e15-a22d-4e09-9067-b41de4d9070b" />

<img width="3973" height="1452" alt="post_synth_gtkwave_zoom" src="https://github.com/user-attachments/assets/44e553e0-c383-4949-9c47-3579b0ba94a0" />

# Conclusion

With these simulationa , synthesis and analysis for the waveforms , the following points are understood

- RVMYTH ,PLL AND DAC are the key components of VSDBABYSOC

- RVMYTH Microprocessor (RISC-V CPU) is the brain of the SoC and  handles all processing tasks and communicates with other components.

- 8x Phase-Locked Loop (PLL) generates stable, synchronized clock signals and it  ensures CPU and DAC operate in harmony.

- 10-bit Digital-to-Analog Converter (DAC) Converts digital signals from RVMYTH processor into analog output.

- vsdbabysoc.v is the rtl used as top module which instantiates and connects all these modules for good functionality.


# Post-Synthesis GLS

Post synthesis GLS(Gate Level Synthesis) is done to verify the functionality of the gate level netlist of the design after synthesis. The purpose of GLS is to check if the design has the same behaviour even after synthesis/ So to do that first we do synthesis

# Synthesis Flow

Now that we have the Pre-synthesis simulation done last week , we are going to use ``Yosys`` tool to perform synthesis

The purpose of doing synthesis is to get the netlist of the design and perform GLS or post synthesis Simulation to check if the design is having good behaviour even after synthesis

The following are the steps to perform synthesis for this Soc Design

- To invoke Yosys tool

```
Yosys
```

- To read the library files

```
read_liberty -lib /home/jitesh/SOC/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_liberty -lib src/lib/avsddac.lib
 
read_liberty -lib src/lib/avsdpll.lib

read_liberty -lib src/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

- To read the verilog files
  
```
read_verilog  -sv -I src/include/ -I src/module/ src/module/vsdbabysoc.v src/module/clk_gate.v src/module/rvmyth.v
```

- To define the top module for synthesis

```
synth -top vsdbabysoc
```

<img width="1685" height="1810" alt="Screenshot from 2025-10-04 12-20-50" src="https://github.com/user-attachments/assets/27c433e2-2692-426c-8d46-a10cd10820ed" />

<img width="2109" height="2247" alt="Screenshot from 2025-10-04 12-21-04" src="https://github.com/user-attachments/assets/62f3937e-05b5-44d9-9085-d624d774b9d3" />

- To map the dff with the library file

```
dfflibmap -liberty /home/jitesh/SOC/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

- To run the synthesis

```
abc -liberty src/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

- To write the netlist

```
write_verilog vsdbabysoc.synth.v
```

- To check the visual representation of the netlist

```
show vsdbabysoc
```

<img width="3963" height="752" alt="vsdbabysoc_show" src="https://github.com/user-attachments/assets/838f951b-6a95-4743-a0cc-5d559ff58391" />


Now that the synthesis is done , lets verify if its functionality is same for post synthesis simulation too.

# Post Synthesis Simulation Flow

- Here we will compile the netlist along with the standard cells from the library with Iverilog
- Macro: -DPOST_SYNTH_SIM
- the outputs of the simulation are stored in the ``output/post_synth_sim/post_Synth_sim.out`` and ``output/post_synth_sim/post_Synth_sim.vcd``
- ``.out`` is used to finish simulation and write the output into ``.vcd`` file
- ``.vcd`` file is used to check the output waveform in ``gtkwave`` tool

Before we start this simulation, to avoid errors , we have to copy and paste ``primitives.v`` and  ``sky130_fd_sc_hd.v`` in ``src/module`` . This is so because the testbench file will include these two files into simulation here , if these files are not placed here , the compiler might throw errors of misssing or file found when simulation is done.

Now the command to run post synthesis simulation is as follows

```
iverilog -o output/post_synth_sim/post_synth_sim.out -DPOST_SYNTH_SIM  -I src/include/ -I src/module/ src/module/testbench.v
```

The following is terminal log of the simulation flow done 

<img width="3573" height="774" alt="post_synth_log" src="https://github.com/user-attachments/assets/2bb5c780-f1d7-4045-9baa-77c8da96ab66" />

By looking at the waveform , same signals are plotted here too as we did with pre-synthesis simulation and we notce that the behaviour is same.

<img width="3973" height="1452" alt="post_synthesis_gtkwave" src="https://github.com/user-attachments/assets/563f7e15-a22d-4e09-9067-b41de4d9070b" />

<img width="3973" height="1452" alt="post_synth_gtkwave_zoom" src="https://github.com/user-attachments/assets/44e553e0-c383-4949-9c47-3579b0ba94a0" />

# Fundamentals of STA (Static Timing Analysis)

Static Timing Analysis (STA) is one of the key techniques used to verify the timing performance of a digital circuit. Another common approach for timing verification is timing simulation, which evaluates both functionality and timing behavior of a design under applied input stimuli.

<img width="933" height="525" alt="image" src="https://github.com/user-attachments/assets/c99662eb-c38f-4caf-b3b1-5fdc8d5b65ab" />


The term timing analysis generally refers to either static timing analysis or timing simulation, both of which are used to detect and resolve timing-related issues in a circuit. The main distinction between them lies in their methodology — STA performs timing verification statically, without depending on input data patterns, whereas timing simulation relies on dynamic input vectors and observes the circuit’s behavior over time.

<img width="512" height="498" alt="image" src="https://github.com/user-attachments/assets/b220b3bf-7423-4307-846a-dd632a0b50b3" />

In a typical CMOS design flow, static timing analysis can be performed at multiple stages, starting from synthesis up to the post-layout stage. The OpenSTA tool, an open-source static timing analyzer, is widely used for this purpose. It performs gate-level timing verification and is controlled through a TCL command interface to read design files, apply timing constraints, and generate timing reports.


# OpenSTA Inputs

The inputs required for OpenSTA include:

.v — Gate-level Verilog netlist

.lib — Liberty timing library containing cell delays and constraints

.sdc — Synopsys Design Constraints file defining clocks, delays, and exceptions

.sdf — Standard Delay Format file (optional, for annotated delays)

.spef — Parasitic file containing RC extraction data

.vcd / .saif — Switching activity files used for power estimation


# Clock Modeling in STA

Clock modeling plays a vital role in accurate timing analysis. In OpenSTA, clocks are defined and refined using several parameters:

- Generated Clocks: Derived from existing clocks through logic or division.

- Latency: Represents clock propagation delay.

- Source Latency: Specifies delay from the clock source to the clock input pin.

- Uncertainty: Accounts for jitter or skew margin.

- Propagated vs. Ideal: Differentiates between real and ideal clock distribution networks.

- Gated Clock Checks: Ensures proper verification of conditionally enabled clocks.

- Multi-Frequency Clocks: Allows analysis across multiple clock domains.


# Timing Exceptions

Timing exceptions refine the analysis to better represent real circuit behavior:

set_false_path: Excludes non-functional or invalid paths.

set_multicycle_path: Allows data to propagate over multiple clock cycles.

set_max_delay / set_min_delay: Defines custom upper and lower bounds for path delays.


# Delay Calculation

Accurate delay computation is essential in STA.
OpenSTA employs the Dartu/Menezes/Pileggi effective capacitance algorithm to model realistic RC network delays efficiently. This method provides a balance between accuracy and computational runtime.

In addition, OpenSTA supports an external delay calculator API, allowing integration of advanced delay models — such as layout-aware or temperature-dependent timing models — into custom design environments.

# Timing Analysis and Reporting

Timing reports in OpenSTA are generated using the report_checks command, which identifies timing violations across specified paths.
For example:

```
report_checks -from [get_pins U1/Q] -to [get_pins U2/D]
```

This command lists all setup and hold violations between the specified start and end pins, allowing engineers to pinpoint timing-critical regions.

<img width="1804" height="607" alt="image" src="https://github.com/user-attachments/assets/425ef36a-e7de-4903-846a-f26b71bfbbf4" />


# Timing Paths

A timing path represents the logical route that a signal takes within a circuit — from its start point to its end point — passing through various combinational logic elements. STA examines each path to evaluate delays, setup and hold margins, and verify compliance with timing constraints.

Types of Timing Paths:

- Input to Register (in2reg)

- Register to Register (reg2reg)

- Register to Output (reg2out)

- Input to Output (in2out)

Path Elements:

- Start Point: Origin of a signal, either an input port or a register clock pin (launch edge).

- End Point: Destination where the signal terminates, typically a register data pin or output port.

- Combinational Logic: Logic gates or cells between the start and end points that determine data delay.

- The critical path is the longest delay path in the design, dictating the maximum operating frequency.

<img width="646" height="521" alt="image" src="https://github.com/user-attachments/assets/164a4ceb-ec83-43b0-93b7-7bc9349065b8" />


# Setup and Hold Checks

- Setup Check:

The setup time defines the minimum duration for which data must remain stable before the active clock edge. Violation of setup time can lead to incorrect data being latched by the flip-flop.
Setup time depends on the technology, operating conditions, and library characterization.

- Hold Check:

The hold time specifies the minimum duration for which data must remain stable after the active clock edge. Violations cause data corruption or metastability. Proper hold margin ensures reliable data capture.

# Slack Calculation

Slack quantifies how well the design meets its timing requirements.

- ``Setup Slack = Data Required Time – Data Arrival Time``

- ``Hold Slack = Data Arrival Time – Data Required Time``

Where:

Data Arrival Time: The time taken for data to propagate from the start point to the endpoint.

Data Required Time: The time dictated by the clock path for data capture.

Slack Interpretation:

- Positive Slack: Timing requirements are met.

- Zero Slack: Design operates at the critical limit.

- Negative Slack: Timing violation; optimization is required.

# Common SDC Constraints

Synopsys Design Constraints (SDC) guide the STA tool to accurately model the operating environment, timing intent, and physical limitations of the design. These constraints are grouped into several categories:

| **Category**             | **Purpose / Commands**                                                                                                                                                                                                                                                                     |
| ------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Operating Conditions** | `set_operating_conditions` — Defines process, voltage, and temperature (PVT) corners.                                                                                                                                                                                                      |
| **Wire-load Models**     | `set_wire_load_mode`, `set_wire_load_model`, `set_wire_load_selection_group` — Estimate interconnect delays before parasitic extraction.                                                                                                                                                   |
| **Environmental**        | `set_drive`, `set_driving_cell`, `set_load`, `set_fanout_load`, `set_input_transition`, `set_port_fanout_number` — Define input drive strength, output load, and signal environment.                                                                                                       |
| **Design Rules**         | `set_max_capacitance`, `set_max_fanout`, `set_max_transition` — Constrain fanout, load, and transition limits to ensure signal integrity.                                                                                                                                                  |
| **Timing**               | `create_clock`, `create_generated_clock`, `set_clock_latency`, `set_clock_transition`, `set_clock_uncertainty`, `set_propagated_clock`, `set_disable_timing`, `set_input_delay`, `set_output_delay` — Define clock behavior, input/output relationships, and specify analysis preferences. |
| **Exceptions**           | `set_false_path`, `set_multicycle_path`, `set_max_delay` — Exclude or modify special timing paths.                                                                                                                                                                                         |
| **Power**                | `set_max_dynamic_power`, `set_max_leakage_power` — Define allowable power budgets for analysis and optimization.                                                                                                                                                                           |

These constraints ensure that STA accurately reflects the real operating environment and identifies timing issues early in the design cycle.

# Timing Graphs with OpenSTA

To generate timing graphs ,  we first need to install the OpenSTA tool 

# Installation of OpenSTA

For installing OpenSTA tool , the following commands are used

- clone the repository

```
git clone https://github.com/parallaxsw/OpenSTA.git
cd OpenSTA
```

<img width="2657" height="591" alt="Screenshot from 2025-10-11 17-03-17" src="https://github.com/user-attachments/assets/a95c4156-4a48-4dac-89f2-42dac8a1ea9a" />

- Build the Docker image

```
docker build --file Dockerfile.ubuntu22.04 --tag opensta .
```


<img width="2461" height="975" alt="Screenshot from 2025-10-11 17-04-21" src="https://github.com/user-attachments/assets/968e8afa-5496-473b-8a2e-6c0b8c2df2e4" />

we are using this to install and open tool through docker container

- open the STA container

```
docker run -i -v $HOME:/home opensta
```

# Downloading the timing libraries

Now that we have installed openSTA, we need to download the timing libraries to run timing analysis

We can clone the following directory to get the timing libraries

```
https://github.com/efabless/skywater-pdk-libs-sky130_fd_sc_hd/tree/master/timing
```

but we cant directly clone a sub directory, so we do the following 

```
git clone --no-checkout https://github.com/efabless/skywater-pdk-libs-sky130_fd_sc_hd.git
cd skywater-pdk-libs-sky130_fd_sc_hd
git sparse-checkout init --cone
git sparse-checkout set timing
git checkout
```

# Constraints(SDC)

Since we have libraries too , now we use the following as constraints in ``.sdc`` file

```
set_units -time ns
create_clock [get_pins {pll/CLK}] -name clk -period 11
```

# TCL script

The following is the ``.tcl`` script to automate the entire process

```
 set list_of_lib_files(1) "sky130_fd_sc_hd__tt_025C_1v80.lib"
 set list_of_lib_files(2) "sky130_fd_sc_hd__ff_100C_1v65.lib"
 set list_of_lib_files(3) "sky130_fd_sc_hd__ff_100C_1v95.lib"
 set list_of_lib_files(4) "sky130_fd_sc_hd__ff_n40C_1v56.lib"
 set list_of_lib_files(5) "sky130_fd_sc_hd__ff_n40C_1v65.lib"
 set list_of_lib_files(6) "sky130_fd_sc_hd__ff_n40C_1v76.lib"
 set list_of_lib_files(7) "sky130_fd_sc_hd__ss_100C_1v40.lib"
 set list_of_lib_files(8) "sky130_fd_sc_hd__ss_100C_1v60.lib"
 set list_of_lib_files(9) "sky130_fd_sc_hd__ss_n40C_1v28.lib"
 set list_of_lib_files(10) "sky130_fd_sc_hd__ss_n40C_1v35.lib"
 set list_of_lib_files(11) "sky130_fd_sc_hd__ss_n40C_1v40.lib"
 set list_of_lib_files(12) "sky130_fd_sc_hd__ss_n40C_1v44.lib"
 set list_of_lib_files(13) "sky130_fd_sc_hd__ss_n40C_1v76.lib"

 read_liberty /home/VSDBabySoC/src/lib/avsdpll.lib
 read_liberty /home/VSDBabySoC/src/lib/avsddac.lib

 for {set i 1} {$i <= [array size list_of_lib_files]} {incr i} {
 read_liberty /home/jitesh/VSDBabySoC/skywater-pdk-libs-sky130_fd_sc_hd/timing/$list_of_lib_files($i)
 read_verilog /home/VSDBabySoC/src/vsdbabysoc.synth.v
 link_design vsdbabysoc
 current_design
 read_sdc /home/VSDBabySoC/src/sdc/constraints.sdc
 check_setup -verbose
 report_checks -path_delay min_max -fields {nets cap slew input_pins fanout} -digits {4} > /home/VSDBabySoC/STA_OUTPUT/min_max_$list_of_lib_files($i).txt

 exec echo "$list_of_lib_files($i)" >> /home/VSDBabySoC/STA_OUTPUT/sta_worst_max_slack.txt
 report_worst_slack -max -digits {4} >> /home/VSDBabySoC/STA_OUTPUT/sta_worst_max_slack.txt

 exec echo "$list_of_lib_files($i)" >> /home/VSDBabySoC/STA_OUTPUT/sta_worst_min_slack.txt
 report_worst_slack -min -digits {4} >> /home/VSDBabySoC/STA_OUTPUT/sta_worst_min_slack.txt

 exec echo "$list_of_lib_files($i)" >> /home/VSDBabySoC/STA_OUTPUT/sta_tns.txt
 report_tns -digits {4} >> /home/VSDBabySoC/STA_OUTPUT/sta_tns.txt

 exec echo "$list_of_lib_files($i)" >> /home/VSDBabySoC/STA_OUTPUT/sta_wns.txt
 report_wns -digits {4} >> /home/VSDBabySoC/STA_OUTPUT/sta_wns.txt
 }
```

# Timing Analysis and graphs

Now that we have all the requirements sorted , we open the terminal and enter the following command]

```
docker run -it -v $HOME:/home opensta /home/VSDBabySoC/src/script/script.tcl
```

This command will invoke the opensta tool and will go to the mentioned path and source the previously shown tcl script and it will run each and every command mentioned in  the tcl script.

The following are the pictures of Timing Analysis and Timing graphs of VSDBabySoC using OpenSTA

<img width="880" height="337" alt="image" src="https://github.com/user-attachments/assets/bccb5379-6b44-45eb-ace6-e20a7bb3d415" />

- Worst Setup Slack

<img width="1189" height="590" alt="image" src="https://github.com/user-attachments/assets/76f5fec1-0598-43a2-bdae-ef3ec5161aec" />

- Worst Hold Slack

<img width="1189" height="590" alt="image" src="https://github.com/user-attachments/assets/c1a772b5-7ea7-43c5-953c-ddfcae4e7beb" />

- WNS

<img width="1189" height="590" alt="image" src="https://github.com/user-attachments/assets/4415d4d3-dc4a-49f0-be73-a1e6120f26f0" />

- TNS

<img width="1190" height="590" alt="image" src="https://github.com/user-attachments/assets/4c0ab626-9e51-4b9f-801a-1fb6783feb02" />


# RISC-V-SoC-Tapeout---WEEK-7

 The main goal this week was to take a synthesized RISC-V SoC design, the `VsdBabySoC`, and run it through the entire physical design (PD) flow using the **OpenROAD-flow-scripts (ORFS)**.

This task was all about connecting the dots—seeing how the digital logic we write in Verilog (`RTL`) gets translated into a physical, routable layout (`GDSII`) ready for fabrication. It's where the theory of chip design meets the practical reality of layout, timing, and parasitic extraction.

##  The Objective: A Full Physical Design Flow

Up to this point, many steps like synthesis, STA, and layout were separate exercises. The goal of this task was to integrate them into one continuous flow, just like in a real-world ASIC design process.

My mission was to:
1.  **Set up the Environment:** Install and configure the OpenROAD-flow-scripts.
2.  **Configure the Project:** Teach the flow about our `BabySoC` design, its Verilog files, and its special macros (like the PLL and DAC).
3.  **Run the Flow:** Execute all the main physical design stages:
    * Synthesis (if not already done)
    * Floorplanning
    * Placement
    * Clock Tree Synthesis (CTS)
    * Routing
4.  **Extract Parasitics:** Generate the critical **SPEF** (Standard Parasitic Exchange Format) file, which is essential for accurate, post-layout timing signoff.

---

##  Part 1: Setting Up the Environment (OpenROAD)

First things first, I had to get the toolchain (ORFS) installed and built. This flow automates the entire process, using a set of open-source tools like Yosys, OpenROAD, and Magic.

Here are the commands I used to get the environment ready:

```bash
# 1. Clone the repository (recursively to get all submodules)
git clone --recursive [https://github.com/The-OpenROAD-Project/OpenROAD-flow-scripts](https://github.com/The-OpenROAD-Project/OpenROAD-flow-scripts)

# 2. Change into the directory
cd OpenROAD-flow-scripts

# 3. Run the dependency setup script
sudo ./setup.sh

# 4. Build the OpenROAD tool itself (locally)
./build_openroad.sh --local

# 5. VERY IMPORTANT: Source the environment file to set up all the paths
# You have to do this every time you open a new terminal
source ./env.sh

# 6. Run a quick check to make sure the tools are in the path
yosys -help
openroad -help
```

<img width="3790" height="450" alt="directory" src="https://github.com/user-attachments/assets/16f3cf1a-bdc8-4284-b6ea-c208a3f0b880" />

To verify the installation , enter the following code to run the entire automated flow to check if the design works.

```
source ./env.sh
yosys -help
openroad -help
cd flow
make
```

<img width="3986" height="2302" alt="make_openroad_flow_scripts" src="https://github.com/user-attachments/assets/f43f6aff-a4ca-469b-a8c8-5608215c79b8" />

<img width="3975" height="2499" alt="gui_final" src="https://github.com/user-attachments/assets/70e9f7fc-168a-4f2c-8219-d974b4cbed6b" />


---

##  Part 2: Configuring the BabySoC Project

This was the most important setup step. The OpenROAD flow needs to know *where* our design files are and *how* to build them for our target technology (Skywater 130nm, or `sky130hd`).

The flow cleverly separates the **design-specific files** (`src`) from the **platform-specific files** (`sky130hd`).

1.  **Design Source (`/flow/designs/src/vsdbabysoc/`)**
    * This is the PDK-independent folder.
    * I placed all the core Verilog files here (`vsdbabysoc.v`, `rvmyth.v`, `clk_gate.v`).

2.  **Platform Config (`/flow/designs/sky130hd/vsdbabysoc/`)**
    * This folder tells the flow *how* to build our design for `sky130hd`.
    * I copied all the tech-specific files here:
        * `gds/`: GDS files for the hard macros (`avsddac.gds`, `avsdpll.gds`).
        * `lef/`: LEF files for the macros (`avsddac.lef`, `avsdpll.lef`).
        * `lib/`: Timing `.lib` files for the macros (`avsddac.lib`, `avsdpll.lib`).
        * `sdc/`: The timing constraints for synthesis (`vsdbabysoc_synthesis.sdc`).
        * `macro.cfg` & `pin_order.cfg`: Files to guide the floorplanner.

<img width="1815" height="753" alt="tree" src="https://github.com/user-attachments/assets/232b6095-878f-401a-825a-919f956a01b3" />


### The Most important File: `config.mk`

This file, which I created inside `/flow/designs/sky130hd/vsdbabysoc/`, is the main "control panel" for the flow. It points to all the files we just organized and sets our design parameters.

```makefile
# --- Basic Design Info ---
export DESIGN_NICKNAME = vsdbabysoc
export DESIGN_NAME = vsdbabysoc
export PLATFORM    = sky130hd

# --- Source Files ---
# Point to all the Verilog files needed for synthesis
export VERILOG_FILES = $(DESIGN_HOME)/src/$(DESIGN_NICKNAME)/module/vsdbabysoc.v \
                       $(DESIGN_HOME)/src/$(DESIGN_NICKNAME)/module/rvmyth.v \
                       $(DESIGN_HOME)/src/$(DESIGN_NICKNAME)/module/clk_gate.v

# Point to the SDC (timing constraints) file
export SDC_FILE      = $(DESIGN_HOME)/$(PLATFORM)/$(DESIGN_NICKNAME)/sdc/vsdbabysoc_synthesis.sdc

# --- Macro / IP Configuration ---
export vsdbabysoc_DIR = $(DESIGN_HOME)/$(PLATFORM)/$(DESIGN_NICKNAME)
export VERILOG_INCLUDE_DIRS = $(wildcard $(vsdbabysoc_DIR)/include/)
export ADDITIONAL_GDS  = $(wildcard $(vsdbabysoc_DIR)/gds/*.gds)
export ADDITIONAL_LEFS  = $(wildcard $(vsdbabysoc_DIR)/lef/*.lef)
export ADDITIONAL_LIBS = $(wildcard $(vsdbabysoc_DIR)/lib/*.lib)

# --- Clock Configuration ---
export CLOCK_PORT = CLK
export CLOCK_NET = $(CLOCK_PORT)

# --- Floorplan & Die Config ---
export DIE_AREA   = 0 0 1600 1600
export CORE_AREA  = 20 20 1590 1590
export FP_PIN_ORDER_CFG = $(wildcard $(vsdbabysoc_DIR)/pin_order.cfg)
export MACRO_PLACEMENT_CFG = $(wildcard $(vsdbabysoc_DIR)/macro.cfg)

# --- Placement Config ---
export PLACE_PINS_ARGS = -exclude left:0-600 -exclude left:1000-1600: -exclude right:* -exclude top:* -exclude bottom:*

# --- CTS Tuning ---
export CTS_BUF_DISTANCE = 600
export SKIP_GATE_CLONING = 1
```

---

##  Part 3: Running the Full Flow (The Fun Part!)

With the setup complete, all I had to do was `cd` into the `flow/` directory and run the `make` commands for each stage. The `DESIGN_CONFIG` variable points to the `config.mk` file we just made.

### 1. Synthesis

This step runs Yosys to turn the Verilog into a gate-level netlist.
```bash
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk synth
```
The following is the synthesis log

<img width="3964" height="2506" alt="synthesis_1" src="https://github.com/user-attachments/assets/427da15f-b2f5-4e9c-97dc-d8d3b09f897f" />
<img width="3964" height="2506" alt="synthesis_2" src="https://github.com/user-attachments/assets/4976dff9-1a8c-44af-a000-ed14f8f54e9c" />
<img width="3964" height="2506" alt="synthesis_3" src="https://github.com/user-attachments/assets/0554caa2-6e53-4302-8316-92dac7c2aadc" />
<img width="3964" height="2506" alt="synthesis_4" src="https://github.com/user-attachments/assets/545f21cf-2a93-4c24-83a8-df8bf775a084" />
<img width="3964" height="2506" alt="synthesis_5" src="https://github.com/user-attachments/assets/b0da1aea-46f2-48cc-8be7-41af232f4585" />
<img width="3964" height="754" alt="synthesis_6" src="https://github.com/user-attachments/assets/54e624e1-cdf3-4781-b21e-e948cde9c882" />

Now the netlist generated is as follows

<img width="3978" height="2560" alt="netlist" src="https://github.com/user-attachments/assets/f6a260db-e360-43f5-8086-bcd3a4b3cb01" />
<img width="3978" height="2560" alt="netlist_1" src="https://github.com/user-attachments/assets/b5cac365-fe9a-402d-b71d-1208cc67ba25" />

The following is from ``synth_check.txt``

<img width="3978" height="512" alt="synth_check" src="https://github.com/user-attachments/assets/0c8c4fe3-8159-4486-8f82-4bb4e0424b83" />

Now the following picture is from ``Synth_stat`` file

<img width="3978" height="2510" alt="synth_stat" src="https://github.com/user-attachments/assets/30002dd2-f007-4d29-b3c6-e0b0d87bfb9d" />
<img width="3978" height="2510" alt="synth_stat1" src="https://github.com/user-attachments/assets/615009e3-57b7-4128-895f-ae0061f6f9b3" />
<img width="3978" height="2510" alt="synth_stat2" src="https://github.com/user-attachments/assets/9a33a719-e043-4172-8b97-4401e41738e2" />

With this detailed analysis , we see that everything is set in place and is working fine and now we can directly go through the entire PNR flow unless we are stopped by any possible error.

### 2. Floorplan
Here, the flow defines the die area, places the I/O pins, and positions our hard macros (PLL and DAC) according to the `macro.cfg`.
```bash
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk floorplan
```
To see the result, I used the GUI command:
```bash
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk gui_floorplan
```
<img width="1561" height="716" alt="floorplan" src="https://github.com/user-attachments/assets/616d3a67-5ac8-45f4-a986-8ce41ce88a79" />


### 3. Placement
This step takes all the standard cells from the netlist and finds a legal, optimized position for them inside the core area.
```bash
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk place
```
And to view the placement heatmap (to check for congestion):
```bash
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk gui_place
```
<img width="1501" height="716" alt="placement" src="https://github.com/user-attachments/assets/a178412a-a466-4bee-b4f8-26c83f6a7969" />

Now click on z to zoom into this to observe the heatmap

<img width="1506" height="702" alt="heatmap" src="https://github.com/user-attachments/assets/ab09e827-f7ce-4dbb-89bf-224353e8e411" />


### 4. Clock Tree Synthesis (CTS)

This is a critical step where the tool builds a balanced buffer tree to distribute the clock signal (`CLK`) to all the flip-flops with minimal skew.
```bash
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk cts
```
<img width="1507" height="698" alt="cts" src="https://github.com/user-attachments/assets/04723964-aec5-4a10-8113-620731f1dfd6" />


After this step, the tool runs a Static Timing Analysis (STA). and generates the following report

```
==========================================================================
cts final report_tns
--------------------------------------------------------------------------
tns 0.00

==========================================================================
cts final report_wns
--------------------------------------------------------------------------
wns 0.00

==========================================================================
cts final report_worst_slack
--------------------------------------------------------------------------
worst slack 5.55

==========================================================================
cts final report_clock_skew
--------------------------------------------------------------------------
Clock clk
   0.92 source latency core.CPU_result_a4[0]$_DFF_P_/CLK ^
  -0.82 target latency core.CPU_Dmem_value_a5[0][18]$_SDFFE_PP0P_/CLK ^
   0.55 clock uncertainty
   0.00 CRPR
--------------
   0.65 setup skew


==========================================================================
cts final report_checks -path_delay min
--------------------------------------------------------------------------
Startpoint: core.CPU_Xreg_value_a4[25][15]$_SDFFE_PP0P_
            (rising edge-triggered flip-flop clocked by clk)
Endpoint: core.CPU_src2_value_a3[15]$_DFF_P_
          (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: min

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                          0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock source latency
     1    0.10    0.00    0.00    0.00 ^ pll/CLK (avsdpll)
                                         CLK (net)
                  0.00    0.00    0.00 ^ clkbuf_0_CLK/A (sky130_fd_sc_hd__clkbuf_16)
     8    0.23    0.24    0.26    0.26 ^ clkbuf_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_0_CLK (net)
                  0.24    0.00    0.26 ^ clkbuf_3_7_0_CLK/A (sky130_fd_sc_hd__clkbuf_16)
     2    0.04    0.06    0.20    0.47 ^ clkbuf_3_7_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_3_7_0_CLK (net)
                  0.06    0.00    0.47 ^ clkbuf_4_14__f_CLK/A (sky130_fd_sc_hd__clkbuf_16)
    13    0.17    0.18    0.24    0.71 ^ clkbuf_4_14__f_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_4_14__leaf_CLK (net)
                  0.18    0.00    0.71 ^ clkbuf_leaf_108_CLK/A (sky130_fd_sc_hd__clkbuf_16)
    12    0.04    0.06    0.19    0.90 ^ clkbuf_leaf_108_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_leaf_108_CLK (net)
                  0.06    0.00    0.90 ^ core.CPU_Xreg_value_a4[25][15]$_SDFFE_PP0P_/CLK (sky130_fd_sc_hd__dfxtp_1)
     2    0.01    0.06    0.32    1.22 ^ core.CPU_Xreg_value_a4[25][15]$_SDFFE_PP0P_/Q (sky130_fd_sc_hd__dfxtp_1)
                                         core.CPU_Xreg_value_a4[25][15] (net)
                  0.06    0.00    1.22 ^ _14708_/A1 (sky130_fd_sc_hd__a21oi_1)
     1    0.00    0.04    0.06    1.28 v _14708_/Y (sky130_fd_sc_hd__a21oi_1)
                                         _02012_ (net)
                  0.04    0.00    1.28 v _14712_/A (sky130_fd_sc_hd__nand4_1)
     1    0.01    0.13    0.12    1.40 ^ _14712_/Y (sky130_fd_sc_hd__nand4_1)
                                         _02016_ (net)
                  0.13    0.00    1.40 ^ _14729_/A1 (sky130_fd_sc_hd__o32ai_4)
     1    0.04    0.12    0.18    1.58 v _14729_/Y (sky130_fd_sc_hd__o32ai_4)
                                         _02033_ (net)
                  0.12    0.00    1.59 v _14731_/A2 (sky130_fd_sc_hd__o21ai_0)
     1    0.00    0.06    0.16    1.74 ^ _14731_/Y (sky130_fd_sc_hd__o21ai_0)
                                         core.CPU_src2_value_a2[15] (net)
                  0.06    0.00    1.74 ^ core.CPU_src2_value_a3[15]$_DFF_P_/D (sky130_fd_sc_hd__dfxtp_2)
                                  1.74   data arrival time

                          0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock source latency
     1    0.10    0.00    0.00    0.00 ^ pll/CLK (avsdpll)
                                         CLK (net)
                  0.00    0.00    0.00 ^ clkbuf_0_CLK/A (sky130_fd_sc_hd__clkbuf_16)
     8    0.23    0.24    0.26    0.26 ^ clkbuf_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_0_CLK (net)
                  0.24    0.00    0.26 ^ clkbuf_3_6_0_CLK/A (sky130_fd_sc_hd__clkbuf_16)
     2    0.04    0.06    0.21    0.47 ^ clkbuf_3_6_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_3_6_0_CLK (net)
                  0.06    0.00    0.47 ^ clkbuf_4_13__f_CLK/A (sky130_fd_sc_hd__clkbuf_16)
    12    0.16    0.18    0.24    0.71 ^ clkbuf_4_13__f_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_4_13__leaf_CLK (net)
                  0.18    0.00    0.71 ^ clkbuf_leaf_70_CLK/A (sky130_fd_sc_hd__clkbuf_16)
    12    0.04    0.06    0.18    0.89 ^ clkbuf_leaf_70_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_leaf_70_CLK (net)
                  0.06    0.00    0.89 ^ core.CPU_src2_value_a3[15]$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_2)
                          0.88    1.78   clock uncertainty
                          0.00    1.78   clock reconvergence pessimism
                         -0.03    1.74   library hold time
                                  1.74   data required time
-----------------------------------------------------------------------------
                                  1.74   data required time
                                 -1.74   data arrival time
-----------------------------------------------------------------------------
                                  0.00   slack (MET)



==========================================================================
cts final report_checks -path_delay max
--------------------------------------------------------------------------
Startpoint: core.CPU_valid_taken_br_a5$_DFF_P_
            (rising edge-triggered flip-flop clocked by clk)
Endpoint: core.CPU_Xreg_value_a4[7][13]$_SDFFE_PP0P_
          (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: max

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                          0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock source latency
     1    0.10    0.00    0.00    0.00 ^ pll/CLK (avsdpll)
                                         CLK (net)
                  0.00    0.00    0.00 ^ clkbuf_0_CLK/A (sky130_fd_sc_hd__clkbuf_16)
     8    0.23    0.24    0.26    0.26 ^ clkbuf_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_0_CLK (net)
                  0.24    0.00    0.26 ^ clkbuf_3_4_0_CLK/A (sky130_fd_sc_hd__clkbuf_16)
     2    0.04    0.07    0.21    0.47 ^ clkbuf_3_4_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_3_4_0_CLK (net)
                  0.07    0.00    0.47 ^ clkbuf_4_8__f_CLK/A (sky130_fd_sc_hd__clkbuf_16)
    11    0.15    0.16    0.23    0.70 ^ clkbuf_4_8__f_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_4_8__leaf_CLK (net)
                  0.16    0.00    0.70 ^ clkbuf_leaf_149_CLK/A (sky130_fd_sc_hd__clkbuf_16)
    15    0.04    0.06    0.18    0.88 ^ clkbuf_leaf_149_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_leaf_149_CLK (net)
                  0.06    0.00    0.88 ^ core.CPU_valid_taken_br_a5$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_1)
     1    0.00    0.03    0.30    1.18 v core.CPU_valid_taken_br_a5$_DFF_P_/Q (sky130_fd_sc_hd__dfxtp_1)
                                         core.CPU_valid_taken_br_a5 (net)
                  0.03    0.00    1.18 v _07913_/A (sky130_fd_sc_hd__or4_4)
    19    0.22    0.35    0.83    2.01 v _07913_/X (sky130_fd_sc_hd__or4_4)
                                         _02930_ (net)
                  0.35    0.01    2.02 v _07915_/A (sky130_fd_sc_hd__clkinv_16)
    43    0.45    0.31    0.36    2.38 ^ _07915_/Y (sky130_fd_sc_hd__clkinv_16)
                                         _02932_ (net)
                  0.31    0.02    2.39 ^ _09981_/C (sky130_fd_sc_hd__nor3_2)
     2    0.02    0.10    0.14    2.53 v _09981_/Y (sky130_fd_sc_hd__nor3_2)
                                         _04371_ (net)
                  0.10    0.00    2.53 v _09982_/B1 (sky130_fd_sc_hd__a21oi_4)
     6    0.10    0.70    0.57    3.10 ^ _09982_/Y (sky130_fd_sc_hd__a21oi_4)
                                         _04372_ (net)
                  0.70    0.00    3.10 ^ _09988_/A2 (sky130_fd_sc_hd__o21ai_4)
    16    0.12    0.33    0.38    3.47 v _09988_/Y (sky130_fd_sc_hd__o21ai_4)
                                         _04378_ (net)
                  0.33    0.00    3.48 v _13552_/A (sky130_fd_sc_hd__nor3_4)
    23    0.14    1.37    1.19    4.67 ^ _13552_/Y (sky130_fd_sc_hd__nor3_4)
                                         _07042_ (net)
                  1.37    0.00    4.67 ^ _13572_/B (sky130_fd_sc_hd__nand2_2)
     5    0.03    0.29    0.30    4.98 v _13572_/Y (sky130_fd_sc_hd__nand2_2)
                                         _07058_ (net)
                  0.29    0.00    4.98 v _13574_/B1 (sky130_fd_sc_hd__o221ai_1)
     1    0.00    0.20    0.24    5.22 ^ _13574_/Y (sky130_fd_sc_hd__o221ai_1)
                                         _01444_ (net)
                  0.20    0.00    5.22 ^ hold2174/A (sky130_fd_sc_hd__dlygate4sd3_1)
     1    0.00    0.05    0.57    5.79 ^ hold2174/X (sky130_fd_sc_hd__dlygate4sd3_1)
                                         net2283 (net)
                  0.05    0.00    5.79 ^ core.CPU_Xreg_value_a4[7][13]$_SDFFE_PP0P_/D (sky130_fd_sc_hd__dfxtp_1)
                                  5.79   data arrival time

                         11.05   11.05   clock clk (rise edge)
                          0.00   11.05   clock source latency
     1    0.10    0.00    0.00   11.05 ^ pll/CLK (avsdpll)
                                         CLK (net)
                  0.00    0.00   11.05 ^ clkbuf_0_CLK/A (sky130_fd_sc_hd__clkbuf_16)
     8    0.23    0.24    0.26   11.31 ^ clkbuf_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_0_CLK (net)
                  0.24    0.00   11.31 ^ clkbuf_3_6_0_CLK/A (sky130_fd_sc_hd__clkbuf_16)
     2    0.04    0.06    0.21   11.52 ^ clkbuf_3_6_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_3_6_0_CLK (net)
                  0.06    0.00   11.52 ^ clkbuf_4_13__f_CLK/A (sky130_fd_sc_hd__clkbuf_16)
    12    0.16    0.18    0.24   11.76 ^ clkbuf_4_13__f_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_4_13__leaf_CLK (net)
                  0.18    0.00   11.76 ^ clkbuf_leaf_90_CLK/A (sky130_fd_sc_hd__clkbuf_16)
    11    0.04    0.06    0.19   11.94 ^ clkbuf_leaf_90_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_leaf_90_CLK (net)
                  0.06    0.00   11.94 ^ core.CPU_Xreg_value_a4[7][13]$_SDFFE_PP0P_/CLK (sky130_fd_sc_hd__dfxtp_1)
                         -0.55   11.39   clock uncertainty
                          0.00   11.39   clock reconvergence pessimism
                         -0.05   11.34   library setup time
                                 11.34   data required time
-----------------------------------------------------------------------------
                                 11.34   data required time
                                 -5.79   data arrival time
-----------------------------------------------------------------------------
                                  5.55   slack (MET)



==========================================================================
cts final report_checks -unconstrained
--------------------------------------------------------------------------
Startpoint: core.CPU_valid_taken_br_a5$_DFF_P_
            (rising edge-triggered flip-flop clocked by clk)
Endpoint: core.CPU_Xreg_value_a4[7][13]$_SDFFE_PP0P_
          (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: max

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                          0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock source latency
     1    0.10    0.00    0.00    0.00 ^ pll/CLK (avsdpll)
                                         CLK (net)
                  0.00    0.00    0.00 ^ clkbuf_0_CLK/A (sky130_fd_sc_hd__clkbuf_16)
     8    0.23    0.24    0.26    0.26 ^ clkbuf_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_0_CLK (net)
                  0.24    0.00    0.26 ^ clkbuf_3_4_0_CLK/A (sky130_fd_sc_hd__clkbuf_16)
     2    0.04    0.07    0.21    0.47 ^ clkbuf_3_4_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_3_4_0_CLK (net)
                  0.07    0.00    0.47 ^ clkbuf_4_8__f_CLK/A (sky130_fd_sc_hd__clkbuf_16)
    11    0.15    0.16    0.23    0.70 ^ clkbuf_4_8__f_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_4_8__leaf_CLK (net)
                  0.16    0.00    0.70 ^ clkbuf_leaf_149_CLK/A (sky130_fd_sc_hd__clkbuf_16)
    15    0.04    0.06    0.18    0.88 ^ clkbuf_leaf_149_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_leaf_149_CLK (net)
                  0.06    0.00    0.88 ^ core.CPU_valid_taken_br_a5$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_1)
     1    0.00    0.03    0.30    1.18 v core.CPU_valid_taken_br_a5$_DFF_P_/Q (sky130_fd_sc_hd__dfxtp_1)
                                         core.CPU_valid_taken_br_a5 (net)
                  0.03    0.00    1.18 v _07913_/A (sky130_fd_sc_hd__or4_4)
    19    0.22    0.35    0.83    2.01 v _07913_/X (sky130_fd_sc_hd__or4_4)
                                         _02930_ (net)
                  0.35    0.01    2.02 v _07915_/A (sky130_fd_sc_hd__clkinv_16)
    43    0.45    0.31    0.36    2.38 ^ _07915_/Y (sky130_fd_sc_hd__clkinv_16)
                                         _02932_ (net)
                  0.31    0.02    2.39 ^ _09981_/C (sky130_fd_sc_hd__nor3_2)
     2    0.02    0.10    0.14    2.53 v _09981_/Y (sky130_fd_sc_hd__nor3_2)
                                         _04371_ (net)
                  0.10    0.00    2.53 v _09982_/B1 (sky130_fd_sc_hd__a21oi_4)
     6    0.10    0.70    0.57    3.10 ^ _09982_/Y (sky130_fd_sc_hd__a21oi_4)
                                         _04372_ (net)
                  0.70    0.00    3.10 ^ _09988_/A2 (sky130_fd_sc_hd__o21ai_4)
    16    0.12    0.33    0.38    3.47 v _09988_/Y (sky130_fd_sc_hd__o21ai_4)
                                         _04378_ (net)
                  0.33    0.00    3.48 v _13552_/A (sky130_fd_sc_hd__nor3_4)
    23    0.14    1.37    1.19    4.67 ^ _13552_/Y (sky130_fd_sc_hd__nor3_4)
                                         _07042_ (net)
                  1.37    0.00    4.67 ^ _13572_/B (sky130_fd_sc_hd__nand2_2)
     5    0.03    0.29    0.30    4.98 v _13572_/Y (sky130_fd_sc_hd__nand2_2)
                                         _07058_ (net)
                  0.29    0.00    4.98 v _13574_/B1 (sky130_fd_sc_hd__o221ai_1)
     1    0.00    0.20    0.24    5.22 ^ _13574_/Y (sky130_fd_sc_hd__o221ai_1)
                                         _01444_ (net)
                  0.20    0.00    5.22 ^ hold2174/A (sky130_fd_sc_hd__dlygate4sd3_1)
     1    0.00    0.05    0.57    5.79 ^ hold2174/X (sky130_fd_sc_hd__dlygate4sd3_1)
                                         net2283 (net)
                  0.05    0.00    5.79 ^ core.CPU_Xreg_value_a4[7][13]$_SDFFE_PP0P_/D (sky130_fd_sc_hd__dfxtp_1)
                                  5.79   data arrival time

                         11.05   11.05   clock clk (rise edge)
                          0.00   11.05   clock source latency
     1    0.10    0.00    0.00   11.05 ^ pll/CLK (avsdpll)
                                         CLK (net)
                  0.00    0.00   11.05 ^ clkbuf_0_CLK/A (sky130_fd_sc_hd__clkbuf_16)
     8    0.23    0.24    0.26   11.31 ^ clkbuf_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_0_CLK (net)
                  0.24    0.00   11.31 ^ clkbuf_3_6_0_CLK/A (sky130_fd_sc_hd__clkbuf_16)
     2    0.04    0.06    0.21   11.52 ^ clkbuf_3_6_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_3_6_0_CLK (net)
                  0.06    0.00   11.52 ^ clkbuf_4_13__f_CLK/A (sky130_fd_sc_hd__clkbuf_16)
    12    0.16    0.18    0.24   11.76 ^ clkbuf_4_13__f_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_4_13__leaf_CLK (net)
                  0.18    0.00   11.76 ^ clkbuf_leaf_90_CLK/A (sky130_fd_sc_hd__clkbuf_16)
    11    0.04    0.06    0.19   11.94 ^ clkbuf_leaf_90_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_leaf_90_CLK (net)
                  0.06    0.00   11.94 ^ core.CPU_Xreg_value_a4[7][13]$_SDFFE_PP0P_/CLK (sky130_fd_sc_hd__dfxtp_1)
                         -0.55   11.39   clock uncertainty
                          0.00   11.39   clock reconvergence pessimism
                         -0.05   11.34   library setup time
                                 11.34   data required time
-----------------------------------------------------------------------------
                                 11.34   data required time
                                 -5.79   data arrival time
-----------------------------------------------------------------------------
                                  5.55   slack (MET)



==========================================================================
cts final report_check_types -max_slew -max_cap -max_fanout -violators
--------------------------------------------------------------------------
max capacitance

Pin                                    Limit     Cap   Slack
------------------------------------------------------------
_13743_/Y                               0.15    0.15   -0.00 (VIOLATED)
_13455_/Y                               0.15    0.15   -0.00 (VIOLATED)


==========================================================================
cts final max_slew_check_slack
--------------------------------------------------------------------------
0.02021305449306965

==========================================================================
cts final max_slew_check_limit
--------------------------------------------------------------------------
1.4951449632644653

==========================================================================
cts final max_slew_check_slack_limit
--------------------------------------------------------------------------
0.0135

==========================================================================
cts final max_fanout_check_slack
--------------------------------------------------------------------------
1.0000000150474662e+30

==========================================================================
cts final max_fanout_check_limit
--------------------------------------------------------------------------
1.0000000150474662e+30

==========================================================================
cts final max_capacitance_check_slack
--------------------------------------------------------------------------
-0.001024815021082759

==========================================================================
cts final max_capacitance_check_limit
--------------------------------------------------------------------------
0.1538189947605133

==========================================================================
cts final max_capacitance_check_slack_limit
--------------------------------------------------------------------------
-0.0067

==========================================================================
cts final max_slew_violation_count
--------------------------------------------------------------------------
max slew violation count 0

==========================================================================
cts final max_fanout_violation_count
--------------------------------------------------------------------------
max fanout violation count 0

==========================================================================
cts final max_cap_violation_count
--------------------------------------------------------------------------
max cap violation count 2

==========================================================================
cts final setup_violation_count
--------------------------------------------------------------------------
setup violation count 0

==========================================================================
cts final hold_violation_count
--------------------------------------------------------------------------
hold violation count 0

==========================================================================
cts final report_checks -path_delay max reg to reg
--------------------------------------------------------------------------
Startpoint: core.CPU_valid_taken_br_a5$_DFF_P_
            (rising edge-triggered flip-flop clocked by clk)
Endpoint: core.CPU_Xreg_value_a4[7][13]$_SDFFE_PP0P_
          (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: max

  Delay    Time   Description
---------------------------------------------------------
   0.00    0.00   clock clk (rise edge)
   0.00    0.00   clock source latency
   0.00    0.00 ^ pll/CLK (avsdpll)
   0.26    0.26 ^ clkbuf_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.21    0.47 ^ clkbuf_3_4_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.23    0.70 ^ clkbuf_4_8__f_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.18    0.88 ^ clkbuf_leaf_149_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.00    0.88 ^ core.CPU_valid_taken_br_a5$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_1)
   0.30    1.18 v core.CPU_valid_taken_br_a5$_DFF_P_/Q (sky130_fd_sc_hd__dfxtp_1)
   0.83    2.01 v _07913_/X (sky130_fd_sc_hd__or4_4)
   0.36    2.38 ^ _07915_/Y (sky130_fd_sc_hd__clkinv_16)
   0.15    2.53 v _09981_/Y (sky130_fd_sc_hd__nor3_2)
   0.57    3.10 ^ _09982_/Y (sky130_fd_sc_hd__a21oi_4)
   0.38    3.47 v _09988_/Y (sky130_fd_sc_hd__o21ai_4)
   1.20    4.67 ^ _13552_/Y (sky130_fd_sc_hd__nor3_4)
   0.31    4.98 v _13572_/Y (sky130_fd_sc_hd__nand2_2)
   0.24    5.22 ^ _13574_/Y (sky130_fd_sc_hd__o221ai_1)
   0.57    5.79 ^ hold2174/X (sky130_fd_sc_hd__dlygate4sd3_1)
   0.00    5.79 ^ core.CPU_Xreg_value_a4[7][13]$_SDFFE_PP0P_/D (sky130_fd_sc_hd__dfxtp_1)
           5.79   data arrival time

  11.05   11.05   clock clk (rise edge)
   0.00   11.05   clock source latency
   0.00   11.05 ^ pll/CLK (avsdpll)
   0.26   11.31 ^ clkbuf_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.21   11.52 ^ clkbuf_3_6_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.24   11.76 ^ clkbuf_4_13__f_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.19   11.94 ^ clkbuf_leaf_90_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.00   11.94 ^ core.CPU_Xreg_value_a4[7][13]$_SDFFE_PP0P_/CLK (sky130_fd_sc_hd__dfxtp_1)
  -0.55   11.39   clock uncertainty
   0.00   11.39   clock reconvergence pessimism
  -0.05   11.34   library setup time
          11.34   data required time
---------------------------------------------------------
          11.34   data required time
          -5.79   data arrival time
---------------------------------------------------------
           5.55   slack (MET)



==========================================================================
cts final report_checks -path_delay min reg to reg
--------------------------------------------------------------------------
Startpoint: core.CPU_Xreg_value_a4[25][15]$_SDFFE_PP0P_
            (rising edge-triggered flip-flop clocked by clk)
Endpoint: core.CPU_src2_value_a3[15]$_DFF_P_
          (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: min

  Delay    Time   Description
---------------------------------------------------------
   0.00    0.00   clock clk (rise edge)
   0.00    0.00   clock source latency
   0.00    0.00 ^ pll/CLK (avsdpll)
   0.26    0.26 ^ clkbuf_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.21    0.47 ^ clkbuf_3_7_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.24    0.71 ^ clkbuf_4_14__f_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.19    0.90 ^ clkbuf_leaf_108_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.00    0.90 ^ core.CPU_Xreg_value_a4[25][15]$_SDFFE_PP0P_/CLK (sky130_fd_sc_hd__dfxtp_1)
   0.32    1.22 ^ core.CPU_Xreg_value_a4[25][15]$_SDFFE_PP0P_/Q (sky130_fd_sc_hd__dfxtp_1)
   0.06    1.28 v _14708_/Y (sky130_fd_sc_hd__a21oi_1)
   0.12    1.40 ^ _14712_/Y (sky130_fd_sc_hd__nand4_1)
   0.18    1.58 v _14729_/Y (sky130_fd_sc_hd__o32ai_4)
   0.16    1.74 ^ _14731_/Y (sky130_fd_sc_hd__o21ai_0)
   0.00    1.74 ^ core.CPU_src2_value_a3[15]$_DFF_P_/D (sky130_fd_sc_hd__dfxtp_2)
           1.74   data arrival time

   0.00    0.00   clock clk (rise edge)
   0.00    0.00   clock source latency
   0.00    0.00 ^ pll/CLK (avsdpll)
   0.26    0.26 ^ clkbuf_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.21    0.47 ^ clkbuf_3_6_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.24    0.71 ^ clkbuf_4_13__f_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.19    0.89 ^ clkbuf_leaf_70_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.00    0.89 ^ core.CPU_src2_value_a3[15]$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_2)
   0.88    1.78   clock uncertainty
   0.00    1.78   clock reconvergence pessimism
  -0.03    1.74   library hold time
           1.74   data required time
---------------------------------------------------------
           1.74   data required time
          -1.74   data arrival time
---------------------------------------------------------
           0.00   slack (MET)



==========================================================================
cts final critical path target clock latency max path
--------------------------------------------------------------------------
0

==========================================================================
cts final critical path target clock latency min path
--------------------------------------------------------------------------
0

==========================================================================
cts final critical path source clock latency min path
--------------------------------------------------------------------------
0

==========================================================================
cts final critical path delay
--------------------------------------------------------------------------
5.7921

==========================================================================
cts final critical path slack
--------------------------------------------------------------------------
5.5454

==========================================================================
cts final slack div critical path delay
--------------------------------------------------------------------------
95.740750

==========================================================================
cts final report_power
--------------------------------------------------------------------------
Group                  Internal  Switching    Leakage      Total
                          Power      Power      Power      Power (Watts)
----------------------------------------------------------------
Sequential             6.95e-03   9.72e-04   1.45e-08   7.92e-03  37.7%
Combinational          2.12e-03   4.52e-03   3.05e-08   6.64e-03  31.6%
Clock                  3.66e-03   2.76e-03   2.96e-09   6.42e-03  30.6%
Macro                  0.00e+00   0.00e+00   0.00e+00   0.00e+00   0.0%
Pad                    0.00e+00   0.00e+00   0.00e+00   0.00e+00   0.0%
----------------------------------------------------------------
Total                  1.27e-02   8.25e-03   4.80e-08   2.10e-02 100.0%
                          60.7%      39.3%       0.0%
```


The reports showed **no setup or hold violations**, which is great!

> **Post-CTS Timing Report Summary:**
> * **WNS (Worst Negative Slack):** 5.55 ns (MET)
> * **TNS (Total Negative Slack):** 0.00 ns (MET)
> * **Setup Violations:** 0
> * **Hold Violations:** 0

### 5. Routing
This is the final layout step. The global router plans the paths for all the nets, and the detailed router draws the actual metal wires to make the connections.
```bash
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk route
```
And the final view of the fully routed chip:
```bash
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk gui_route
```
- After this i have faced a congestion error , basically after routing or during the place left for routing isnt enough or very less so that the routes , or tracks are too close each other causing DRC violations. 

<img width="1438" height="231" alt="route_Error" src="https://github.com/user-attachments/assets/a559dea6-7149-4b4d-849f-39846827bb4a" />

---

##  Part 4: What is SPEF?

The final step of the flow, and a key goal of this week, was to generate the **SPEF (Standard Parasitic Exchange Format)** file.

### Why is SPEF so important?

Before routing, our timing analysis (pre-STA) is just an *estimate*. It uses a "Wire Load Model" to guess the resistance (R) and capacitance (C) of the wires that *will* be drawn.

After routing, we have the *actual, physical wires*. The SPEF file contains the **real, extracted parasitic R and C values** for every single net in the design.

**In short: SPEF = Ground Truth.**

When we run our final Static Timing Analysis (known as **post-route STA**), we feed it this SPEF file. This allows the timer to calculate the *actual* delays through the wires. This is the "signoff" analysis that determines if our chip will *really* work at the target frequency.

Without a SPEF file, you're just guessing. With it, you *know*.

- To get the RC extraction and generate a spef file we can use the following tcl script

```
# Extraction

if { $rcx_rules_file != "" } {
  define_process_corner -ext_model_index 0 X
  extract_parasitics -ext_model_file $rcx_rules_file

  set spef_file [make_result_file ${design}_${platform}.spef]
  write_spef $spef_file

  read_spef $spef_file
} else {
  # Use global routing based parasitics inlieu of rc extraction
  estimate_parasitics -global_routing
}
```


---

## Post-Routing STA analysis of VSDBabySoC Design

### `Key Files`

To perform reliable timing verification of the BabySoC design after routing, we use OpenSTA with a dedicated TCL script, a post-CTS constraints file, and the post-route SPEF file.

The `sta_across_pvt_route.tcl` script automates static timing analysis across multiple process, voltage, and temperature (PVT) corners, while the `vsdbabysoc_post_cts.sdc` file provides the design-specific timing constraints generated after clock tree synthesis, and the `vsdbabysoc.spef` file supplies the post-route parasitic RC data. Together, these files ensure that STA is run under the correct operating conditions with accurate parasitic delays, and that reports such as setup/hold slack, WNS, and TNS are captured for each library corner.

<details> <summary><strong>sta_across_pvt_route.tcl</strong></summary>

```
 set list_of_lib_files(1) "sky130_fd_sc_hd__tt_025C_1v80.lib"
 set list_of_lib_files(2) "sky130_fd_sc_hd__ff_100C_1v65.lib"
 set list_of_lib_files(3) "sky130_fd_sc_hd__ff_100C_1v95.lib"
 set list_of_lib_files(4) "sky130_fd_sc_hd__ff_n40C_1v56.lib"
 set list_of_lib_files(5) "sky130_fd_sc_hd__ff_n40C_1v65.lib"
 set list_of_lib_files(6) "sky130_fd_sc_hd__ff_n40C_1v76.lib"
 set list_of_lib_files(7) "sky130_fd_sc_hd__ss_100C_1v40.lib"
 set list_of_lib_files(8) "sky130_fd_sc_hd__ss_100C_1v60.lib"
 set list_of_lib_files(9) "sky130_fd_sc_hd__ss_n40C_1v28.lib"
 set list_of_lib_files(10) "sky130_fd_sc_hd__ss_n40C_1v35.lib"
 set list_of_lib_files(11) "sky130_fd_sc_hd__ss_n40C_1v40.lib"
 set list_of_lib_files(12) "sky130_fd_sc_hd__ss_n40C_1v44.lib"
 set list_of_lib_files(13) "sky130_fd_sc_hd__ss_n40C_1v76.lib"

 read_liberty /data/VLSI/VSDBabySoC/OpenSTA/examples/timing_libs/avsdpll.lib
 read_liberty /data/VLSI/VSDBabySoC/OpenSTA/examples/timing_libs/avsddac.lib

 for {set i 1} {$i <= [array size list_of_lib_files]} {incr i} {
 read_liberty /data/VLSI/VSDBabySoC/OpenSTA/examples/timing_libs/$list_of_lib_files($i)
 read_verilog /data/OpenROAD-flow-scripts/flow/results/sky130hd/vsdbabysoc/base/5_route.v
 link_design vsdbabysoc
 current_design
 read_sdc /data/VLSI/VSDBabySoC/OpenSTA/examples/BabySoC/vsdbabysoc_post_cts.sdc
 read_spef /data/OpenROAD-flow-scripts/flow/designs/sky130hd/vsdbabysoc/vsdbabysoc.spef
 check_setup -verbose
 report_checks -path_delay min_max -fields {nets cap slew input_pins fanout} -digits {4} > /data/VLSI/VSDBabySoC/OpenSTA/examples/BabySoC/STA_OUTPUT/route/min_max_$list_of_lib_files($i).txt

 exec echo "$list_of_lib_files($i)" >> /data/VLSI/VSDBabySoC/OpenSTA/examples/BabySoC/STA_OUTPUT/route/sta_worst_max_slack.txt
 report_worst_slack -max -digits {4} >> /data/VLSI/VSDBabySoC/OpenSTA/examples/BabySoC/STA_OUTPUT/route/sta_worst_max_slack.txt

 exec echo "$list_of_lib_files($i)" >> /data/VLSI/VSDBabySoC/OpenSTA/examples/BabySoC/STA_OUTPUT/route/sta_worst_min_slack.txt
 report_worst_slack -min -digits {4} >> /data/VLSI/VSDBabySoC/OpenSTA/examples/BabySoC/STA_OUTPUT/route/sta_worst_min_slack.txt

 exec echo "$list_of_lib_files($i)" >> /data/VLSI/VSDBabySoC/OpenSTA/examples/BabySoC/STA_OUTPUT/route/sta_tns.txt
 report_tns -digits {4} >> /data/VLSI/VSDBabySoC/OpenSTA/examples/BabySoC/STA_OUTPUT/route/sta_tns.txt

 exec echo "$list_of_lib_files($i)" >> /data/VLSI/VSDBabySoC/OpenSTA/examples/BabySoC/STA_OUTPUT/route/sta_wns.txt
 report_wns -digits {4} >> /data/VLSI/VSDBabySoC/OpenSTA/examples/BabySoC/STA_OUTPUT/route/sta_wns.txt
 }
```
</details>

This `vsdbabysoc_post_cts.sdc` file is an auto-generated SDC created after clock tree synthesis. It sets the current design to `vsdbabysoc` and defines the basic timing environment. The file specifies a clock named `clk` with an `11 ns` period, driven from the pin `pll/CLK`, and marks it as a propagated clock for STA. Sections for environment and design rules are also included for adding further constraints if needed.

```shell
###############################################################################
# Created by write_sdc
###############################################################################
current_design vsdbabysoc
###############################################################################
# Timing Constraints
###############################################################################
create_clock -name clk -period 11.0000 [get_pins {pll/CLK}]
set_propagated_clock [get_clocks {clk}]
###############################################################################
# Environment
###############################################################################
###############################################################################
# Design Rules
###############################################################################
```

### `Running STA`

To run the post-route STA using Docker, follow these steps to execute the `sta_across_pvt_route.tcl` script. Launch a Docker container with your local directory mounted, run the script inside the container, and it will generate all timing reports such as setup/hold slack, WNS, and TNS in the mounted `/data` folder. Using Docker ensures a consistent and reproducible environment for performing the analysis.

```shell
docker run -it -v $HOME:/data opensta /data/VLSI/VSDBabySoC/OpenSTA/examples/BabySoC/sta_across_pvt_route.tcl
```

![Alt Text](Images/1.jpg)

After running the STA script, you can navigate to the `STA_OUTPUT/route/` directory to see all the generated timing reports. This includes detailed path delay reports for each library corner (`min_max_*.txt`), worst setup and hold slack summaries (`sta_worst_max_slack.txt and sta_worst_min_slack.txt`), as well as total negative slack (sta_tns.txt) and worst negative slack (`sta_wns.txt`). These files provide a complete overview of the BabySoC design’s timing performance after routing.

![Alt Text](Images/2.jpg)

### `Post-routing Results Summary`

Here is a tabulated view of the key timing results generated by the STA script.

![Alt Text](Images/table.jpg)

### 📈 `Comparison Graphs`

Here is a graph showing the comparison of `worst-case hold slack` post-synthesis vs post-routing for the BabySoC design.

![Alt Text](Images/WHS.jpg)

Here is a graph showing the comparison of `worst-case setup slack` post-synthesis vs post-routing for the BabySoC design.

![Alt Text](Images/WSS.jpg)

Here is a graph showing the comparison of `WNS` post-synthesis vs post-routing for the BabySoC design.

![Alt Text](Images/WNS.jpg)

Here is a graph showing the comparison of `TNS` post-synthesis vs post-routing for the BabySoC design.

![Alt Text](Images/TNS.jpg)

### `Key Differences: Post-Synthesis vs Post-Route Timing Analysis`

| Aspect             | Post-Synthesis Analysis                            | Post-Route Analysis                                           |
| ------------------ | -------------------------------------------------- | ------------------------------------------------------------- |
| **Timing Model**   | Wire-load models (fanout/cell-based estimation)    | Extracted parasitics (RC) from routed layout                  |
| **Clock Network**  | Ideal clock, zero skew, no latency                 | Real clock tree with buffer delays, skew, and insertion delay |
| **Interconnect**   | Delay estimated from fanout-based lookup tables    | Delay calculated from actual metal routing and vias           |
| **Accuracy**       | \~70–80% correlation with sign-off                 | \~95–98% correlation with sign-off                            |
| **Critical Paths** | Critical paths may differ due to estimation errors | Matches actual layout critical paths                          |

### `Summary`
- Post-synthesis analysis serves as an **early timing checkpoint**.  
- Post-route analysis represents the **golden reference for timing sign-off**.  
- Transition from estimated to actual physical parameters often reveals:
  - New critical paths revealed
  - Realistic clock tree effects (skew, latency)
  - Interconnect-dominated delays
  - Impact of physical proximity & coupling





































