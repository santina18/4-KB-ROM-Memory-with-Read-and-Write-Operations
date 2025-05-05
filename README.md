# 4 KB-ROM-Memory-with-Read-and-Write-Operations
# Aim
To design and simulate a 4KB ROM memory with read and write operations using Verilog HDL and verify the functionality through a testbench in the Vivado 2023.1 simulation environment.

# Apparatus Required
Vivado 2023.1 or equivalent Verilog simulation tool.
Computer system with a suitable operating system.

# Procedure
Launch Vivado 2023.1:

Open Vivado and create a new project.
Design the Verilog Code for ROM:

Write the Verilog code for a 4KB ROM memory with read and write capabilities.
Create the Testbench:

Write a testbench to simulate both the read and write operations, verifying that the data is correctly written to and read from the memory.
Add the Verilog Files:

Add the ROM Verilog module and the testbench file to the project.
Run Simulation:

Run the behavioral simulation in Vivado and check the memory's read and write operations.
Observe the Waveforms:

Analyze the waveform to verify that the memory read and write operations work as expected.
Save and Document Results:

Capture the waveform and include the simulation results in the final report.

# Verilog Code for Rom
```
module rom_mem(input clk,rst,input[9:0] address,output reg[7:0] data_out);

reg [7:0] rom_mem[1023:0];

initial begin
rom_mem[0]=8'h1A;
rom_mem[1]=8'h2A;
rom_mem[2]=8'h3A;
rom_mem[3]=8'h92;
rom_mem[4]=8'h5E;
rom_mem[5]=8'h6F;
rom_mem[6]=8'h70;

end

always@(posedge clk) 
begin

if(rst)

data_out <= 8'b0;

else

data_out <= rom_mem[address];

end

endmodule
```
# Testbench code for ROM
```
module rom_design_tb;
reg clk, rst;
reg [2:0] address;
wire [3:0] dataout;

rom_design uut (
  .clk(clk),
  .rst(rst),
  .address(address),
  .dataout(dataout)
);

initial begin
  clk = 1'b0;
  forever #5 clk = ~clk;
end

initial begin
  rst = 1'b1;
  address = 3'b000;
  
  // Reset assertion
  #10 rst = 1'b0;
  
  // Test address 0
  #10 address = 3'b000;
  #10 $display("Address: %d, Dataout: %d", address, dataout);
  
  // Test address 1
  #10 address = 3'b001;
  #10 $display("Address: %d, Dataout: %d", address, dataout);
  
  // Test address 2
  #10 address = 3'b010;
  #10 $display("Address: %d, Dataout: %d", address, dataout);
  
  // Test address 4
  #10 address = 3'b100;
  #10 $display("Address: %d, Dataout: %d", address, dataout);
  
  // Test address 5
  #10 address = 3'b101;
  #10 $display("Address: %d, Dataout: %d", address, dataout);
  
  // Test address 6
  #10 address = 3'b110;
  #10 $display("Address: %d, Dataout: %d", address, dataout);
  
  // Test address 7
  #10 address = 3'b111;
  #10 $display("Address: %d, Dataout: %d", address, dataout);
  
  // Test reset
  #10 rst = 1'b1;
  #10 $display("Reset asserted, Dataout: %d", dataout);
end

endmodule
```
# Output
![Screenshot 2025-05-03 135802](https://github.com/user-attachments/assets/bb1b925e-83ae-4d00-82c1-094b2b2c3e84)
  
# Verilog code for RAM
```
module ram_memory(input clk,rst,
input[7:0] data_in,input wr_en,input[9:0] address,
output reg[7:0] data_out);
reg[7:0] ram_memory[1023:0];
always@(posedge clk)
 begin 
  if(rst)
     data_out <= 8'b0;
    else if(wr_en)
    ram_memory[address] <= data_in;
    else
    data_out <= ram_memory[address];
    end
endmodule
```
# Testbench code for RAM
```
module rom_design_tb;
reg clk, rst;
reg [2:0] address;
wire [3:0] dataout;

rom_design uut (
  .clk(clk),
  .rst(rst),
  .address(address),
  .dataout(dataout)
);

initial begin
  clk = 1'b0;
  forever #5 clk = ~clk;
end

initial begin
  rst = 1'b1;
  address = 3'b000;
  
  // Reset assertion
  #10 rst = 1'b0;
  
  // Test address 0
  #10 address = 3'b000;
  #10 $display("Address: %d, Dataout: %d", address, dataout);
  
  // Test address 1
  #10 address = 3'b001;
  #10 $display("Address: %d, Dataout: %d", address, dataout);
  
  // Test address 2
  #10 address = 3'b010;
  #10 $display("Address: %d, Dataout: %d", address, dataout);
  
  // Test address 4
  #10 address = 3'b100;
  #10 $display("Address: %d, Dataout: %d", address, dataout);
  
  // Test address 5
  #10 address = 3'b101;
  #10 $display("Address: %d, Dataout: %d", address, dataout);
  
  // Test address 6
  #10 address = 3'b110;
  #10 $display("Address: %d, Dataout: %d", address, dataout);
  
  // Test address 7
  #10 address = 3'b111;
  #10 $display("Address: %d, Dataout: %d", address, dataout);
  
  // Test reset
  #10 rst = 1'b1;
  #10 $display("Reset asserted, Dataout: %d", dataout);
end

endmodule
```
# Output
![Screenshot 2025-05-03 142532](https://github.com/user-attachments/assets/651f7d2f-b7ab-4eba-83c4-9d86446837d5)
# Verilog code for FIFO
```
module fifo_8 #(parameter depth = 8, data_width = 8)
  (input  clk, rst, 
   input    w_en, r_en,
   input   [data_width-1:0] data_in,
   output  reg [data_width-1:0] data_out,
   output  full, empty);

reg [$clog2(depth)-1:0] w_ptr, r_ptr;
reg [data_width-1:0] fifo [depth-1:0];  // Adjusted array bounds

always @(posedge clk) begin
  if (!rst) begin
    w_ptr <= 0;
    r_ptr <= 0;
    data_out <= 0;
  end
end

always @(posedge clk) begin
  if (w_en && !full) begin
    fifo[w_ptr] <= data_in;
    w_ptr <= w_ptr + 1;  // Pointer wrap-around
  end
end

always @(posedge clk) begin
  if (r_en && !empty) begin
    data_out <= fifo[r_ptr];  // Corrected read data assignment
    r_ptr <= r_ptr + 1;  // Pointer wrap-around
  end
end

assign full = (w_ptr + 1 == r_ptr);
assign empty = (w_ptr == r_ptr);




endmodule
```
# Testbench code for FIFO
```
module fifo_8_tb;
reg clk, rst;
reg w_en, r_en;
reg [7:0] data_in;
wire [7:0] data_out;
wire full, empty;

fifo_8 #(.depth(8), .data_width(8)) uut (
    .clk(clk),
    .rst(rst),
    .w_en(w_en),
    .r_en(r_en),
    .data_in(data_in),
    .data_out(data_out),
    .full(full),
    .empty(empty)
);

initial begin
    clk = 1'b0;
    forever #5 clk = ~clk;
end

initial begin
    rst = 1'b1;
    w_en = 1'b0;
    r_en = 1'b0;
    data_in = 8'd0;
    
    // Reset assertion
    #10 rst = 1'b0;
    
    // Write data 1
    #10 w_en = 1'b1;
    data_in = 8'd1;
    #10;
    w_en = 1'b0;
    $display("Write 1, Full: %b, Empty: %b", full, empty);
    
    // Write data 2
    #10 w_en = 1'b1;
    data_in = 8'd2;
    #10;
    w_en = 1'b0;
    $display("Write 2, Full: %b, Empty: %b", full, empty);
    
    // Write data 3-8
    #10 w_en = 1'b1; data_in = 8'd3; #10; w_en = 1'b0;
    #10 w_en = 1'b1; data_in = 8'd4; #10; w_en = 1'b0;
    #10 w_en = 1'b1; data_in = 8'd5; #10; w_en = 1'b0;
    #10 w_en = 1'b1; data_in = 8'd6; #10; w_en = 1'b0;
    #10 w_en = 1'b1; data_in = 8'd7; #10; w_en = 1'b0;
    #10 w_en = 1'b1; data_in = 8'd8; #10; w_en = 1'b0;
    $display("Write 3-8, Full: %b, Empty: %b", full, empty);
    
    // Read data 1
    #10 r_en = 1'b1;
    #10;
    r_en = 1'b0;
    $display("Read 1, Data: %d, Full: %b, Empty: %b", data_out, full, empty);
    
    // Read data 2-8
    #10 r_en = 1'b1; #10; r_en = 1'b0;
    #10 r_en = 1'b1; #10; r_en = 1'b0;
    #10 r_en = 1'b1; #10; r_en = 1'b0;
    #10 r_en = 1'b1; #10; r_en = 1'b0;
    #10 r_en = 1'b1; #10; r_en = 1'b0;
    #10 r_en = 1'b1; #10; r_en = 1'b0;
    #10 r_en = 1'b1; #10; r_en = 1'b0;
    $display("Read 2-8, Full: %b, Empty: %b", full, empty);
    
    // Overflow test
    #10 w_en = 1'b1;
    data_in = 8'd9;
    #10;
    $display("Overflow, Full: %b, Empty: %b", full, empty);
    
    // Underflow test
    #10 r_en = 1'b1;
    #10;
    $display("Underflow, Full: %b, Empty: %b", full, empty);
end

endmodule
```
# Output
![Screenshot 2025-05-05 155005](https://github.com/user-attachments/assets/888a5b0d-be33-4ff6-b47e-cfa17fed1f20)


# Conclusion

In this experiment, a 4KB ROM memory with read and write operations was designed and successfully simulated using Verilog HDL. The testbench verified both the write and read functionalities by simulating the memory operations and observing the output waveforms. The experiment demonstrates how to implement memory operations in Verilog, effectively modeling both the reading and writing processes for ROM.
