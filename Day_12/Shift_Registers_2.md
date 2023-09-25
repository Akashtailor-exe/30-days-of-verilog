# Parallel In Serial Out shift register:

```verilog
`timescale 1ns / 1ps

module PISO_shift_register (
    input wire clk, reset, parallel_in,
    output wire serial_out );

reg [3:0] shift_reg;

always @(posedge clk or posedge reset) begin
    if (reset) begin
        shift_reg <= 4'b0000; 
    end else begin
        shift_reg[0] <= parallel_in;
        shift_reg[1] <= shift_reg[0];
        shift_reg[2] <= shift_reg[1];
        shift_reg[3] <= shift_reg[2];
    end
end

assign serial_out = shift_reg[3];

endmodule
```

# Parallel In Serial Out shift register Test Bench:

```verilog
`timescale 1ns / 1ps

module PISO_shift_register_tb;
    reg clk;
    reg reset;
    reg parallel_in;

    wire serial_out;

    PISO_shift_register dut ( .clk(clk), .reset(reset), .parallel_in(parallel_in), 	.serial_out(serial_out) );

    always begin
        #5 clk = ~clk; 
    end

    initial begin
        clk = 0;
        reset = 0;
        parallel_in = 0;

        parallel_in = 1;
        #10 parallel_in = 0;
        #10 parallel_in = 1;
        #10 parallel_in = 0;
        #50 $finish;
    end

    always @(posedge clk) begin
        $display("Serial Out = %b", serial_out);
    end

endmodule
```

## Schematics
![Alt Text](https://i.ibb.co/fDvrTzg/PISO.png)

## Simulation
![Alt Text](https://i.ibb.co/tz6yGwj/PISO-TB.png)

# Parallel In parallel Out shift register:

```verilog
module pipo(din,clk,dout);
input wire [3:0] din;
input wire clk;
output reg [3:0] dout;

always @(posedge clk)
begin 
dout <= din;
end
endmodule
```

# Parallel In parallel Out shift register Test Bench:

```verilog
`timescale 1ns / 1ps

module testbench;

reg clk;
reg [3:0] din;
wire [3:0] dout;

pipo dut (.clk(clk), .din(din), .dout(dout));

initial begin
clk = 0;
forever #5 clk = ~clk;
end

initial begin

  $monitor("Input=%b, output=%b",din,dout);

  din = 4'b0000; #10;
  din = 4'b0110;  #10;
  din = 4'b0011; #10;
  din = 4'b1100;  #10;
  din = 4'b1010; #10;
  din = 4'b0101;  #10;
  #50
  $stop;

end

endmodule
```

## Schematics
![Alt Text](https://i.ibb.co/Y7YLFwB/PIPO.png)

## Simulation
![Alt Text](https://i.ibb.co/jvw4Hpd/PIPO-TB.png)

# Bidirectional Shift Register:

```verilog
module shift_reg #(parameter MSB = 8) ( input d, clk, en, dir, rstn,

output reg [MSB-1:0] out );


always @ (posedge clk)

if (!rstn)

out <= 0;

else begin

if (en)

case (dir)

0 : out <= {out[MSB-2:0], d};

1 : out <= {d, out[MSB-1:1]};

endcase

else

out <= out;

end

endmodule
```

# Bidirectional Shift Register Test Bench:

```verilog 
`timescale 1ns / 1ps

module shift_reg_tb;

parameter MSB = 8;

reg d;
reg clk;
reg en;
reg dir; 
reg rstn;

wire [MSB-1:0] out;

shift_reg #(MSB) dut ( .d(d),  .clk(clk), .en(en), .dir(dir), .rstn(rstn), .out(out));

always #5 clk = ~clk;  

initial begin
  clk = 0;
  rstn = 1;
  en = 1;
  dir = 0;

  rstn = 0;
  #10;
  rstn = 1;

  repeat (8) begin
    d = $random; 
    #10; 
  end

  dir = 1;

  repeat (8) begin
    d = 0; 
    #10;
  end
  $monitor("d=%b en=%b dir=%b rstn=%b out=%b",  d, en, dir, rstn, out);

  #100;
  $finish;
  
end
endmodule
```

## Schematics
![Alt Text](https://i.ibb.co/vPsnygG/Bi-dir-shift-reg.png)

## Simulation
![Alt Text](https://i.ibb.co/HdjSxxw/Bi-dir-shift-reg-TB.png)
