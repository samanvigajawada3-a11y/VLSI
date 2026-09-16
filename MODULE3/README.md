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
<img width="1103" height="512" alt="Screenshot 2026-09-16 174145" src="https://github.com/user-attachments/assets/587d06ed-cfe8-41e6-8aa8-371b86fc7df2" />

### Result
Constant propagation was explored to see how synthesis tools replace known values and remove unnecessary logic.

## 2. 🔄 Sequential Logic Optimizations
Sequential optimization improves registers and related logic without changing the circuit's behavior. The module introduces techniques including constant propagation, retiming, and state optimization.
<img width="1096" height="576" alt="Screenshot 2026-09-16 174155" src="https://github.com/user-attachments/assets/76202ae2-13e6-420c-83d3-59342292fda8" />


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
<img width="1193" height="393" alt="Screenshot 2026-09-16 174226" src="https://github.com/user-attachments/assets/84ce1611-1457-4bd6-99cf-4566bacf2b1a" />

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
<img width="952" height="242" alt="Screenshot 2026-09-16 174245" src="https://github.com/user-attachments/assets/3b6e2afc-f0c9-491a-a71e-b64db52bdbd4" />


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
<img width="953" height="551" alt="Screenshot 2026-09-16 174255" src="https://github.com/user-attachments/assets/fd9df8d7-5ff7-4e95-9936-148e79dc11f0" />

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
<img width="882" height="756" alt="Screenshot 2026-09-16 174318" src="https://github.com/user-attachments/assets/29161cf1-b514-4e01-b352-71f8dc3a6fc5" />


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
<img width="886" height="797" alt="Screenshot 2026-09-16 174334" src="https://github.com/user-attachments/assets/d8955509-0285-411e-8d15-18e9a309d362" />


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
<img width="895" height="225" alt="Screenshot 2026-09-16 174354" src="https://github.com/user-attachments/assets/ade230c3-74fc-4296-afd9-88111b931bab" />


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
<img width="885" height="753" alt="Screenshot 2026-09-16 174408" src="https://github.com/user-attachments/assets/6ccd8f9d-23c7-4942-90c4-56abba002b13" />



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
<img width="890" height="457" alt="Screenshot 2026-09-16 174429" src="https://github.com/user-attachments/assets/3a3c137d-d155-427d-a477-91ab3161db01" />



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
<img width="892" height="258" alt="Screenshot 2026-09-16 174439" src="https://github.com/user-attachments/assets/f130f223-c151-4ed9-b60b-c2f30e927a9a" />


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
<img width="892" height="258" alt="Screenshot 2026-09-16 174439" src="https://github.com/user-attachments/assets/f50192ac-65c4-408a-a777-4f28d38145b3" />


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
<img width="448" height="241" alt="Screenshot 2026-09-16 174448" src="https://github.com/user-attachments/assets/5425ed3d-52d0-46b8-b32a-dd3fc65172fa" />



## 15. Optimized Counter Circuit
Information
The optimized gate-level implementation contains the flip-flops and logic required for the observed output.
### Commands
```bash
write_verilog -noattr counter_opt_net.v

gvim counter_opt_net.v
```
Output
<img width="887" height="196" alt="Screenshot 2026-09-16 174456" src="https://github.com/user-attachments/assets/a6b0cdb7-bc7a-4700-a524-4aaa776e85f0" />



## 16. Optimized Counter Netlist
Information
The generated netlist provides a gate-level view of the hardware remaining after synthesis optimization.
### Commands
```bash
write_verilog -noattr counter_opt_net.v

gvim counter_opt_net.v
```

Output
<img width="441" height="242" alt="Screenshot 2026-09-16 174503" src="https://github.com/user-attachments/assets/4ee435f4-d923-4570-a0d9-cd8ab0ec19ee" />


#🎯 Overall Result

The optimization experiments were completed using Verilog HDL, Yosys, Icarus Verilog, GTKWave, and the SKY130 standard-cell library. Simulation and synthesis were used to verify both combinational and sequential designs. The results showed that redundant hardware could be removed while preserving the intended functionality.

# 📝 Conclusion

This module provided hands-on practice with logic optimization, RTL simulation, technology mapping, and gate-level synthesis. Constant propagation, logic simplification, and counter optimization were demonstrated through practical experiments. The resulting netlists showed how synthesis removes unnecessary hardware and supports more efficient digital implementations.

## 👤 Author

**Samanvi Gajawada**  
B.Tech – Electronics & Communication Engineering  
Anurag University  
[RTL Workshop Repository](https://github.com/samanvigajawada3-a11y/RTL_Workshop)

