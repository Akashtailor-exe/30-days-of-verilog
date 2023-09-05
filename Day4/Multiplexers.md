#  2 X 1 Mux:
```verilog

`timescale 1ns / 1ps

module mux21 (input i0,i1,sel, output reg out);
always @(*) begin
if(sel)
out = i1;
else
out = i0;
end
endmodule

```

# 2 X 1 Mux Testbench

```verilog

`timescale 1ns / 1ps

module testbench_mux_2_X_1;
  reg i0, i1, sel;
  wire out;
 
 mux21 dut ( .i0(i0), .i1(i1), .sel(sel), .out(out) );

  initial begin
    i0 = 0;
    i1 = 1;
    sel = 0;
   
 $display("i0 = %b, i1 = %b, sel = %b, out = %b", i0, i1, sel, out);

	  sel = 0;
    #10;
    $display("i0 = %b, i1 = %b, sel = %b, out = %b", i0, i1, sel, out);

    sel = 1; 
    #10; 
    $display("i0 = %b, i1 = %b, sel = %b, out = %b", i0, i1, sel, out);

    $finish;
  end

endmodule

```

## Schematics
![Alt Text](https://i.ibb.co/59y1PBg/2X1_Mux.png)

## Simulation
![Alt Text](https://i.ibb.co/6BvbVgs/2X1_Mux_Tb.png)


# 4 X 1 Mux:
```verilog

`timescale 1ns / 1ps

module mux41 (
  input wire [3:0] data_in,
  input wire [1:0] sel, 
  output wire data_out
);

  assign data_out = (sel == 2'b00) ? data_in[0] :
                   (sel == 2'b01) ? data_in[1] :
                   (sel == 2'b10) ? data_in[2] :
                                   data_in[3];

endmodule

```

# 4 X 1 Mux Test Bench:

```verilog

`timescale 1ns / 1ps

module mux41_tb;

  reg [3:0] data_in;
  reg [1:0] sel;
  wire data_out;

  mux41 dut ( .data_in(data_in), .sel(sel), .data_out(data_out) );

  initial begin

    data_in = 4'b0000;
    sel = 2'b00;

    $display("data_in = %b, sel = %b, data_out = %b", data_in, sel, data_out);

    sel = 2'b00;
    #10;
    $display("data_in = %b, sel = %b, data_out = %b", data_in, sel, data_out);

    sel = 2'b01;
    data_in = 4'b1010;
    #10;
    $display("data_in = %b, sel = %b, data_out = %b", data_in, sel, data_out);

    sel = 2'b10; 
    data_in = 4'b1100;
    #10;
    $display("data_in = %b, sel = %b, data_out = %b", data_in, sel, data_out);

    sel = 2'b11;
    data_in = 4'b1111;
    #10;
    $display("data_in = %b, sel = %b, data_out = %b", data_in, sel, data_out);

    $finish;
  end

endmodule

```

## Schematics
![Alt Text](https://i.ibb.co/2kYgpMz/4X1_Mux.png)

## Simulation
![Alt Text](https://i.ibb.co/fdkGCLd/4X1_Mux_Tb.png)


# 8 X 1 Mux
```verilog

`timescale 1ns / 1ps

module mux81 (
  input wire [7:0] data_in, 
  input wire [2:0] sel, 
  output wire data_out 
);

  assign data_out = (sel == 3'b000) ? data_in[0] :
                   (sel == 3'b001) ? data_in[1] :
                   (sel == 3'b010) ? data_in[2] :
                   (sel == 3'b011) ? data_in[3] :
                   (sel == 3'b100) ? data_in[4] :
                   (sel == 3'b101) ? data_in[5] :
                   (sel == 3'b110) ? data_in[6] :
                                     data_in[7];

endmodule

```

# 8 X 1 Mux TestBench

```verilog

`timescale 1ns / 1ps

module mux81_tb ;

  reg [7:0] data_in;
  reg [2:0] sel;
  wire data_out;

  mux81 dut ( .data_in(data_in), .sel(sel), .data_out(data_out) );

  initial begin
    data_in = 8'b00000000;
    sel = 3'b000;

    $display("data_in = %b, sel = %b, data_out = %b", data_in, sel, data_out);

    sel = 3'b000; 
    #10; 
    $display("data_in = %b, sel = %b, data_out = %b", data_in, sel, data_out);

    sel = 3'b001;
    data_in = 8'b11011011;
    #10;
    $display("data_in = %b, sel = %b, data_out = %b", data_in, sel, data_out);

    sel = 3'b010;
    data_in = 8'b10101010;
    #10;
    $display("data_in = %b, sel = %b, data_out = %b", data_in, sel, data_out);


    sel = 3'b111;
    data_in = 8'b11111111;
    #10; 
    $display("data_in = %b, sel = %b, data_out = %b", data_in, sel, data_out);

    $finish;
  end

endmodule

```

## Schematics
![Alt Text](https://i.ibb.co/Lv4qQ2h/8X1_Mux.png)

## Simulation
![Alt Text](https://i.ibb.co/cy6mcJZ/8X1_Mux_Tb.png)
