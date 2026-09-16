# 🛠️ Module 2 — Timing Libraries, Synthesis Methods & Flip-Flop RTL

## 🎯 Objectives

This module introduces timing libraries, the SKY130 PDK, synthesis techniques, and practical RTL coding styles for D flip-flops.

The major activities completed in this module include:

- Understanding the SKY130 timing library
- Examining `.lib` files and operating conditions
- Comparing hierarchical and flattened synthesis
- Implementing different D flip-flop coding styles
- Simulating RTL designs using Icarus Verilog
- Viewing waveforms using GTKWave
- Synthesizing RTL designs using Yosys
- Mapping the synthesized design to SKY130 standard cells


## 🔧 Tools and Technologies

- **HDL:** Verilog
- **Simulator:** Icarus Verilog
- **Waveform Viewer:** GTKWave
- **Synthesis Tool:** Yosys
- **PDK:** SKY130


## 📚 Table of Contents

1. [Timing Libraries](#1-timing-libraries)
2. [Hierarchical and Flattened Synthesis](#2-hierarchical-and-flattened-synthesis)
3. [Flip-Flop Coding Styles](#3-flip-flop-coding-styles)
4. [RTL Simulation and Synthesis Flow](#4-rtl-simulation-and-synthesis-flow)
5. [Optimization](#5-optimization)
6. [Overall Result](#6-overall-result)
7. [Conclusion](#7-conclusion)


# 1. 📚 Timing Libraries

## 1.1 SKY130 PDK

The SKY130 PDK provides the technology files and standard-cell information required for designing digital circuits using 130 nm CMOS technology.

For this module, the following timing library was used:

```bash
sky130_fd_sc_hd__tt_025C_1v80.lib
```

## 1.2 🔍 Timing Library Operating Conditions

The timing library name provides information about the process corner, operating temperature, and supply voltage used for cell characterization.

The selected library is:

```bash
sky130_fd_sc_hd__tt_025C_1v80.lib
```

## 1.3 📂 Examining the `.lib` File

A Liberty (`.lib`) file describes the standard cells available in a technology library, including their electrical, timing, and functional characteristics.

It provides important data such as:

- Cell definitions
- Input and output pins
- Timing information
- Power characteristics
- Operating conditions
- Functional behavior

The file was examined to understand how standard-cell data is organized and how synthesis tools use it during cell selection and technology mapping.
<img width="1837" height="847" alt="Screenshot 2026-09-16 165400" src="https://github.com/user-attachments/assets/d7541d72-75da-494e-80f0-df8454d7a081" />

### Result

The Liberty file was successfully examined, and the available standard-cell and characterization information was studied.

# 2. 🏗️ Hierarchical and Flattened Synthesis

Synthesis translates RTL into a gate-level hardware representation. Both hierarchical and flattened approaches were explored to observe how module structure affects the synthesized design.

## 2.1 📁 Hierarchical Synthesis

Hierarchical synthesis retains the original RTL module structure.

The individual modules remain separate, making it easier to identify the relationship between different blocks and debug the synthesized design.
<img width="1858" height="830" alt="Screenshot 2026-09-16 165807" src="https://github.com/user-attachments/assets/4dab1d70-4cc1-4b7b-99a2-2d54167406a9" />


### Result

The design was synthesized while preserving the module hierarchy and the connections between the submodules.

## 2.2 🌐 Flattened Synthesis

Flattened synthesis removes individual module boundaries and combines the complete design into one representation.

This allows the synthesis tool to analyze and optimize logic across different modules instead of treating each module separately.

The following command is used in Yosys to flatten the design:

```bash
flatten
```

<img width="1252" height="642" alt="Screenshot 2026-09-16 170623" src="https://github.com/user-attachments/assets/e6ba9064-1a27-4e79-a4a8-3c90cb65e5f6" />

## 2.3 ⚖️ Comparison of Synthesis Methods

Hierarchical and flattened synthesis differ mainly in how the original RTL module structure is handled during the synthesis process.

- **Hierarchical synthesis** preserves the individual modules and their boundaries.
- **Flattened synthesis** combines the modules into one unified design.
- Hierarchical designs are generally easier to trace and debug.
- Flattened designs provide more scope for optimization across module boundaries.

### Result

Both approaches were studied to understand the effect of module hierarchy on synthesis, optimization, and design representation.

# 3. 🔄 Flip-Flop Coding Styles

Flip-flops are sequential storage elements that hold binary data. This section implements and verifies multiple D flip-flop variants using Verilog HDL.

The following coding styles are covered:

- Asynchronous Reset D Flip-Flop
- Asynchronous Set D Flip-Flop
- Synchronous Reset D Flip-Flop

## 3.1 🔴 Asynchronous Reset D Flip-Flop

An asynchronous reset clears the flip-flop output as soon as reset is asserted; it does not wait for a clock edge. 

### Verilog Code

```verilog
module dff_asyncres (
    input clk,
    input async_reset,
    input d,
    output reg q
);

always @(posedge clk, posedge async_reset)
    if (async_reset)
        q <= 1'b0;
    else
        q <= d;

endmodule
```
### Working
When async_reset is high, the output q is immediately driven to 0.
When the reset is inactive, the input d is captured at the rising edge of the clock.
### Simulation Commands
```bash
iverilog dff_asyncres.v tb_dff_asyncres.v
./a.out
gtkwave tb_dff_asyncres.vcd
```
<img width="1262" height="617" alt="Screenshot 2026-09-16 170640" src="https://github.com/user-attachments/assets/ac400579-220b-41cd-ba2f-fea34cf01718" />


### Result
The asynchronous-reset D flip-flop was successfully simulated, and the waveform was verified using GTKWave.

## 3.2 🟢 Asynchronous Set D Flip-Flop

An asynchronous set drives the flip-flop output to logic `1` immediately when asserted, independently of the clock.

### Verilog Code

```verilog
module dff_async_set (
    input clk,
    input async_set,
    input d,
    output reg q
);

always @(posedge clk, posedge async_set)
    if (async_set)
        q <= 1'b1;
    else
        q <= d;

endmodule
```
### Working
When async_set is high, the output q is immediately set to 1.
When the set signal is inactive, the input d is transferred to q at the rising edge of the clock.
<img width="615" height="567" alt="Screenshot 2026-09-16 170724" src="https://github.com/user-attachments/assets/f4896c21-8bb9-4ff4-bc69-6a8ec5691c9a" />


### Simulation Commands
```bash
iverilog dff_async_set.v tb_dff_async_set.v
./a.out
gtkwave tb_dff_async_set.vcd
```

### Result
The asynchronous-set D flip-flop was successfully simulated, and its expected behavior was confirmed through the generated waveform.

## 3.3 🔵 Synchronous Reset D Flip-Flop

A synchronous reset is checked only at the active clock edge, together with the input data.

### Verilog Code

```verilog
module dff_syncres (
    input clk,
    input sync_reset,
    input d,
    output reg q
);

always @(posedge clk)
    if (sync_reset)
        q <= 1'b0;
    else
        q <= d;

endmodule
```
### Working
At every rising edge of clk:
When sync_reset is high, q is cleared to 0.
When sync_reset is low, the value of d is stored in q.
Unlike asynchronous reset, a change in sync_reset alone does not immediately affect the output.
<img width="943" height="480" alt="Screenshot 2026-09-16 170742" src="https://github.com/user-attachments/assets/2b4f78f8-aabd-4d65-9ffa-c16d386516ff" />


### Simulation Commands
```bash
iverilog dff_syncres.v tb_dff_syncres.v
./a.out
gtkwave tb_dff_syncres.vcd
```

### Result
The synchronous-reset D flip-flop was successfully simulated, and its clock-dependent operation was verified using GTKWave.

# 4. 🧪 RTL Simulation and Synthesis Flow

After RTL coding, each design was functionally verified through simulation and then synthesized into a gate-level representation.

The overall flow followed in this module is:

```text
Verilog RTL
     ↓
Icarus Verilog
     ↓
Simulation
     ↓
GTKWave
     ↓
Yosys Synthesis
     ↓
Gate-Level Design
     ↓
SKY130 Technology Mapping
```

The simulation stage helps verify the functional behavior of the RTL before moving to synthesis.
### Main Steps
Compile the Verilog design and testbench.
Execute the simulation.
Generate the waveform file.
Inspect signal transitions using GTKWave.
Synthesize the verified RTL using Yosys.
Map the synthesized logic to SKY130 standard cells.

## 4.1 🧪 RTL Simulation Using Icarus Verilog

Icarus Verilog was used to compile and simulate the RTL design along with its corresponding testbench.

### Compilation

```bash
iverilog dff_asyncres.v
 tb_dff_asyncres.v
```
## Run The Simulation
```bash
./a.out
```
The simulation generates a VCD file containing the signal activity of the design.
### View the Waveform
```bash
gtkwave tb_dff_asyncres.vcd
```
The waveform was examined to verify the behavior of:
Clock signal
Reset or set signal
Data input
Flip-flop output

### Result
The RTL simulation was completed successfully, and the generated waveform confirmed the expected operation of the flip-flop.

## 4.2 ⚡ RTL Synthesis Using Yosys

Yosys was used to convert the verified Verilog RTL into a synthesized gate-level representation.

### Start Yosys

```bash
yosys
```

### Read the Verilog Design
```bash
read_verilog dff_asyncres.v
```
### Select the Top Module
```bash
hierarchy -top dff_asyncres
```
### Process the RTL
```bash
proc
```

### Optimize the Design
```bash
opt
```
### Technology Mapping
```bash
techmap
opt
```

These steps transform the RTL description into an optimized gate-level structure that can be further mapped to the required standard-cell library.
🖼️ Figure: Yosys synthesis output

### Result
The RTL design was successfully processed and synthesized using Yosys, producing a gate-level representation of the original Verilog design.

## 5️⃣ Optimization Examples

RTL synthesis tools simplify hardware while preserving functionality. This section demonstrates how Yosys optimizes constant multiplication into simpler hardware structures.

### 5.1 `mul2` Optimization

```verilog
module mul2 (
    input [2:0] a,
    output [3:0] y
);

assign y = a * 2;

endmodule
```

The input a is multiplied by 2 and assigned to output y.
```text
yosys
read_verilog mul2.v
prep -top mul2
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_5v.lib
show
write_verilog -noattr mul2_net.v
gvim mul2_net.v
```
<img width="297" height="88" alt="Screenshot 2026-09-16 170804" src="https://github.com/user-attachments/assets/7ab0aaf8-8a45-4a96-ab41-2fa26a054410" />


### Result

The `mul2` design was synthesized successfully. Since multiplication by 2 is equivalent to a one-bit left shift, Yosys reduced the operation to wiring and generated the synthesized Verilog netlist.
## 5.2 `mult8` Optimization
```
module mult8 (
    input [2:0] a,
    output [5:0] y
);

assign y = a * 9;

endmodule
```
The input a is multiplied by 9 and assigned to output y.
```text
yosys
read_verilog mult8.v
prep -top mult8
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_5v.lib
show
write_verilog -noattr mult8_net.v
gvim mult8_net.v
```
<img width="326" height="70" alt="Screenshot 2026-09-16 170813" src="https://github.com/user-attachments/assets/f51d7715-a804-41f3-984c-0cac93d2f0cb" />


### Result

The `mult8` design was synthesized and optimized successfully. The generated netlist shows the hardware representation produced from the original RTL.
## 5.3 Generated Synthesized Netlists
```text
write_verilog -noattr mul2_net.v
write_verilog -noattr mult8_net.v
gvim mul2_net.v
gvim mult8_net.v
```
<img width="940" height="786" alt="Screenshot 2026-09-16 170826" src="https://github.com/user-attachments/assets/8b8b6415-0a33-4641-99b3-d7cc6cc48517" />


### Result

The synthesized Verilog netlists were generated and examined successfully, demonstrating the conversion of RTL into an optimized hardware representation.

## 🏁 Overall Result

- ✅ The SKY130 timing library was explored and its operating conditions understood.
- ✅ Hierarchical and flattened synthesis approaches were studied and compared.
- ✅ Three D flip-flop coding styles (async reset, async set, sync reset) were implemented and verified.
- ✅ RTL designs were simulated using Icarus Verilog and GTKWave.
- ✅ Designs were synthesized and technology-mapped using Yosys with the SKY130 standard-cell library.
- ✅ Constant-multiplication optimizations (`×2`, `×9`) were shown to resolve into pure wiring instead of multiplier hardware.

  ## 📌 Conclusion

Module 2 provided hands-on experience with timing libraries, synthesis methods, flip-flop RTL coding, simulation, waveform analysis, and technology mapping. The exercises demonstrate how RTL is transformed into an optimized gate-level implementation using standard-cell libraries.

---
## 👤 Author

**Amrutha Madapa**  
B.Tech – Electronics & Communication Engineering  
Anurag University  
[RTL Workshop Repository](https://github.com/madapaamrutha-svg/RTL_Workshop)


