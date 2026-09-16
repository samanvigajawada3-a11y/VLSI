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
<img width="958" height="930" alt="incomp latvh 1st" src="https://github.com/user-attachments/assets/20e96880-2907-4f39-ac25-3de694b1efa2" />


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
 <img width="958" height="930" alt="incomp_if 2nd image" src="https://github.com/user-attachments/assets/92f02d1b-1498-40c0-8ac5-1f5bf52c356a" />


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
<img width="958" height="930" alt="incomp if 2 image 3rd" src="https://github.com/user-attachments/assets/b7b7dfa2-4bdd-4b63-ae06-643ee97a6c1f" />


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

 <img width="958" height="930" alt="incomp if2 4th image" src="https://github.com/user-attachments/assets/b61d9a3a-fd44-471e-8c32-7337126517a8" />


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
<img width="958" height="930" alt="incomp case diag 6th image" src="https://github.com/user-attachments/assets/4232315c-a6ec-48bf-9f9e-114cd7840424" />


### Result

The synthesized netlist shows that a latch is inferred because the case statement does not define outputs for all possible input conditions. This indicates incomplete combinational logic and may lead to synthesis-simulation mismatches.
###  Simulation Waveform
The GTKWave simulation verifies the behavior of the incomplete case implementation. The output changes according to the defined case conditions, while retaining its previous value for undefined input combinations.
<img width="958" height="930" alt="incomp case wave 5th" src="https://github.com/user-attachments/assets/96de6c3e-3e37-4de5-acfd-7e3786cb3228" />


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
<img width="958" height="930" alt="comp case 8th image" src="https://github.com/user-attachments/assets/650df85d-feed-4259-8ed3-7fc6acc9e108" />

### Simulation Waveform
<img width="958" height="930" alt="comp wave 7th" src="https://github.com/user-attachments/assets/42b04923-7e92-49a0-9130-65abc99de451" />


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
<img width="958" height="930" alt="partial case 9th" src="https://github.com/user-attachments/assets/361ee625-29d0-4ac3-8180-b4f5e2eca1b5" />


### Result

The synthesis report indicates latch inference due to partially specified output assignments. Missing assignments force the synthesizer to preserve previous output values, resulting in sequential storage elements.

###  Simulation Waveform
The GTKWave simulation confirms the synthesized behavior. The waveform shows that the output retains its previous value whenever an assignment is missing, matching the inferred latch behavior.
<img width="958" height="930" alt="partial waveform 10th" src="https://github.com/user-attachments/assets/3b30c7bc-4e66-4d75-bcf5-b04d2926cf51" />


### Result

The RTL simulation confirms the behavior of the partial case statement. The waveform shows that the output holds its previous state whenever the case statement does not assign a new value, demonstrating latch inference.

## 8 – Bad Case Assignmrnt
### Overview
This experiment verifies the RTL functionality of the implemented design using GTKWave. Different input combinations are applied through the testbench to ensure the circuit behaves as expected before synthesis.
<img width="958" height="930" alt="11th image" src="https://github.com/user-attachments/assets/fa936eb8-adfb-4bfa-a8a5-a29689682b1b" />

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
<img width="958" height="930" alt="mux 12th" src="https://github.com/user-attachments/assets/ff95c75d-87d0-4f29-8751-25d15fadfdc8" />


### Observation
The output follows the selected input as the select signal changes. The waveform confirms correct switching between all input lines.
### Result
The multiplexer functions correctly, and the simulation validates proper data selection based on the select inputs.

## 10 – Demultiplexer (DEMUX) Verification
### Overview
This experiment demonstrates the behavior of a demultiplexer, where a single input is routed to one of several output lines based on the select signal.

<img width="958" height="930" alt="demux wave 13" src="https://github.com/user-attachments/assets/e866c0ae-c957-47ea-88ee-416ed30bff1c" />

### Observation
The waveform shows that only the selected output receives the input signal, while all remaining outputs stay inactive.
### Result
The demultiplexer operates correctly, successfully routing the input to the selected output according to the select lines.

## 11 – Ripple Carry Adder
### Overview
This experiment verifies the functionality of an 8-bit Ripple Carry Adder. The design performs binary addition by propagating the carry from one full adder stage to the next.
<img width="958" height="930" alt="ripple carry adder 14th image" src="https://github.com/user-attachments/assets/f0c2228d-548b-48dd-b195-88b50290579b" />


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

