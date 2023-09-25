# Serial in Serial Out (SISO) Shift Register:

```verilog
`timescale 1ns / 1ps

module siso_shift_reg (
  input clk,
  input serial_in,
  output serial_out );

reg [3:0] shift_reg;

always @(posedge clk) begin
  shift_reg[0] <= serial_in;  // Shift data in from the right
  shift_reg[1] <= shift_reg[0];
  shift_reg[2] <= shift_reg[1];
  shift_reg[3] <= shift_reg[2];
end

assign serial_out = shift_reg[3];

endmodule
```

# SISO Shift Register Test Bench:

```verilog
`timescale 1ns / 1ps

module testbench;

  reg clk;
  reg serial_in;
  wire serial_out;

  siso_shift_reg dut ( .clk(clk), .serial_in(serial_in), .serial_out(serial_out) );

  always begin
    #5 clk = ~clk;
  end

  initial begin
    clk = 0;
    serial_in = 0;

    $display("Time\tSerial_In\tSerial_Out");
    $monitor("%d\t%d\t%d", $time, serial_in, serial_out);

    serial_in = 1;
    #10;
    serial_in = 0;
    #10;
    serial_in = 1;
    #10;
    serial_in = 0;
    #10;

    $finish;
  end

endmodule
```

## Schematics
![Alt Text](https://i.ibb.co/Tg3xnyS/SISO.png)

## Simulation
![Alt Text](https://i.ibb.co/1qf4fvm/SISO-TB.png)

# Serial in Parallel Out (SIPO) Shift Register:

```verilog
`timescale 1ns / 1ps

module sipo_shift_reg (
  input clk, reset, serial_in,
  output [3:0] parallel_out
);

reg [3:0] shift_reg;

always @(posedge clk or posedge reset) begin
  if (reset) begin
    shift_reg <= 4'b0000;
  end else begin
    shift_reg[0] <= serial_in;
    shift_reg[1] <= shift_reg[0];
    shift_reg[2] <= shift_reg[1];
    shift_reg[3] <= shift_reg[2];
  end
end

assign parallel_out = shift_reg;

endmodule
```

# SIPO Shift Register Test Bench:

```verilog 
`timescale 1ns / 1ps

module sipo_shift_reg_tb;

  reg clk;
  reg reset;
  reg serial_in;
  wire [3:0] parallel_out;

  sipo_shift_reg dut ( .clk(clk), .reset(reset), .serial_in(serial_in), 
												.parallel_out(parallel_out) );

  always begin
    #5 clk = ~clk;
  end

  initial begin
    clk = 0;
    reset = 0;
    serial_in = 0;
    
    $display("Time\tSerial_In\tParallel_Out");
    $monitor("%d\t%d\t%d", $time, serial_in, parallel_out);

    serial_in = 1;
    #10;
    serial_in = 0;
    #10;
    serial_in = 1;
    #10;
    serial_in = 0;
    #10;

    $finish;
  end

endmodule
```

## Schematics
![Alt Text](https://i.ibb.co/FhFtQCD/SIPO.png)

## Simulation
![Alt Text](https://i.ibb.co/kKBFxc5/SIPO-TB.png)
