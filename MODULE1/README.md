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
<img width="880" height="556" alt="6d5a017b-25ac-44ad-a186-e54d5fedb3fc" src="https://github.com/user-attachments/assets/022fdf42-a030-4252-826c-c7e68716bc98" />




## Testbench
A testbench provides different input combinations to verify whether the design produces the expected outputs.
<img width="976" height="556" alt="5a3dc0d0-1386-440f-bc1d-d6aabb3a5c63" src="https://github.com/user-attachments/assets/34b40dc2-47be-496a-8a5f-a8b135d372df" />



# 2. Getting Started with Iverilog

Iverilog is an open-source Verilog simulator.

Simulation Flow:

Design + Testbench → Iverilog → VCD File → GTKWave

<img width="1080" height="640" alt="8063fea1-a7b5-44a6-bacd-73d3d8463bd5" src="https://github.com/user-attachments/assets/b6870a1b-47f9-4177-8bd5-7be55b810925" />


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
<img width="1920" height="940" alt="mux wave" src="https://github.com/user-attachments/assets/0bd68e56-52d8-40ad-b0bb-f821ed3408aa" />



# 4. Verilog Code Analysis

## Multiplexer Code

<img width="1080" height="616" alt="b027b3d7-04f8-4027-bc3b-aa204b61ed2d" src="https://github.com/user-attachments/assets/6f7f7336-5aeb-4c19-a707-8df1903213c2" />

   

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
<img width="897" height="324" alt="Screenshot (60)" src="https://github.com/user-attachments/assets/d2a2442b-91f8-475a-b8ea-8c5626fc9350" />


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
<img width="958" height="292" alt="yosysblockdiagram" src="https://github.com/user-attachments/assets/4712fea6-798b-4a72-b867-94e2df2e32a0" />

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
