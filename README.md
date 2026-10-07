# Low-Power 8×8 SRAM Using Sleep Transistors and Stacked SRAM Cell

A transistor-level design and simulation of a **low-power 8×8 CMOS SRAM memory array** using a **stacked SRAM cell** and **sleep-transistor-based power gating**.

The design was implemented and evaluated using **Cadence Virtuoso and Spectre**. The work focuses on reducing power consumption while maintaining reliable SRAM read, write, and hold operation.

## Project Overview

Static Random Access Memory (SRAM) is widely used in processors, embedded systems, cache memories, and other VLSI circuits where fast and reliable data storage is required.

This project combines two low-power techniques:

- **Transistor stacking** to reduce subthreshold leakage.
- **Sleep transistors** to disconnect the SRAM array from the power rails during standby.

The complete 8×8 memory system contains **64 SRAM cells**, a **3-to-8 row decoder**, precharge circuits, sense amplifier, write driver, word lines, bit lines, and a sleep-transistor network.

## Key Features

- 8×8 SRAM array
- 64 memory cells
- Conventional 6T SRAM baseline
- Stacked 6T SRAM cell
- Sleep-transistor-based power gating
- 3-to-8 row decoder
- Precharge circuit
- Sense amplifier
- Write driver
- Cadence Virtuoso schematic design
- Spectre transient/DC simulation
- Read, write, and hold verification
- Power, leakage, access-time, and SNM evaluation

## Design Flow

```text
          Address Inputs
                │
                ▼
          ┌────────────┐
          │ 3-to-8     │
          │  Decoder   │
          └─────┬──────┘
                │
                ▼
       ┌───────────────────┐
       │    8×8 SRAM Array │
       │                   │
       │  64 SRAM Cells    │
       └───────────────────┘
          ▲      ▲      ▲
          │      │      │
      Precharge  │  Sleep Control
                 │
        ┌────────┴────────┐
        │                 │
   Sense Amplifier    Write Driver
        │                 │
        └────── Data ─────┘
```

## 1. 3-to-8 Decoder

The 3-to-8 decoder generates eight mutually exclusive word-line signals from three address inputs.

![3-to-8 Decoder Schematic](images/decoder_schematic.png)

### Decoder Transient Response

The transient simulation verifies one-of-eight output selection.

![3-to-8 Decoder Transient Response](images/decoder_transient.png)

## 2. Precharge Circuit

The precharge circuit initializes BL and BLB before a read operation. This provides balanced initial conditions and helps improve read sensing.

![Precharge Circuit](images/precharge_circuit.png)

### Precharge Transient Response

![Precharge Transient Response](images/precharge_transient.png)

## 3. Sense Amplifier

The sense amplifier detects the small differential voltage between BL and BLB and converts it into a full logic-level output.

![Sense Amplifier](images/sense_amplifier.png)

### Sense Amplifier Transient Response

![Sense Amplifier Transient Response](images/sense_amplifier_transient.png)

## 4. Write Driver

The write driver forces the required logic value onto the bit lines during a write operation.

![Write Driver](images/write_driver.png)

### Write Driver Transient Response

![Write Driver Transient Response](images/write_driver_transient.png)

## 5. Conventional 6T SRAM Cell

A conventional 6T SRAM cell is used as the baseline design. It consists of two cross-coupled CMOS inverters and two access transistors.

![Conventional 6T SRAM Cell](images/conventional_6t_sram.png)

The baseline cell is evaluated for:

- Read operation
- Write operation
- Hold operation
- Power consumption
- Delay
- Static Noise Margin (SNM)

## 6. Stacked 6T SRAM Cell

The conventional SRAM cell is modified using transistor stacking. Additional series-connected transistors are introduced in the pull-up and pull-down paths.

![Stacked 6T SRAM Cell](images/stacked_6t_sram.png)

The stacking effect increases the effective resistance and suppresses subthreshold leakage during standby operation.

### Stacked SRAM Transient Response

![Stacked 6T SRAM Transient Response](images/stacked_6t_transient.png)

The stacked cell is verified for:

- Write operation
- Read operation
- Hold operation

## 7. Sleep Transistors

High-threshold-voltage sleep transistors are used between the SRAM array and the power/ground network.

- **Active mode:** sleep transistors remain ON.
- **Standby mode:** sleep transistors turn OFF to reduce leakage.

This provides power-gating capability for standby operation.

## 8. 8×8 SRAM Array

After validating the SRAM cell, 64 cells are arranged in an 8×8 memory array.

The array includes:

- 64 stacked SRAM cells
- 3-to-8 row decoder
- Word lines
- Bit lines
- Precharge circuits
- Sense amplifier
- Write driver
- Sleep transistor network

![8×8 SRAM Array](images/sram_8x8_array.png)

## 9. 8×8 SRAM Testbench

The Cadence Virtuoso testbench applies the bit-line, word-line, and sleep-control signals and observes the storage nodes.

![8×8 SRAM Testbench](images/sram_8x8_testbench.png)

### 8×8 SRAM Transient Response

The transient simulation verifies the write and storage behavior of the selected memory cell.

![8×8 SRAM Transient Response](images/sram_8x8_transient.png)

## Performance Results

The paper reports the following comparison between the conventional 6T SRAM and the proposed SRAM:

| Metric | Conventional 6T | Proposed SRAM |
|---|---:|---:|
| Power | 18.22 µW | **2.9 µW** |
| Leakage | 26.5 nW | **24.9 nW** |
| Read Time | 1.20 ns | 1.34 ns |
| Write Time | 1.10 ns | 1.25 ns |
| SNM | 235 mV | **248 mV** |

The reported results show a significant reduction in power consumption and improved SNM, with a small increase in read/write access time.

## Tools Used

- **Cadence Virtuoso**
- **Cadence Spectre**
- CMOS transistor-level simulation
- Transient analysis
- DC analysis

## Verification

The following blocks were functionally verified:

| Module | Verified Function | Result |
|---|---|---|
| CMOS Inverter | Logic inversion | Pass |
| 4-input NAND | NAND operation | Pass |
| 3-to-8 Decoder | Word-line decoding | Pass |
| Precharge Circuit | Bit-line precharging | Pass |
| Sense Amplifier | Read-data sensing | Pass |
| Write Driver | Data write operation | Pass |
| 6T SRAM Cell | Read/Write/Hold | Pass |
| Stacked 6T SRAM Cell | Read/Write/Hold | Pass |
| 8×8 SRAM Array | Complete memory operation | Pass |

## Future Work

The reported future work includes:

- Extend the memory array to **16×16 and 32×32**.
- Further optimize leakage power.
- Improve access time.
- Optimize Static Noise Margin.
- Perform physical layout implementation.
- Perform DRC and LVS.
- Perform parasitic extraction.
- Perform post-layout simulation.

## Repository Structure

```text
low-power-8x8-sram/
│
├── README.md
│
└── images/
    ├── decoder_schematic.png
    ├── decoder_transient.png
    ├── precharge_circuit.png
    ├── precharge_transient.png
    ├── sense_amplifier.png
    ├── sense_amplifier_transient.png
    ├── write_driver.png
    ├── write_driver_transient.png
    ├── conventional_6t_sram.png
    ├── stacked_6t_sram.png
    ├── stacked_6t_transient.png
    ├── sram_8x8_array.png
    ├── sram_8x8_testbench.png
    └── sram_8x8_transient.png
```

## Reference

**Design of LOW POWER 8×8 S-RAM Memory using Sleep Transistors and Stacked SRAM Cell**

Yuvaraj Dhayal D, Vetriveeran Rajamani, Poongundran Selvaprabu, and Antony Xavier Glittas X.

---

### Note

The numerical performance values in this README are taken from the project paper. The paper reports the proposed SRAM power as **2.9 µW**, leakage as **24.9 nW**, read time as **1.34 ns**, write time as **1.25 ns**, and SNM as **248 mV**.
