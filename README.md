# 4_1_Multiplexer
# EXP NO: 1.A SIMULATION AND IMPLEMENTATION OF 4:1 MULTIPLEXER
# AIM


To design and simulate a 4:1 Multiplexer (MUX) using Verilog HDL in four different modeling styles—Gate-Level, Data Flow, Behavioral, 
and Structural—and to verify its functionality through a testbench using the Vivado 2023.1 simulation environment. The experiment aims
to understand how different abstraction levels in Verilog can be used to describe the same digital logic circuit
and analyze their performance.

# APPARATUS REQUIRED
Vivado 2023.1


# Procedure

1.Open Vivado 2023.1.

2.Create a New RTL Project and give a name (e.g., Mux4_to_1).

3.Add/create your Verilog files and testbench.

4.Select an FPGA part (e.g., xc7a35ticsg324-1L).

5.Run Synthesis to check for errors.

6.Run Simulation → Run Behavioral Simulation.

7.Observe the waveforms of inputs and outputs.

8.Adjust simulation time if needed (e.g., 1000ns).

9.Save the project and take screenshots of results.

10.Close simulation.

# Logic Diagram

<img width="614" height="424" alt="Screenshot 2026-02-11 195225" src="https://github.com/user-attachments/assets/03cabe3f-914c-4163-bea7-2ba5257ed5a7" />


# Truthtable 

<img width="496" height="376" alt="Screenshot 2026-02-11 194234" src="https://github.com/user-attachments/assets/3530b20d-cc75-480c-bc14-d10b21a36376" />

# Verilog Code
# 4:1 MUX Gate-Level Implementation
```
module mux_4_1_gl(A,B,C,D,S1,S2,Y);
    input A,B,C,D,S1,S2;
    output Y;
    wire p,q,w1,w2,w3,w4;
    not (p,S1);
    not (q,S2);
    and (w1,A,p,q);
    and (w2,B,p,S2);
    and (w3,C,S1,q);
    and (w4,D,S1,S2);
    or (Y,w1,w2,w3,w4);    
endmodule
```
# 4:1 MUX Gate-Level Implementation- Testbench
````
`timescale 1ns / 1ps

module mux_4_1_tb;

    reg a,b,c,d;
    reg s1,s2;
    wire y;

    mux_4_1_gl dut (a,b,c,d,s1,s2,y);

    initial 
    begin
        a=1'b0;
        b=1'b0;
        c=1'b1;
        d=1'b0;
        s2=1'b1;
        s1=1'b0;
    #100
        a=1'b1;
        b=1'b0;
        c=1'b0;
        d=1'b1;
        s2=1'b1;
        s1=1'b1;
    end

endmodule
````
# Simulated Output Gate Level Modelling

<img width="1920" height="1200" alt="Gate_lvl_mux_4_1" src="https://github.com/user-attachments/assets/68bfd6b4-080a-43ce-9fac-2a18ef951950" />

---
# 4:1 MUX Data flow Modelling
```
module mux_4_1_df(a,b,c,d,s1,s0,y);
    input a,b,c,d,s1,s0;
    output y;
    
    assign y =   (s1 == 0 && s0 == 0) ? a:
                 (s1 == 0 && s0 == 1) ? b:
                 (s1 == 1 && s0 == 0) ? c: 
                 (s1 == 1 && s0 == 1) ? d: 1'b0;
                                         
endmodule
```
# 4:1 MUX Data flow Modelling- Testbench
```
`timescale 1ns / 1ps

module mux_4_1_tb;

    reg a,b,c,d;
    reg s1,s0;
    wire y;

    mux_4_1_df dut (a,b,c,d,s1,s0,y);

    initial 
    begin
        a=1'b0;
        b=1'b1;
        c=1'b1;
        d=1'b0;
        s1=1'b0;
        s0=1'b1;
    #100
        a=1'b1;
        b=1'b0;
        c=1'b0;
        d=1'b1;
        s1=1'b1;
        s0=1'b1;
    end

endmodule

```
# Simulated Output Dataflow Modelling

<img width="1920" height="1200" alt="Data_flow_mux_4_1" src="https://github.com/user-attachments/assets/0e87c778-6782-4fcf-93d5-5008d478428d" />

---
# 4:1 MUX Behavioral Implementation
```
module mux_4_1_bhv(a,b,c,d,s,y);
    input a,b,c,d;
    input [1:0] s;
    output reg y;
    always @(*) 
    begin
        case (s)
            2'b00: y = a;
            2'b01: y = b;
            2'b10: y = c;
            2'b11: y = d;
            default: y = 1'b0;
        endcase
    end
endmodule
```
# 4:1 MUX Behavioral Modelling- Testbench
```
`timescale 1ns / 1ps

module mux_4_1_tb;

    reg a,b,c,d;
    reg [1:0] s;
    wire y;

    mux_4_1_bhv dut (a,b,c,d,s,y);

    initial 
    begin
        a=1'b0;
        b=1'b1;
        c=1'b1;
        d=1'b0;
        s=2'b01;
    #100
        a=1'b1;
        b=1'b0;
        c=1'b0;
        d=1'b1;
        s=2'b11;
    end

endmodule
```
# Simulated Output Behavioral Modelling

<img width="1920" height="1200" alt="Behavioural_mux_4_1" src="https://github.com/user-attachments/assets/701b6d5a-983d-43e1-881c-0c0545f208ec" />

---
# 4:1 MUX Structural Implementation
```
module mux_4_1_str(a,b,c,d,s1,s0,y);
    input a,b,c,d,s1,s0;
    output y;
    wire y1,y2;
    
    mux_2_1 m1 (a,b,s0,y1);
    mux_2_1 m2 (c,d,s0,y2);
    mux_2_1 m3 (y1,y2,s1,y);
    
endmodule    

module mux_2_1(a,b,s,y);
    input a,b,s;
    output y;    
    assign y = (s) ? b : a;
endmodule

```
# Testbench Implementation
```
`timescale 1ns / 1ps

module mux_4_1_tb;

    reg a,b,c,d,s1,s0;
    wire y;

    mux_4_1_str dut (a,b,c,d,s1,s0,y);

    initial 
    begin
        a=1'b0;
        b=1'b1;
        c=1'b1;
        d=1'b0;
        s1=1'b0;
        s0=1'b1;
    #100
        a=1'b1;
        b=1'b0;
        c=1'b0;
        d=1'b1;
        s1=1'b1;
        s0=1'b0;
    #100 $stop;
    end

endmodule

```
# Simulated Output Structural Modelling

<img width="1920" height="1200" alt="Structural_mux_4_1" src="https://github.com/user-attachments/assets/7e288986-1570-496b-b8a1-465f5add4f88" />

---
# CONCLUSION
In this experiment, a 4:1 Multiplexer was successfully designed and simulated using Verilog HDL across four different modeling styles: Gate-Level, Data Flow, Behavioral, and Structural.The simulation results verified the correct functionality of the MUX, with all implementations producing identical outputs for the given input conditions.
