# 🚀 Module 4 – Blocking vs Non-Blocking Assignments and Synthesis-Simulation Mismatch

## 📖 Introduction

This module focuses on procedural assignments in Verilog HDL and their effect on RTL behavior. It covers multiplexer design, blocking-assignment execution, and situations where simulation results may differ from synthesized hardware. Each experiment follows an RTL-to-netlist workflow using Icarus Verilog, GTKWave, Yosys, and the SKY130 standard-cell library.

---

# 🎯 Objectives

- To understand the difference between blocking (`=`) and non-blocking (`<=`) assignments in Verilog HDL.
- To study the causes of synthesis-simulation mismatch.
- To design and simulate multiplexer circuits using Verilog.
- To analyze the behavior of blocking assignments through RTL simulation.
- To verify circuit functionality using Icarus Verilog and GTKWave.
- To synthesize RTL designs using the Yosys synthesis tool.
- To map synthesized circuits to the SKY130 standard-cell library.
- To compare RTL simulation results with synthesized hardware.

---

# 🛠️ Tools and Technologies Used

| Tool / Technology | Purpose |
|-------------------|---------|
| Verilog HDL | RTL Design |
| Icarus Verilog | Verilog Compiler and Simulator |
| GTKWave | Waveform Viewer |
| Yosys | RTL Synthesis Tool |
| SKY130 Standard Cell Library | Technology Mapping |
| Linux Terminal | Command Execution |
| gVim | Viewing and Editing Verilog Files |

---

# 📚 Table of Contents


1. RTL Simulation of a 2×1 Multiplexer
2. Technology Mapping of the Multiplexer
3. Functional Verification Using Simulation Waveform
4. Analysis of an Incorrect Multiplexer Design
5. Verification of Bad Multiplexer Behavior
6. Simulation of Blocking Assignment Behavior
7. Synthesis of Blocking Assignment Circuit
8. Blocking Assignment Using Past Value
9. Overall Result
10. Conclusion

---

# 1️⃣ RTL Simulation of a 2×1 Multiplexer

## 📖 Overview

A 2×1 multiplexer was implemented using the Verilog ternary operator to understand combinational logic design. The RTL design was simulated using Icarus Verilog, and the output waveform was verified using GTKWave. The experiment demonstrates how the output changes based on the select signal and validates the functional correctness of the multiplexer before synthesis.



### ⚙️ Simulation Commands
```bash
iverilog -o mux ternary_operator_mux.v tb_ternary_operator_mux.v

gtkwave ternary_operator_mux.vcd
```


### 📷 Output Waveform
<img width="1080" height="612" alt="Screenshot 2026-09-16 180534" src="https://github.com/user-attachments/assets/0b3ec466-ebaf-4972-8001-358bf2f41799" />




### ✅ Observation

The waveform verified that the output follows the selected input correctly.

---

# 2️⃣ Technology Mapping of the Multiplexer

## 📖  Overview

The multiplexer design was synthesized using Yosys and mapped to the SKY130 standard-cell library. During synthesis, the RTL description was optimized and converted into a technology-specific multiplexer cell, reducing hardware complexity while preserving functionality.

### ⚙️ Synthesis Commands
```bash
yosys

read_verilog mux_generate.v

synth -top mux_generate

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

show
```


### 📷 Synthesized Circuit
<img width="1075" height="527" alt="Screenshot 2026-09-16 180545" src="https://github.com/user-attachments/assets/2661da5b-9daf-4ccc-b3bc-960da85087b4" />




### ✅ Observation

Yosys successfully converted the RTL multiplexer into a technology-mapped SKY130 implementation.

---

# 3️⃣ Functional Verification Using Simulation Waveform

## 📖  Overview

The simulation waveform verifies that the multiplexer output correctly follows the selected input. Different input combinations were applied to validate the functionality of the design under various conditions before synthesis.

### ⚙️ Simulation Commands
```bash
iverilog -o mux mux_generate.v tb_mux_generate.v

gtkwave mux_generate.vcd
```


### 📷 Output Waveform
<img width="1071" height="597" alt="Screenshot 2026-09-16 180555" src="https://github.com/user-attachments/assets/f17a72e6-8b54-4641-8cdf-2a5506d9cacf" />




### ✅ Observation

All tested input combinations produced the expected multiplexer output.

---

# 4️⃣ Analysis of an Incorrect Multiplexer Design

## 📖  Overview

This experiment demonstrates an improperly coded multiplexer that leads to synthesis-simulation mismatch. Incomplete assignments inside the always block may infer latches during synthesis, resulting in hardware behavior different from RTL simulation.



### ⚙️ Simulation Commands
```bash
iverilog -o bad_mux bad_mux.v tb_bad_mux.v

gtkwave bad_mux.vcd
```


### 📷 Output
<img width="1067" height="617" alt="Screenshot 2026-09-16 180605" src="https://github.com/user-attachments/assets/635a9669-15e3-4050-b91f-f8d8c418cf7a" />




### ✅ Observation

The incomplete RTL description caused the output to retain its previous value in some cases.

---

# 5️⃣ Verification of Bad Multiplexer Behavior

## 📖 Overview

The waveform illustrates the behavior of the incorrectly implemented multiplexer. Missing assignments cause previous output values to be retained, resulting in latch inference and mismatch between simulation and synthesized hardware.

### ⚙️ Simulation Commands
```bash
iverilog -o bad_mux bad_mux.v tb_bad_mux.v

gtkwave bad_mux.vcd
```


### 📷 Output Waveform
<img width="1065" height="587" alt="Screenshot 2026-09-16 180616" src="https://github.com/user-attachments/assets/8a20689c-7433-4af7-a3f2-e0832e2b0fd8" />



### ✅ Observation

The waveform highlighted how incomplete assignments can create latch-like behavior and lead to a synthesis-simulation mismatch.

---

# 6️⃣ Simulation of Blocking Assignment Behavior

## 📖  Overview

Blocking assignments execute statements sequentially within an always block. This experiment illustrates their behavior during simulation and explains why they are mainly recommended for combinational logic. Incorrect usage in sequential circuits may produce unexpected results.



### ⚙️ Simulation Commands
```bash
iverilog -o blocking blocking_caveat.v tb_blocking_caveat.v

gtkwave blocking_caveat.vcd
```

### 📷 Output Waveform
<img width="1072" height="598" alt="Screenshot 2026-09-16 180624" src="https://github.com/user-attachments/assets/0eff217f-e490-4d87-90d4-7db275cc531b" />




### ✅ Observation

The waveform showed that blocking statements are evaluated in order and update variables immediately.

---

# 7️⃣ Synthesis of Blocking Assignment Circuit

## 📖Overview

The blocking assignment circuit was synthesized using Yosys to compare RTL simulation with the generated hardware. The synthesized circuit illustrates how the Verilog description is interpreted and mapped to the SKY130 standard-cell library.

### ⚙️ Synthesis Commands
```bash
yosys

read_verilog blocking_caveat.v

synth -top blocking_caveat

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

show
```


### 📷 Technology-Mapped Circuit
![Uploading Screenshot 2026-09-16 180633.png…]()



### ✅ Observation

The synthesized result showed how Yosys interpreted the blocking-assignment RTL and mapped it into standard cells.

---
## 8️⃣ Blocking Assignment Using Past Value

This experiment demonstrates how blocking assignments (=) use the updated value immediately within the same procedural block. The simulation illustrates how the output depends on the sequence of statements and the previously assigned values. This helps in understanding the behavior of blocking assignments and why they can sometimes lead to unexpected results if used in sequential logic. Therefore, blocking assignments are generally recommended for combinational logic, while non-blocking assignments (<=) are preferred for sequential circuits.
