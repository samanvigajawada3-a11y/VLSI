# 🚀 Module 5 – Optimization in Synthesis

## 📖 Overview

This module explores how synthesis tools interpret Verilog RTL and simplify hardware implementations. Through a series of combinational-circuit experiments, the designs were simulated, synthesized, and examined to understand latch inference, complete coding styles, logic selection, and arithmetic hardware.

---

# 📑 Table of Contents

- 1 – Incomplete IF Statement
- 2 – RTL Schematic of Incomplete IF Statement
- 3 – RTL Simulation of Incomplete IF Statement
- 4 – RTL Schematic of Incomplete IF-ELSE Statement 
- 5 – Incomplete Case Statement
- 6–  Complete Case Statement
- 7 – Partial Case Assignment
- 8 – Bad Case Assignment
- 9 – Multiplexer 
- 10 – Demultiplexer
- 11 – Ripple Carry Adder
- 12- Overall Results
- 13- Conclusion 

---

# 🎯 Objectives

- Understand how synthesis tools optimize RTL designs.
- Implement and simulate combinational circuits using Verilog.
- Perform RTL synthesis and technology mapping using Yosys.
- Observe how coding styles affect the synthesized hardware.
- Analyze the RTL schematics and synthesized structures.
- Verify circuit behavior through GTKWave waveforms.
- Relate RTL descriptions to efficient hardware implementations.

---

# 🛠️ Tools & Technologies Used

| Tool | Purpose |
|------|---------|
| Verilog HDL | Hardware Description Language |
| Icarus Verilog | Compilation & Simulation |
| GTKWave | Waveform Analysis |
| Yosys | RTL Synthesis & Optimization |
| SKY130 Standard Cell Library | Technology Mapping |
| Ubuntu Linux | Development Environment |

---
## 📌 1. Incomplete IF Statement
📖 Overview
This experiment demonstrates the behavior of an incomplete IF statement in Verilog HDL. An incomplete IF statement assigns the output only when a specific condition is satisfied, leaving the remaining conditions without any assignment. During RTL simulation, the output retains its previous value whenever the condition is false. During synthesis, this behavior results in latch inference, as the synthesis tool inserts a latch to preserve the previous output value. This experiment highlights the importance of assigning outputs under all possible conditions in combinational logic to avoid unintended hardware.

## ⚙️ Simulation Commands
```bash

iverilog -o incom_if incom_if.v incom_if_tb.v

gtkwave incom_if.vcd
```
 Output
<img width="1065" height="646" alt="Screenshot 2026-09-16 181354" src="https://github.com/user-attachments/assets/5c02e8c4-599e-40ed-9d7b-1f1d0491d6de" />


## 📊 Observation
The output changes only when the select signal (sel) is high.
When sel is low, the output retains its previous value because no assignment is made.
This behavior indicates memory retention, which results in latch inference during synthesis.
The experiment demonstrates why complete conditional assignments are essential for combinational logic design.
## ✅ Result
The RTL simulation successfully demonstrated the behavior of an incomplete IF statement. The waveform confirmed that the output retains its previous value for unspecified conditions, illustrating latch inference caused by incomplete conditional assignments.

## 📌  2 – RTL Schematic of Incomplete IF Statement
### 📖  Overview
This experiment focuses on the synthesis of the incomplete IF statement using the Yosys synthesis tool. During synthesis, the RTL description is analyzed to determine the required hardware implementation. Since the output is not assigned for every possible condition, the synthesis tool infers a latch to preserve the previous output value. The generated RTL schematic visually demonstrates how incomplete combinational logic results in unintended storage elements.

### ⚙️ Synthesis Commands
```bash
yosys

read_verilog incom_if.v

synth -top incom_if

show
```
 Output
<img width="1071" height="592" alt="Screenshot 2026-09-16 181406" src="https://github.com/user-attachments/assets/606c3b14-93a4-49cf-8179-6b67ddec37c4" />


### 📊 Observation
The RTL schematic contains a latch inferred by the synthesis tool.
The latch stores the previous output whenever the IF condition is not satisfied.
The synthesized circuit confirms that incomplete assignments introduce unintended memory elements.
This experiment emphasizes the importance of assigning outputs for all input conditions in combinational logic.
### ✅ Result
The synthesized RTL schematic successfully demonstrates latch inference caused by an incomplete IF statement. The generated hardware includes a latch, confirming that incomplete conditional assignments lead to unintended storage elements during synthesis.

## 📌 3 – RTL Simulation of Incomplete IF Statement
### 📖 Overview
This experiment analyzes the RTL simulation waveform of an incomplete IF statement. The objective is to observe how the output behaves when all input conditions are not explicitly assigned. During simulation, whenever the specified condition is false, the output retains its previous value instead of changing immediately. This behavior indicates memory retention and is one of the primary reasons for latch inference during synthesis. The waveform generated using GTKWave helps verify the functional behavior of the RTL design.

### ⚙️ Simulation Commands
```bash
iverilog -o incom_if incom_if.v incom_if_tb.v

gtkwave incom_if.vcd
```
🖼️ Experimental Output
<img width="1083" height="598" alt="Screenshot 2026-09-16 181417" src="https://github.com/user-attachments/assets/f5eddfa8-2f1b-4d11-b29f-2edf4d1977fe" />


### 📊 Observation
The waveform shows that the output changes only when the select signal is active.
When the condition is false, the output retains its previous value.
This behavior confirms that the output is not assigned under all possible conditions.
The waveform clearly demonstrates the need for complete conditional assignments in combinational logic.
### ✅ Result
The RTL simulation successfully demonstrated the behavior of an incomplete IF statement. The waveform confirms that the output retains its previous value whenever no assignment is made, illustrating latch-like behavior that leads to latch inference during synthesis.

## 📌 4 – RTL Schematic of Incomplete IF-ELSE Statement
### 📖 Overview
This experiment demonstrates the synthesis of an incomplete IF-ELSE statement using the Yosys synthesis tool. The RTL description is synthesized to observe how incomplete conditional assignments affect the generated hardware. Since the output is not assigned for every possible condition, the synthesis tool automatically infers a latch to preserve the previous output value. The generated RTL schematic clearly illustrates the impact of incomplete coding on hardware implementation.

### ⚙️ Synthesis Commands
```bash
yosys

read_verilog incom_if2.v

synth -top incom_if2

show
```

 Output

<img width="1067" height="403" alt="Screenshot 2026-09-16 181428" src="https://github.com/user-attachments/assets/c1a4c00a-7321-4c40-9d29-5343bd6ed192" />


### 📊 Observation
The RTL schematic represents the synthesized hardware for the IF-ELSE design.
The synthesis process analyzes the conditional assignments and generates the corresponding logic.
The circuit implementation can be verified by comparing the RTL schematic with the Verilog description.
Proper conditional assignments help ensure predictable and optimized hardware generation.
### ✅ Result
The RTL schematic was successfully generated for the IF-ELSE design using Yosys. The synthesized circuit accurately represents the Verilog description and demonstrates the importance of correct conditional coding in digital circuit design.

## 5 – Incomplete Case Statement

### Overview
This experiment demonstrates the synthesis result of an **incomplete case statement**. Since not all possible input combinations are covered, the synthesis tool infers additional hardware (such as latches) to preserve the previous output value.
### Code
```verilog
module incomp_case (input i0, input i1, input i2, input [1:0] sel, output reg y);
always @(*)
begin
    case(sel)
        2'b00 : y = i0;
        2'b01 : y = i1;
    endcase
end
endmodule
```

### Synthesized Netlist (Incomplete Case)
The synthesized circuit generated by Yosys shows the hardware implementation of the incomplete case statement. The presence of a latch indicates that the output is not assigned for every possible case, causing memory elements to be inferred during synthesis.
<img width="1073" height="542" alt="Screenshot 2026-09-16 181436" src="https://github.com/user-attachments/assets/9935b686-55ef-4b82-871f-b85d70bfa641" />


### Result

The synthesized netlist shows that a latch is inferred because the case statement does not define outputs for all possible input conditions. This indicates incomplete combinational logic and may lead to synthesis-simulation mismatches.
###  Simulation Waveform
The GTKWave simulation verifies the behavior of the incomplete case implementation. The output changes according to the defined case conditions, while retaining its previous value for undefined input combinations.
<img width="1063" height="638" alt="Screenshot 2026-09-16 181446" src="https://github.com/user-attachments/assets/75830a25-1007-43e1-8d3b-c7a3eee81dd8" />


### Result

The RTL simulation waveform verifies the behavior of the incomplete case statement. The output retains its previous value for undefined conditions, confirming latch behavior during simulation.

## 6– Complete Case Statement

### Overview
This experiment demonstrates the synthesis of a **complete case statement**, where all possible input conditions are defined. This allows the synthesis tool to generate purely combinational logic without inferring latches.
### Code
```verilog
module comp_case (input i0, input i1, input i2, input [1:0] sel, output reg y);
always @(*)
begin
    case(sel)
        2'b00 : y = i0;
        2'b01 : y = i1;
        default : y = i2;
    endcase
end
endmodule
```

### Synthesized Netlist (Complete Case)
The Yosys synthesized schematic shows a combinational implementation of the complete case statement. Since every input condition is covered, the design does not require storage elements.
<img width="1075" height="512" alt="Screenshot 2026-09-16 181456" src="https://github.com/user-attachments/assets/f2d2aa10-e0a2-4f18-b200-68b69d798004" />

### Simulation Waveform
<img width="1067" height="667" alt="Screenshot 2026-09-16 181504" src="https://github.com/user-attachments/assets/74ea353e-21e7-479d-bc76-039df88beb3c" />


### Result

The synthesized design contains only combinational logic without any inferred latches. Since all input combinations are covered, the circuit is optimized for reliable synthesis and predictable hardware implementation.

## 7 – Partial Case Assignment

### Overview
This experiment demonstrates the effect of **partial assignments** inside a case statement. When the output is not assigned in every branch, synthesis infers latch-based storage to maintain the previous output value.
### Code
```verilog
module partial_case_assign (input i0, input i1, input i2, input [1:0] sel, output reg y, output reg x);
always @(*)
begin
    case(sel)
        2'b00 : begin
            y = i0;
            x = i2;
        end
        2'b01 : y = i1;
        default : begin
            x = i1;
        end
    endcase
end
endmodule
```
###  Synthesized Netlist (Partial Assignment)
The synthesized circuit generated by Yosys includes latch elements because the output assignment is incomplete. This illustrates how partial assignments can unintentionally introduce sequential behavior into the design.
<img width="1062" height="712" alt="Screenshot 2026-09-16 181518" src="https://github.com/user-attachments/assets/3d149086-a5ca-4415-a9ed-663e032e0d22" />


### Result

The synthesis report indicates latch inference due to partially specified output assignments. Missing assignments force the synthesizer to preserve previous output values, resulting in sequential storage elements.

###  Simulation Waveform
The GTKWave simulation confirms the synthesized behavior. The waveform shows that the output retains its previous value whenever an assignment is missing, matching the inferred latch behavior.
<img width="1061" height="616" alt="Screenshot 2026-09-16 181526" src="https://github.com/user-attachments/assets/df212eef-9a8e-436e-a250-fd6d0fc9fbd7" />


### Result

The RTL simulation confirms the behavior of the partial case statement. The waveform shows that the output holds its previous state whenever the case statement does not assign a new value, demonstrating latch inference.

## 8 – Bad Case Assignmrnt
### Overview
This experiment verifies the RTL functionality of the implemented design using GTKWave. Different input combinations are applied through the testbench to ensure the circuit behaves as expected before synthesis.
<img width="1057" height="598" alt="Screenshot 2026-09-16 181535" src="https://github.com/user-attachments/assets/318b1473-4d7a-4be5-859c-e09d25cfd62e" />

### code
```verilog
module bad_case (input i0, input i1, input i2, input i3, input [1:0] sel, output reg y);
always @(*)
begin
    case(sel)
        2'b00: y = i0;
        2'b01: y = i1;
        2'b10: y = i2;
        2'b11: y = i3;
        //2'b11: y = i3;
    endcase
end
endmodule
```

### Observation
The waveform shows that the output changes correctly for every valid input combination. No unexpected behavior is observed during RTL simulation.
### Result
The RTL simulation successfully verifies the functional correctness of the design. The observed waveform matches the expected behavior.

##  9 – Multiplexer (MUX) Verification
### Overview
This experiment demonstrates the operation of a multiplexer. The select lines determine which input is connected to the output.
<img width="1060" height="642" alt="Screenshot 2026-09-16 181544" src="https://github.com/user-attachments/assets/02049800-67f9-4707-a052-9442153ef6b3" />


### Observation
The output follows the selected input as the select signal changes. The waveform confirms correct switching between all input lines.
### Result
The multiplexer functions correctly, and the simulation validates proper data selection based on the select inputs.

## 10 – Demultiplexer (DEMUX) Verification
### Overview
This experiment demonstrates the behavior of a demultiplexer, where a single input is routed to one of several output lines based on the select signal.

<img width="1061" height="690" alt="Screenshot 2026-09-16 181552" src="https://github.com/user-attachments/assets/4fed7165-1d3f-491d-a0d7-2ed87df9fdad" />

### Observation
The waveform shows that only the selected output receives the input signal, while all remaining outputs stay inactive.
### Result
The demultiplexer operates correctly, successfully routing the input to the selected output according to the select lines.

## 11 – Ripple Carry Adder
### Overview
This experiment verifies the functionality of an 8-bit Ripple Carry Adder. The design performs binary addition by propagating the carry from one full adder stage to the next.
<img width="1060" height="612" alt="Screenshot 2026-09-16 181601" src="https://github.com/user-attachments/assets/4258f5f0-ea58-417c-b24e-3f47bfc4ec14" />


### Observation
The waveform confirms that the sum and carry outputs are generated correctly for different input values. Carry propagation is observed across the adder stages.
### Result
The Ripple Carry Adder successfully performs binary addition, and the simulation confirms accurate sum and carry generation for all tested cases.

# 📊 Overall Results

All Verilog designs were successfully simulated and synthesized. Functional verification was performed using GTKWave, while RTL schematics generated by Yosys confirmed that synthesis optimizations such as constant propagation, logic simplification, resource sharing, multiplexer optimization, and efficient arithmetic implementation were correctly applied. The optimized circuits maintained the expected functionality while reducing unnecessary hardware resources.

---

# ✅ Conclusion

Module 5 provided practical experience in synthesis optimization using Verilog and Yosys. Different optimization techniques were explored through multiple design examples, demonstrating how synthesis tools transform RTL descriptions into optimized hardware implementations. The experiments improved understanding of efficient digital design, RTL synthesis, and technology mapping for ASIC design using the SKY130 standard-cell library.

## 👤 Author

**Samanvi Gajawada**  
B.Tech – Electronics & Communication Engineering  
Anurag University  
[RTL Workshop Repository](https://github.com/samanvigajawada3-a11y)

