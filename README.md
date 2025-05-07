![Screenshot 2025-05-07 210530](https://github.com/user-attachments/assets/8c5cc514-53bc-4dd5-940c-3c1abcb821a8)# 4Bit-Up-Down-Asynchronous-Reset-Counter-Synthesis

## Aim:

Synthesize 4Bit-Up-Down-Asynchronous-Reset-Counter design using Constraints and analyse reports, Timing, area and Power.

## Tool Required:

![Screenshot 2025-05-07 210041](https://github.com/user-attachments/assets/4f6b9b8d-bca5-4ace-b627-9e230fdcc6d0)

Functional Simulation: Incisive Simulator (ncvlog, ncelab, ncsim)

Synthesis: Genus

### Step 1: Getting Started

Synthesis requires three files as follows,

◦ Liberty Files (.lib)

![Screenshot 2025-04-29 110045](https://github.com/user-attachments/assets/39a7d456-2d9b-45b9-93c4-5a4f16cf1518)

![Screenshot 2025-04-29 110729](https://github.com/user-attachments/assets/7a26b116-0ad6-4a2b-aebe-e728b19e6b85)

◦ Verilog/VHDL Files (.v or .vhdl or .vhd)


**DESIGN FILES**

`timescale 1ns / 1ps
module up_down_counter(
    input wire clk,       
    input wire reset,     
    input wire up_down,   
    input wire enable,    
    output reg [3:0] count 
);


    always @(posedge clk or posedge reset) begin
        if (reset) 
        begin
            count <= 4'b0000;
        end

        else if (enable)
        begin
            if (up_down)
            begin
                count <= count + 1'b1;
            end
            else begin
                count <= count - 1'b1;
            end
        end
    end

endmodule

**TESTBENCH**
 
`timescale 1ns / 1ps
module up_down_counter_tb();

    reg clk;
    reg reset;
    reg up_down;
    reg enable;
    wire [3:0] count;
    

    up_down_counter uut(
        .clk(clk),
        .reset(reset),
        .up_down(up_down),
        .enable(enable),
        .count(count)
    );
    

    initial begin
        clk = 0;
        forever #5 clk = ~clk;  
    end
    

    initial begin

        reset = 1;
        up_down = 1;
        enable = 0;
        

        #20 reset = 0;
        

        #10 enable = 1;
        #200;  
        

        up_down = 0;
        #200;  
        

        enable = 0;
        #50;
        

        reset = 1;
        #20 reset = 0;
        

        #50;
        

        $finish;
    end
    

    initial begin
        $monitor("Time: %t, Reset: %b, Up/Down: %b, Enable: %b, Count: %b", 
                 $time, reset, up_down, enable, count);
    end
    
endmodule

![Screenshot 2025-05-02 105016](https://github.com/user-attachments/assets/6cfbe563-bad5-4a58-a006-35751b203a81)


◦ SDC (Synopsis Design Constraint) File (.sdc)

 ### Step 2 : Creating an SDC File



•	In your terminal type “gedit input_constraints.sdc” to create an SDC File if you do not have one.

•	The SDC File must contain the following commands;

create_clock -name clk -period 2 -waveform {0 1} [get_ports "clk"]

set_clock_transition -rise 0.1 [get_clocks "clk"]

set_clock_transition -fall 0.1 [get_clocks "clk"]

set_clock_uncertainty 0.01 [get_ports "clk"]

set_input_delay -max 0.8 [get_ports "rst"] -clock [get_clocks "clk"]

set_output_delay -max 0.8 [get_ports "count"] -clock [get_clocks "clk"]

i→ Creates a Clock named “clk” with Time Period 2ns and On Time from t=0 to t=1.

ii, iii → Sets Clock Rise and Fall time to 100ps.

iv → Sets Clock Uncertainty to 10ps.

v, vi → Sets the maximum limit for I/O port delay to 1ps.

### Step 3 : Performing Synthesis

The Liberty files are present in the library path,

• The Available technology nodes are 180nm ,90nm and 45nm.

• In the terminal, initialise the tools with the following commands if a new terminal is being
used.

◦ csh

◦ source /cadence/install/cshrc

• The tool used for Synthesis is “Genus”. Hence, type “genus -gui” to open the tool.

![Screenshot 2025-05-02 112020](https://github.com/user-attachments/assets/0bb1c7c5-75e8-4024-99b5-07c74ef859ad)


• Genus Script file with .tcl file Extension commands are executed one by one to synthesize the netlist.

#### Synthesis RTL Schematic :

#### Area report:
![Screenshot 2025-05-07 210451](https://github.com/user-attachments/assets/5470ef34-b787-454e-b07b-a903b9251d3e)

#### Power Report:
![Screenshot 2025-05-07 210530](https://github.com/user-attachments/assets/abdb2a99-bf25-40cd-bcd6-cbd18c909f4f)

#### Timing Report: 
![Screenshot 2025-05-07 210509](https://github.com/user-attachments/assets/ad3bf5ce-0f87-4296-9e9c-76277f93d03c)


#### Result: 

The generic netlist has been created, and area, power, and timing reports have been tabulated and generated using Genus.





