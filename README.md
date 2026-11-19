# FPGA Bidirectional Digital Stopwatch

A hardware-level **Up/Down Digital Stopwatch** implemented on the **Altera/Intel DE0 FPGA** development board using **Intel Quartus Prime**. The design combines hierarchical Block Diagram Files (`.bdf`), Library of Parameterized Modules (LPM), and **VHDL** components.

## Key Features
- **100 Hz Precision Timing:** Divides the onboard 50 MHz clock (`CLK_50`) down to 10 ms ticks (`100 Hz`) for accurate centisecond tracking.
- **Bidirectional Counting:** Supports both count-up and count-down modes with Terminal Count (`TC`) carry/borrow generation.
- **Hardware Debouncing:** Integrates a custom VHDL key-debouncer (`clean_key.vhd`) configured for a 10 ms bounce window.
- **7-Segment Display Multiplexing & Decoding:** Splits multi-digit values into tens and units (`lpm_divide`) and drives onboard 7-segment displays (`hexsse.vhd` / `decoder_7_led.vhd`).

## System Architecture
- **`STOPPER` (Top-Level Entity):** Integrates clock generation, control logic, counters, edge detection, and display drivers.
- **`SECGEN`:** Clock divider (`modulus 500,000`) converting 50 MHz to 100 Hz pulses (`t_out` and symmetric `sym_out`).
- **`BCD24` / `BCD60` / `BCD100`:** Modular BCD up/down counters with comparator-based boundary detection (`lpm_compare`).
- **`FALL`:** Synchronous falling-edge detector for clean state transitions.
- **`clean_key.vhd` & `hexsse.vhd`:** Custom VHDL modules for switch debouncing and 7-segment display interfacing.

## Hardware & Tools
- **FPGA Board:** Terasic Altera DE0 (Cyclone III)
- **EDA Tool:** Intel Quartus Prime
- **Languages & Design Entry:** VHDL, BDF (Schematic Capture), LPM Megafunctions, Tcl Pin Assignments (`de0_pins.tcl`)

## How to Run
1. Clone this repository and open `BCD60.qpf` (or the top-level project file) in **Intel Quartus Prime**.
2. Run **Assignments > Import Assignments** and select `de0_pins.tcl` to map I/O ports to the DE0 board pins.
3. Compile the design (**Processing > Start Compilation**).
4. Program the FPGA via **Tools > Programmer** using a USB-Blaster connection.
