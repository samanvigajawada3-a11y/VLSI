# 🛠️ Module 2 — Timing Libraries, Synthesis Methods & Flip-Flop RTL

## 🎯 Objectives

This module focuses on understanding timing libraries, the SKY130 PDK, different synthesis approaches, and RTL coding styles for D flip-flops.

The main activities covered in this module are:

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

The Liberty (`.lib`) file contains detailed information about the standard cells available in the selected technology library.

It provides important data such as:

- Cell definitions
- Input and output pins
- Timing information
- Power characteristics
- Operating conditions
- Functional behavior

The file was inspected to understand how standard-cell information is represented and used during the synthesis process.
<img width="1080" height="486" alt="image" src="https://github.com/user-attachments/assets/a34ea2d9-9890-4f50-952e-75499a7fc326" />

### Result

The Liberty file was successfully examined, and the available standard-cell and characterization information was studied.

# 2. 🏗️ Hierarchical and Flattened Synthesis

Synthesis converts the RTL description into a gate-level hardware representation. In this module, both hierarchical and flattened synthesis approaches were studied to understand how the design structure changes during synthesis.

## 2.1 📁 Hierarchical Synthesis

Hierarchical synthesis keeps the original module structure of the RTL design.

The individual modules remain separate, making it easier to identify the relationship between different blocks and debug the synthesized design.
<img width="1080" height="484" alt="image" src="https://github.com/user-attachments/assets/923e03ee-6776-4b6e-81bb-ebcfd6ef52df" />


### Result

The design was synthesized while preserving the module hierarchy and the connections between the submodules.

## 2.2 🌐 Flattened Synthesis

Flattened synthesis removes the individual module boundaries and combines the complete RTL design into a single representation.

This allows the synthesis tool to analyze and optimize logic across different modules instead of treating each module separately.

The following command is used in Yosys to flatten the design:

```bash
flatten
```

<img width="1080" height="555" alt="image" src="https://github.com/user-attachments/assets/d13fdd2d-bbd1-46ae-90e0-09d41e918a5a" />

## 2.3 ⚖️ Comparison of Synthesis Methods

Hierarchical and flattened synthesis differ mainly in how the original RTL module structure is handled during the synthesis process.

- **Hierarchical synthesis** preserves the individual modules and their boundaries.
- **Flattened synthesis** combines the modules into one unified design.
- Hierarchical designs are generally easier to trace and debug.
- Flattened designs provide more scope for optimization across module boundaries.

### Result

Both approaches were studied to understand the effect of module hierarchy on synthesis, optimization, and design representation.

# 3. 🔄 Flip-Flop Coding Styles

Flip-flops are sequential logic elements used to store binary data. In this section, different D flip-flop implementations are developed and verified using Verilog HDL.

The following coding styles are covered:

- Asynchronous Reset D Flip-Flop
- Asynchronous Set D Flip-Flop
- Synchronous Reset D Flip-Flop

## 3.1 🔴 Asynchronous Reset D Flip-Flop

An asynchronous reset allows the flip-flop output to be cleared immediately when the reset signal is activated, without waiting for a clock transition.

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
<img width="1080" height="539" alt="image" src="https://github.com/user-attachments/assets/e15a4aae-1eac-466f-b6da-cb512a3b9cae" />


### Result
The asynchronous-reset D flip-flop was successfully simulated, and the waveform was verified using GTKWave.

## 3.2 🟢 Asynchronous Set D Flip-Flop

An asynchronous set forces the flip-flop output to logic `1` immediately when the set signal is asserted, independent of the clock.

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
<img width="1080" height="1005" alt="image" src="https://github.com/user-attachments/assets/3ebde68c-1884-4319-b607-c0c8f35b9259" />

### Simulation Commands
```bash
iverilog dff_async_set.v tb_dff_async_set.v
./a.out
gtkwave tb_dff_async_set.vcd
```

### Result
The asynchronous-set D flip-flop was successfully simulated, and its expected behavior was confirmed through the generated waveform.

## 3.3 🔵 Synchronous Reset D Flip-Flop

A synchronous reset affects the flip-flop only when the active clock edge occurs. The reset signal is therefore evaluated along with the input data at the clock edge.

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
<img width="1080" height="550" alt="image" src="https://github.com/user-attachments/assets/4c645bf3-f57e-41f6-9d22-f6e25de01210" />

### Simulation Commands
```bash
iverilog dff_syncres.v tb_dff_syncres.v
./a.out
gtkwave tb_dff_syncres.vcd
```

### Result
The synchronous-reset D flip-flop was successfully simulated, and its clock-dependent operation was verified using GTKWave.

# 4. 🧪 RTL Simulation and Synthesis Flow

After completing the RTL coding, the designs were verified through simulation and then synthesized to obtain their gate-level representation.

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

## 5️⃣ Interesting Optimization

RTL synthesis tools optimize a design while preserving its required functionality. This section shows how Yosys simplifies constant multiplication into efficient hardware.

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
<img width="322" height="97" alt="image" src="https://github.com/user-attachments/assets/09cf1f88-24bb-42ba-b20d-60e603f79fff" />

### Result: The mul2 design was successfully synthesized. Yosys optimized the multiplication into pure wiring (a left shift) and generated the corresponding synthesized Verilog netlist.
## 5.2 mult8 Optimization
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
<img width="355" height="74" alt="image" src="https://github.com/user-attachments/assets/49b685f8-18fe-41e8-8222-f5363ae7031a" />

### Result: 
The mult8 design was successfully synthesized and optimized. The generated netlist shows the optimized hardware representation obtained from the original RTL code.
## 5.3 Generated Synthesized Netlists
```text
write_verilog -noattr mul2_net.v
write_verilog -noattr mult8_net.v
gvim mul2_net.v
gvim mult8_net.v
```
<img width="1054" height="893" alt="image" src="https://github.com/user-attachments/assets/be4425b0-1f15-48f4-b490-d95f4ef2c3e9" />

### Result:
The synthesized Verilog netlists were successfully generated and examined, showing how the original RTL code was converted into an optimized hardware representation.

## 🏁 Overall Result

- ✅ The SKY130 timing library was explored and its operating conditions understood.
- ✅ Hierarchical and flattened synthesis approaches were studied and compared.
- ✅ Three D flip-flop coding styles (async reset, async set, sync reset) were implemented and verified.
- ✅ RTL designs were simulated using Icarus Verilog and GTKWave.
- ✅ Designs were synthesized and technology-mapped using Yosys with the SKY130 standard-cell library.
- ✅ Constant-multiplication optimizations (`×2`, `×9`) were shown to resolve into pure wiring instead of multiplier hardware.

  ## 📌 Conclusion

Module 2 provided practical experience with timing libraries, synthesis techniques, flip-flop coding styles, RTL simulation, waveform analysis, and technology mapping — building a clear understanding of how an RTL design is converted into an optimized gate-level implementation using standard-cell libraries.

---
## 👤 Author

**Amrutha Madapa**  
B.Tech – Electronics & Communication Engineering  
Anurag University  
[RTL Workshop Repository](https://github.com/madapaamrutha-svg/RTL_Workshop)


