# FSM_for_Sequence_Detector
# EXP NO.6.A. Sequence Detector Using Moore Machine and Mealy Machine

# Aim
To design and simulate a Finite-State-Machine-for-Sequence-Detector-1011 using Verilog HDL, and verify its functionality through a testbench in the Vivado 2023.1 environment.

# Apparatus Required
Vivado 2023.1

# Procedure
1.  Launch Vivado 2023.1 Open Vivado and create a new project.
2.  Design the Verilog Code Write the Verilog code for the RAM,ROM,FIFO Create the Testbench Write a testbench to simulate the memory behavior.
3.  The testbench should apply various and monitor the corresponding output.
4.  Create the Verilog Files Create both the design module and the testbench in the Vivado project. Run Simulation Run the behavioral simulation to verify the output.
5.  Observe the Waveforms Analyze the output waveforms in the simulation window, and verify that the correct read and write operation.
6.  Save and Document Results Capture screenshots of the waveform and save the simulation logs. These will be included in the lab report.

# Code
# Mealy 1011
// Verilog code
```
module mealy_1011(
    input clk,
    input rst,
    input x,
    output reg z
);

reg [1:0] state, next_state;

parameter S0 = 2'b00,
          S1 = 2'b01,
          S2 = 2'b10,
          S3 = 2'b11;

always @(posedge clk or posedge rst)
begin
    if(rst)
        state <= S0;
    else
        state <= next_state;
end

always @(*)
begin
    case(state)

        S0:
        begin
            if(x)
                next_state = S1;
            else
                next_state = S0;
            z = 0;
        end

        S1:
        begin
            if(x)
                next_state = S1;
            else
                next_state = S2;
            z = 0;
        end

        S2:
        begin
            if(x)
                next_state = S3;
            else
                next_state = S0;
            z = 0;
        end

        S3:
        begin
            if(x)
            begin
                next_state = S1;
                z = 1;   // sequence detected
            end
            else
            begin
                next_state = S2;
                z = 0;
            end
        end

        default:
        begin
            next_state = S0;
            z = 0;
        end

    endcase
end

endmodule
```
// Test bench
```
module mealy_1011_tb;

reg clk;
reg rst;
reg x;
wire z;

mealy_1011 uut(
    .clk(clk),
    .rst(rst),
    .x(x),
    .z(z)
);

always #5 clk = ~clk;

initial
begin
    clk = 0;
    rst = 1;
    x = 0;

    #10 rst = 0;

    // input sequence
    #10 x = 1;
    #10 x = 0;
    #10 x = 1;
    #10 x = 1;  // 1011 detected

    #10 x = 0;
    #10 x = 1;
    #10 x = 1;
    #10 x = 0;

    #50 $stop;
end

endmodule
```
// output Waveform

<img width="1912" height="928" alt="image" src="https://github.com/user-attachments/assets/b7c0fa95-e34c-4665-81a5-6c77dda8f6f5" />

# Moore 1011
// write verilog code for ROM using $random
```
module moore_1011_rom (
    input clk,
    input rst,
    input x,
    output reg z
);

reg [2:0] state;
reg [2:0] next_state;

reg [3:0] rom [0:31];  
// [next_state(2:0), output(0)]

integer i;

initial
begin
    for(i = 0; i < 32; i = i + 1)
        rom[i] = $random;
end

wire [4:0] addr;
assign addr = {state, x};

always @(*)
begin
    {next_state, z} = rom[addr];
end

always @(posedge clk or posedge rst)
begin
    if(rst)
        state <= 3'b000;
    else
        state <= next_state;
end

endmodule
```
// Test bench
```module moore_1011_rom_tb;

reg clk;
reg rst;
reg x;
wire z;

moore_1011_rom uut(
    .clk(clk),
    .rst(rst),
    .x(x),
    .z(z)
);

// clock generation
always #5 clk = ~clk;

initial
begin
    clk = 0;
    rst = 1;
    x = 0;

    #10 rst = 0;

    #10 x = 1;
    #10 x = 0;
    #10 x = 1;
    #10 x = 1;

    #10 x = 0;
    #10 x = 1;
    #10 x = 1;

    #50 $stop;
end

endmodule

```
// output Waveform

<img width="1916" height="985" alt="image" src="https://github.com/user-attachments/assets/ad24d0c8-1a3b-4cdc-8ec4-e657546e8b41" />


# Conclusion 
The Mealy and Moore state machine for sequence 1011 was designed and successfully simulated using Verilog HDL. The testbench verified both the write and read functionalities by simulating the sequence operations and observing the output waveforms.

