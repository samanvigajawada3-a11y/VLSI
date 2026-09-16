 # 🔍 Module-3 Introduction to Combinational And Sequential Optimization
 ## Objectives
 
To learn how digital circuits can be simplified without changing their required functionality.
To explore optimization methods for both combinational and sequential circuits.
To practice RTL synthesis with the Yosys synthesis tool.
To map Verilog RTL designs onto the SKY130 standard-cell library.
To run Verilog simulations using Icarus Verilog.
To inspect and verify circuit behavior through GTKWave waveforms.
To examine the gate-level netlists produced after optimization and synthesis.
## Tools And Technologies
HDL: Verilog
Simulator: Icarus Verilog
Waveform Viewer: GTKWave
Synthesis Tool: Yosys
PDK: SKY130
Operating System: Linux(Ubuntu)
# 📚 Table of Contents

1. Introduction to Logic Optimizations
2. Sequential Logic Optimizations
3. AND Gate Optimization (opt_check)
4. OR Gate Optimization (opt_check2)
5. Three-Input AND Gate Optimization (opt_check3)
6. Verilog Code for D Flip-Flop Constant Propagation
7. Simulation Waveform – dff_const1
8. Simulation Waveform – dff_const2
9. D Flip-Flop Netlist Before Optimization
10. Sequential Logic Optimization Result
11. D Flip-Flop Constraint Simulation
12. Synthesized D Flip-Flop Circuit
13. Counter Optimization
14. Counter Optimization Result
15. Optimized Counter Circuit
16. Optimized Counter Netlist
17. Overall Result
18. Conclusion

## 1.Introduction to Logic Optimizations
Logic optimization simplifies a circuit while preserving its intended behavior. It can reduce hardware area, switching activity, and propagation delay. This module demonstrates combinational and sequential optimization using Yosys together with the SKY130 standard-cell library.
<img width="1277" height="595" alt="Screenshot (69)" src="https://github.com/user-attachments/assets/534b0d39-3317-4dba-80d2-2f2a0190e454" />

### Result
Constant propagation was explored to see how synthesis tools replace known values and remove unnecessary logic.

## 2. 🔄 Sequential Logic Optimizations
Sequential optimization improves registers and related logic without changing the circuit's behavior. The module introduces techniques including constant propagation, retiming, and state optimization.
<img width="1096" height="581" alt="Screenshot (70)" src="https://github.com/user-attachments/assets/cb6ca621-a87f-4a50-b89f-942325ff151f" />


### Result
These techniques were reviewed to understand their effect on circuit efficiency and implementation cost.

## 3. ⚙️ AND Gate Optimization (opt_check)
```Verilog
module opt_check (
    input a,
    input b,
    output y
);

assign y = a & b;

endmodule
```
### Yosys Commands
```bash
yosys
read_verilog opt_check.v
synth -top opt_check
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```
<img width="958" height="930" alt="opt check and gate mod3 start 1st" src="https://github.com/user-attachments/assets/fff7b4a0-edc0-4608-a821-edf0b32ebd47" />

### Result
The two-input AND function was synthesized and mapped to the SKY130 AND2 standard cell.
## 4. ⚙️ OR Gate Optimization (opt_check2)
```Verilog 
module opt_check2 (
    input a,
    input b,
    output y
);

assign y = a | b;

endmodule
```
### Yosys Commands
```bash
yosys
read_verilog opt_check2.v
synth -top opt_check2
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```
<img width="958" height="930" alt="norgate mod3 2nd" src="https://github.com/user-attachments/assets/fe49600c-e152-4be2-8f05-9f0a6bdbdcd3" />


### Result
The two-input OR function was synthesized and mapped to the SKY130 OR2 standard cell.

## 5. ⚙️ Three-Input AND Gate Optimization (opt_check3)
```Verilog
module opt_check3 (
    input a,
    input b,
    input c,
    output y
);

assign y = a & b & c;

endmodule
```
### Yosys Commands
```bash
yosys
read_verilog opt_check3.v
synth -top opt_check3
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```
<img width="958" height="930" alt="3ip and gate 3rd image module3" src="https://github.com/user-attachments/assets/bfac1861-30c3-4f2e-b5b9-0041b9954059" />

### Result
The three-input AND function was synthesized and mapped to the SKY130 AND3 standard cell.

## 6. Verilog Code for D Flip-Flop Constant Propagation
Description
These two D Flip-Flop examples demonstrate sequential constant propagation. Their different reset and next-state values help show how synthesis identifies and simplifies constant behavior.
```verilog
// dff_const1.v
module dff_const1(input clk, input reset, output reg q);
always @(posedge clk, posedge reset)
begin
    if(reset)
        q <= 1'b0;
    else
        q <= 1'b1;
end
endmodule
```
```verilog
// dff_const2.v
module dff_const2(input clk, input reset, output reg q);
always @(posedge clk, posedge reset)
begin
    if(reset)
        q <= 1'b1;
    else
        q <= 1'b1;
end
endmodule
```
### Commands
```bash
vim dff_const1.v
vim dff_const2.v
```
Output
<img width="958" height="930" alt="code 4th image" src="https://github.com/user-attachments/assets/f9222591-7c82-484d-a1a6-489039067849" />


## 7. Simulation Waveform – dff_const1
Description
The waveform illustrates dff_const1 during reset and clock events, showing how its output responds to the applied signals.
### Code
```verilog
module dff_const1(input clk, input reset, output reg q);
always @(posedge clk, posedge reset)
begin
    if(reset)
        q <= 1'b0;
    else
        q <= 1'b1;
end
endmodule
```

### Commands
```bash
iverilog -o dff_const1.out dff_const1.v tb_dff_const1.v
gtkwave tb_dff_const1.vcd
```
Output
<img width="958" height="930" alt="wave 5th" src="https://github.com/user-attachments/assets/54c5497a-2444-4b3b-be37-2906dd7e0ea6" />


## 8. Simulation Waveform – dff_const2
Description
The waveform illustrates dff_const2, whose output remains constant because both branches assign the same value.
### Code
```verilog
module dff_const2(input clk, input reset, output reg q);
always @(posedge clk, posedge reset)
begin
    if(reset)
        q <= 1'b1;
    else
        q <= 1'b1;
end
endmodule
```

### Commands
```bash
iverilog -o dff_const2.out dff_const2.v tb_dff_const2_.v
gtkwave tb_dff_const2_.vcd
```
Output
<img width="958" height="930" alt="6th" src="https://github.com/user-attachments/assets/81fa31f8-9500-41a5-b01c-2d2de2b22f1e" />


## 9. D Flip-Flop Netlist Before Optimization
Description
This section shows the synthesized netlist before sequential optimization is applied.
### Commands
```bash
yosys
read_liberty -lib sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_const1.v
synth -top dff_const1
abc -liberty sky130_fd_sc_hd__tt_025C_1v80.lib
show
```
Output
<img width="958" height="930" alt="dff const1 7th" src="https://github.com/user-attachments/assets/28083af0-94a2-4c36-85f4-853ff4a617db" />



## 10. Sequential Logic Optimization Result
Description
After constant propagation, unnecessary logic is eliminated and the remaining circuit is represented using a simpler implementation.
### Commands
```bash
yosys
read_liberty -lib sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_const2.v
synth -top dff_const2
abc -liberty sky130_fd_sc_hd__tt_025C_1v80.lib
show
```
Output
<img width="958" height="930" alt="seq optimization 8th" src="https://github.com/user-attachments/assets/ec7fe013-f9d5-4ab3-ac20-436c7639a72a" />



## 11. D Flip-Flop Constraint Simulation
Information
This experiment simulates a D Flip-Flop with constant propagation. The waveform is used to check its response during reset and clock transitions.
### Code
```
module dff_const3(input clk, input reset, output reg q);

always @(posedge clk)
begin
    if(reset)
        q <= 1'b0;
    else
        q <= 1'b1;
end

endmodule
```
### Commands
```bash
iverilog -o dff_const3.out dff_const3.v dff_const3_tb.v

gtkwave dff_const3.vcd
```
Output
<img width="958" height="930" alt="dffconst3 9th" src="https://github.com/user-attachments/assets/564760ac-8465-4be7-8a5c-6b452d4f041b" />


## 12. Synthesized D Flip-Flop Circuit
Information
The D Flip-Flop is synthesized with Yosys and mapped to cells from the SKY130 standard-cell library.
### Commands
```bash
yosys

read_verilog dff_const3.v

synth -top dff_const3

show
```
Output
<img width="958" height="930" alt="2ff is there set and reset 10th image" src="https://github.com/user-attachments/assets/fbcd222c-c976-47e5-8ee4-94cc75efc218" />


## 13. Counter Optimization
Information
This experiment demonstrates how synthesis can remove counter logic that does not contribute to the required output.
### Code
```
module counter_opt(input clk, input reset, output q);

reg [2:0] count;

assign q = count[0];

always @(posedge clk, posedge reset)
begin
    if(reset)
        count <= 3'b000;
    else
        count <= count + 1;
end

endmodule
```
### Commands
```bash
yosys

read_verilog counter_opt.v

synth -top counter_opt


show
```


## 14. Counter Optimization Result
Information
After optimization, the synthesized counter keeps only the logic needed to produce the specified output.
### Commands
```bash
yosys

read_verilog counter_opt.v

synth -top counter_opt

show
```
Output
<img width="958" height="930" alt="unused op optimization 11 image" src="https://github.com/user-attachments/assets/53b8fc3f-3a6a-4937-a479-a029e2507987" />



## 15. Optimized Counter Circuit
Information
The optimized gate-level implementation contains the flip-flops and logic required for the observed output.
### Commands
```bash
write_verilog -noattr counter_opt_net.v

gvim counter_opt_net.v
```
Output
<img width="958" height="930" alt="counter dff 3bit but 1flop there 12th image" src="https://github.com/user-attachments/assets/6d05c5ac-b53f-4dd9-87b2-4980300107a1" />



## 16. Optimized Counter Netlist
Information
The generated netlist provides a gate-level view of the hardware remaining after synthesis optimization.
### Commands
```bash
write_verilog -noattr counter_opt_net.v

gvim counter_opt_net.v
```

Output
<img width="958" height="930" alt="opt2 counter 13thimage" src="https://github.com/user-attachments/assets/bfc5cf2c-211c-41fe-b7f9-374725b41489" />


#🎯 Overall Result

The optimization experiments were completed using Verilog HDL, Yosys, Icarus Verilog, GTKWave, and the SKY130 standard-cell library. Simulation and synthesis were used to verify both combinational and sequential designs. The results showed that redundant hardware could be removed while preserving the intended functionality.

# 📝 Conclusion

This module provided hands-on practice with logic optimization, RTL simulation, technology mapping, and gate-level synthesis. Constant propagation, logic simplification, and counter optimization were demonstrated through practical experiments. The resulting netlists showed how synthesis removes unnecessary hardware and supports more efficient digital implementations.

## 👤 Author

**Samanvi Gajawada**  
B.Tech – Electronics & Communication Engineering  
Anurag University  
[RTL Workshop Repository](https://github.com/samanvigajawada3-a11y/RTL_Workshop)

