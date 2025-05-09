![Screenshot 2025-05-09 101423](https://github.com/user-attachments/assets/7392650a-d963-4427-8a3d-ee22920dec1a)# 4Bit-Up-Down-Asynchronous-Reset-Counter-Synthesis

## Aim:

Synthesize 4Bit-Up-Down-Asynchronous-Reset-Counter design using Constraints and analyse reports, Timing, area and Power.

## Tool Required:

Functional Simulation: Incisive Simulator (ncvlog, ncelab, ncsim)

Synthesis: Genus

### Step 1: Getting Started

Synthesis requires three files as follows,

◦ Liberty Files (.lib)

![Screenshot 2025-04-22 181212](https://github.com/user-attachments/assets/ebd85a3f-7a9d-4e78-9136-e4239ac8e979)


◦ Verilog/VHDL Files (.v or .vhdl or .vhd)

**Testbench :**

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

**DESIGN :**

imescale 1ns / 1ps
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

◦ SDC (Synopsis Design Constraint) File (.sdc)

![Screenshot 2025-05-02 105125](https://github.com/user-attachments/assets/5614e71d-cd71-47fe-9015-b7d36a8f725e)


 ### Step 2 : Creating an SDC File

•	In your terminal type “gedit input_constraints.sdc” to create an SDC File if you do not have one.

![Screenshot 2025-05-07 210428](https://github.com/user-attachments/assets/5cccd6be-f2cb-4dae-b551-3234a488c1b3)


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

![Screenshot 2025-05-02 112020](https://github.com/user-attachments/assets/a14bdad7-8759-4738-ac44-4af5c580186f)

![Screenshot 2025-05-07 210655](https://github.com/user-attachments/assets/5467a2a7-6907-4b73-a2ec-c7cdbd61486d)

• Genus Script file with .tcl file Extension commands are executed one by one to synthesize the netlist.

#### Synthesis RTL Schematic :

#### Area report:

![Screenshot 2025-05-07 210451](https://github.com/user-attachments/assets/897b235a-d135-43e7-b520-797e7e9c9ce9)

#### Power Report:

![Screenshot 2025-05-07 210530](https://github.com/user-attachments/assets/432e4739-c84c-46ec-ac36-a3c3490e1d9a)

#### Timing Report: 

![Screenshot 2025-05-09 101423](https://github.com/user-attachments/assets/a2f41aae-82e2-4c71-b59f-2bf982509dcc)


#### Result: 

The generic netlist has been created, and area, power, and timing reports have been tabulated and generated using Genus.

![Screenshot 2025-05-02 112320](https://github.com/user-attachments/assets/0f266f1e-4074-4729-b6f2-145ef15f8494)





