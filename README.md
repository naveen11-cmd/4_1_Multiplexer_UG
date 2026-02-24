# SIMULATION AND IMPLEMENTATION OF 4:1 MULTIPLEXER

## AIM
To design and simulate a 4:1 Multiplexer (MUX) using Verilog HDL in four different modeling styles—Gate-Level, Data Flow, Behavioral, and Structural—and to verify its functionality through a testbench using the Vivado 2023.1 simulation environment. The experiment aims to understand how different abstraction levels in Verilog can be used to describe the same digital logic circuit and analyze their performance.

## APPARATUS REQUIRED
- **Vivado 2023.1**

## Procedure

1. Open **Vivado 2023.1**.  
2. Create a **New RTL Project** and give a name (e.g., `Mux4_to_1`).  
3. Add/create your Verilog files and testbench.  
4. Select an FPGA part (e.g., `xc7a35ticsg324-1L`).  
5. Run **Synthesis** to check for errors.  
6. Run **Simulation** → **Run Behavioral Simulation**.  
7. Observe the waveforms of inputs and outputs.  
8. Adjust simulation time if needed (e.g., 1000ns).  
9. Save the project and take screenshots of results.  
10. Close simulation.  

---

## Logic Diagram
![image](https://github.com/user-attachments/assets/d4ab4bc3-12b0-44dc-8edb-9d586d8ba856)

---

## Truth Table
![image](https://github.com/user-attachments/assets/c850506c-3f6e-4d6b-8574-939a914b2a5f)

---

## Verilog Code

### 4:1 MUX Gate-Level Implementation
```verilog
`timescale 1ns / 1ps
module mux_4_1(i,s,y);
input [3:0]i;
input [1:0]s;
output y;
wire [4:1]w;
and g1(w[1],i[0],~s[1],~s[0]);
and g2(w[2],i[1],~s[1],s[0]);
and g3(w[3],i[2],s[1],~s[0]);
and g4(w[4],i[3],s[1],s[0]);
or g5(y,w[1],w[2],w[3],w[4]);
endmodule


```
### 4:1 MUX Gate-Level Implementation- Testbench
```verilog
module mux_4_1_tb;
reg [3:0]i;
reg [1:0]s;
wire y;
mux_4_1 uut(i,s,y);
initial
begin
i=4'b1001;
s=2'b00;
#10;
$display("selection is %b %b, output is %b",s[1],s[0],y);
s=2'b01;
#10;
$display("selection is %b %b, output is %b",s[1],s[0],y);
s=2'b10;
#10;
$display("selection is %b %b, output is %b",s[1],s[0],y);
s=2'b11;
#10;
$display("selection is %b %b, output is %b",s[1],s[0],y);
$finish;
end
endmodule
```
## Simulated Output Gate Level Modelling

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/772dc979-633c-45bf-8680-89c6313d7368" />


---
### 4:1 MUX Data flow Modelling
```verilog

`timescale 1ns / 1ps
module mux_4_1(i, s, y);
input [3:0]i;
input [1:0]s;
output y;
assign y = (~s[1] & ~s[0] & i[0]) |
(~s[1] & s[0] & i[1]) |
( s[1] & ~s[0] & i[2]) |
( s[1] & s[0] & i[3]);
endmodule


```
### 4:1 MUX Data flow Modelling- Testbench
```verilog
module mux_4_1_tb;
reg [3:0]i;
reg [1:0]s;
wire y;
mux_4_1 uut(i,s,y);
initial
begin
i=4'b0110;
s=2'b00;
#20;
$display("Selection is %b %b, Output is %b",s[1],s[0],y);
s=2'b01;
#20;
$display("Selection is %b %b, Output is %b",s[1],s[0],y);
s=2'b10;
#20;
$display("Selection is %b %b, Output is %b",s[1],s[0],y);
s=2'b11;
#20;
$display("Selection is %b %b, Output is %b",s[1],s[0],y);
$finish;
end
endmodule


```
## Simulated Output Dataflow Modelling

<img width="1267" height="786" alt="41_dataflow_modelling" src="https://github.com/user-attachments/assets/bea59e49-2625-4f97-820c-9ad4f0679256" />


---
### 4:1 MUX Behavioral Implementation
```verilog
module mux4_to_1_behavioral (
timescale 1ns / 1ps
module mux_4_1(i,s,y);
input [3:0]i;
input [1:0]s;
output reg y;
always @(i,s)
begin
if (s[1]==0&&s[0]==0)
y=i[0];
else if (s[1]==0&&s[0]==1)
y=i[1];
else if (s[1]==1&&s[0]==0)
y=i[2];
else
y=i[3];
end
endmodule
```
### 4:1 MUX Behavioral Modelling- Testbench
```verilog
module mux_4_1_tb;
reg [3:0]i;
reg [1:0]s;
wire y;
mux_4_1 uut(i,s,y);
initial
begin
i=4'b1001;
s=2'b00;
#20;
$display("Selection is %b %b, Output is %b",s[1],s[0],y);
s=2'b01;
#20;
$display("Selection is %b %b, Output is %b",s[1],s[0],y);
s=2'b10;
#20;
$display("Selection is %b %b, Output is %b",s[1],s[0],y);
s=2'b11;
#20;
$display("Selection is %b %b, Output is %b",s[1],s[0],y);
$finish;
end
endmodule

```
## Simulated Output Behavioral Modelling

<img width="1262" height="788" alt="41_bahavioural_modelling" src="https://github.com/user-attachments/assets/893d76e0-8604-455d-bba3-6fa9f388d7ad" />



### 4:1 MUX Structural Implementation




```verilog
module mux2to1(Y, I0, I1, S);
  input I0, I1, S;
  output Y;
  wire w1, w2, w3;
  not (w1, S);
  and (w2, I0, w1);
  and (w3, I1, S);
  or  (Y, w2, w3);
endmodule


module mux_41(Y, I, S);
  output Y;
  input [3:0] I;
  input [1:0] S;
  wire w1, w2;
  mux2to1 m1(w1, I[0], I[1], S[0]);
  mux2to1 m2(w2, I[2], I[3], S[0]);
  mux2to1 m3(Y, w1, w2, S[1]);
endmodule
```
### Testbench Implementation
```verilog
module mux_4_1_tb;
reg [3:0]i;
reg [1:0]s;
wire y;
mux_4_1 uut(i,s,y);
initial
begin
i=4'b1010;
s=2'b00;
#20;
$display("Selection is %b %b, Output is %b",s[1],s[0],y);
s=2'b01;
#20;
$display("Selection is %b %b, Output is %b",s[1],s[0],y);
s=2'b10;
#20;
$display("Selection is %b %b, Output is %b",s[1],s[0],y);
s=2'b11;
#20;
$display("Selection is %b %b, Output is %b",s[1],s[0],y);
$finish;
end
endmodule
```
## Simulated Output Structural Modelling

<img width="1262" height="788" alt="41_structural_modelling" src="https://github.com/user-attachments/assets/3e23ea91-89cf-419e-a973-98408722890d" />


---
### CONCLUSION

In this experiment, a 4:1 Multiplexer was successfully designed and simulated using Verilog HDL across four different modeling styles: Gate-Level, Data Flow, Behavioral, and Structural.The simulation results verified the correct functionality of the MUX, with all implementations producing identical outputs for the given input conditions.
