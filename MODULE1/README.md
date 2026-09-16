# Introduction to Verilog RTL Design & Synthesis

## Overview
Welcome to Day 1 of the RTL Design Workshop. In this session, I learned the basics of Verilog RTL Design, simulation using Icarus Verilog (iverilog), waveform analysis using GTKWave, and logic synthesis using Yosys.



## Table of Contents
1. What is a Simulator, Design, and Testbench?
2. Getting Started with Iverilog
3. Lab: Simulating a 2-to-1 Multiplexer
4. Verilog Code Analysis
5. Introduction to yosys
6. Takeawyas




# 1. What is a Simulator, Design, and Testbench?

## Simulator
A simulator is a software tool used to verify the functionality of a digital circuit before hardware implementation.

## Design
The design is the Verilog code that describes the required hardware functionality.
<img width="983" height="621" alt="Screenshot 2026-09-16 172309" src="https://github.com/user-attachments/assets/753f9859-14c3-4458-a41b-c576d83b8aa3" />




## Testbench
A testbench provides different input combinations to verify whether the design produces the expected outputs.
<img width="1082" height="615" alt="Screenshot 2026-09-16 172326" src="https://github.com/user-attachments/assets/c577f8fe-d2c4-48bb-b6e9-1e1c563275d3" />



# 2. Getting Started with Iverilog

Iverilog is an open-source Verilog simulator.

Simulation Flow:

Design + Testbench → Iverilog → VCD File → GTKWave

<img width="1103" height="632" alt="Screenshot 2026-09-16 172347" src="https://github.com/user-attachments/assets/0aed6317-fe89-41cd-8369-8bd430375859" />


# 3. Lab: Simulating a 2-to-1 Multiplexer

## Step 1: Install Required Tools

```bash
sudo apt install iverilog
sudo apt install gtkwave
```

## Step 2: Compile

```bash
iverilog good_mux.v tb_good_mux.v
```

## Step 3: Run Simulation

```bash
./a.out
```

## Step 4: View Waveform

```bash
gtkwave tb_good_mux.vcd
```

**Image: GTKWave Output**
<img width="1115" height="565" alt="Screenshot 2026-09-16 172403" src="https://github.com/user-attachments/assets/a1eafb72-b758-474a-b9e9-a7868a6e474c" />



# 4. Verilog Code Analysis

## Multiplexer Code

<img width="1055" height="576" alt="Screenshot 2026-09-16 172412" src="https://github.com/user-attachments/assets/da625529-f1be-4a7e-aff7-8d757f504c28" />

   

### Explanation
- Inputs: i0, i1
- Select Line: sel
- Output: y
- If sel = 1, output = i1
- If sel = 0, output = i0

## 5. Introduction to Yosys and Logic Synthesis

### 5.1 What Synthesis Does

Simulation is used to check whether the RTL design behaves as expected. However, simulation alone does not convert the RTL description into hardware. Synthesis is the process of translating the Verilog RTL design into a gate-level representation using standard cells from a target technology library.

Yosys is an open-source RTL synthesis tool used to perform this conversion. In this workshop, Yosys is used along with the SKY130 standard-cell library to transform the Verilog design into a gate-level netlist.

The basic synthesis flow is:

**Verilog RTL Design → Yosys → Gate-Level Netlist**
<img width="987" height="327" alt="Screenshot 2026-09-16 172421" src="https://github.com/user-attachments/assets/5a49018a-7d18-45ec-97db-f9b558c4196b" />


### 5.2 Lab: Synthesizing the Design

The synthesis process was performed using Yosys. First, Yosys was launched from the terminal.

```bash
yosys
```

The required SKY130 Liberty library was then loaded so that Yosys could use the available standard cells during synthesis.
```bash
read_liberty -lib my_lib/lib/sky130_fd_sc_hd__*.lib
```
Next, the Verilog RTL design was read into Yosys.
```bash
read_verilog good_mux.v
```
The synthesis process was then performed by specifying the top-level module.
```bash
synth -top good_mux.v
```
After synthesis, the design was mapped to the SKY130 standard-cell library.
```bash
abc -liberty my_lib/lib/sky130_fd_sc_hd__*.lib
```
The synthesized design was then viewed as a gate-level schematic.
```bash
show
```
<img width="1060" height="290" alt="Screenshot 2026-09-16 172434" src="https://github.com/user-attachments/assets/23a4258b-82e1-42d6-947f-0654f37d30fe" />

This schematic represents the RTL design using standard cells such as flip-flops and logic gates.

### 5.3 Generating the Gate-Level Netlist

After completing synthesis and technology mapping, the synthesized design can be written into a Verilog gate-level netlist.
```bash
write_verilog -noattr
good_mux_netlist.v
```
The generated netlist describes the design using cells from the target technology library. This provides a representation of how the RTL design can be implemented using actual standard cells.

### 5.4 Synthesis Flow

The complete synthesis flow followed in this module is:
RTL Verilog Design → Read Library → Read Verilog → Synthesis → Technology Mapping → Gate-Level Schematic → Gate-Level Netlist
This helped in understanding how a high-level Verilog RTL description is converted into a hardware-oriented gate-level representation.

## 6. Takeaways

Understood the purpose of RTL synthesis.
Learned how Yosys is used for open-source RTL synthesis.
Loaded the SKY130 standard-cell library for synthesis.
Read the Verilog RTL design into Yosys.
Performed synthesis using the selected top-level module.
Mapped the synthesized design to SKY130 standard cells.
Viewed the resulting gate-level schematic.
Generated a gate-level Verilog netlist.
Understood the overall flow from RTL design to gate-level implementation.

## 👤 Author

**Amrutha Madapa**  
B.Tech – Electronics & Communication Engineering  
Anurag University  
[RTL Workshop Repository](https://github.com/madapaamrutha-svg/RTL_Workshop)
