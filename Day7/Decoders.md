#  2 to 4 Decoder:
```verilog
`timescale 1ns / 1ps

module decoder_2to4 (
    input [1:0] in,
    output reg [3:0] out
);

always @(*) begin
    case(in)
        2'b00: out = 4'b0001;
        2'b01: out = 4'b0010;
        2'b10: out = 4'b0100;
        2'b11: out = 4'b1000;
        default: out = 4'b0000;
    endcase
end

endmodule
```

# 2 to 4 Decoder Test Bench:

```verilog
`timescale 1ns / 1ps

module testbench;

reg [1:0] in;
wire [3:0] out;

decoder_2to4 dut (.in(in), .out(out));

initial begin

in = 2'b00; #10;
in = 2'b01; #10;
in = 2'b10; #10;
in = 2'b11; #10;
end

endmodule
```

## Schematics
![Alt Text](https://i.ibb.co/yXC4t2y/2-to-4-Decoder.png)

## Simulation
![Alt Text](https://i.ibb.co/5FmzGpQ/2-to-4-Decoder-simu.png)


# 3 to 8 Decoder:
```verilog
`timescale 1ns / 1ps

module decoder_3to8 (
    input [2:0] in,
    output reg [7:0] out
);
integer i;
always @(*) begin
    out = 8'b00000000; 
    for (i = 0; i < 8; i = i + 1) begin
        if (i == in) begin
            out[i] = 1'b1; 
        end
    end
end

endmodule
```

# 3 to 8 Decoder Test Bench:

```verilog
`timescale 1ns / 1ps

module testbench;

    reg [2:0] in;
    wire [7:0] out;
    
    decoder_3to8 dut ( .in(in), .out(out) );

    initial begin
        in = 3'b000; #10;
        $display("Input = %b, Output = %b", in, out);
        in = 3'b001; #10;
        $display("Input = %b, Output = %b", in, out);
        in = 3'b010; #10;
        $display("Input = %b, Output = %b", in, out);
        in = 3'b011; #10;
        $display("Input = %b, Output = %b", in, out);
        in = 3'b100; #10;
        $display("Input = %b, Output = %b", in, out);
        in = 3'b101; #10;
        $display("Input = %b, Output = %b", in, out);
        in = 3'b110; #10;
        $display("Input = %b, Output = %b", in, out);
        in = 3'b111; #10;
        $display("Input = %b, Output = %b", in, out);
        
        $finish;
    end

endmodule
```

## Schematics
![Alt Text](https://i.ibb.co/XytTMLt/3-to-8-Decoder.png)

## Simulation
![Alt Text](https://i.ibb.co/2t089ph/3-to-8-Decoder-simu.png)


# 4 to 16 decoder:
```verilog
`timescale 1ns / 1ps

module decoder_4to16 (
    input [3:0] in,
    output [15:0] out
);

assign out[0] = (in == 4'b0000) ? 1'b1 : 1'b0;
assign out[1] = (in == 4'b0001) ? 1'b1 : 1'b0;
assign out[2] = (in == 4'b0010) ? 1'b1 : 1'b0;
assign out[3] = (in == 4'b0011) ? 1'b1 : 1'b0;
assign out[4] = (in == 4'b0100) ? 1'b1 : 1'b0;
assign out[5] = (in == 4'b0101) ? 1'b1 : 1'b0;
assign out[6] = (in == 4'b0110) ? 1'b1 : 1'b0;
assign out[7] = (in == 4'b0111) ? 1'b1 : 1'b0;
assign out[8] = (in == 4'b1000) ? 1'b1 : 1'b0;
assign out[9] = (in == 4'b1001) ? 1'b1 : 1'b0;
assign out[10] = (in == 4'b1010) ? 1'b1 : 1'b0;
assign out[11] = (in == 4'b1011) ? 1'b1 : 1'b0;
assign out[12] = (in == 4'b1100) ? 1'b1 : 1'b0;
assign out[13] = (in == 4'b1101) ? 1'b1 : 1'b0;
assign out[14] = (in == 4'b1110) ? 1'b1 : 1'b0;
assign out[15] = (in == 4'b1111) ? 1'b1 : 1'b0;

endmodule
```

# 4 to 16 decoder Test Bench:

```verilog
`timescale 1ns / 1ps

module tb_decoder_4to16;

reg [3:0] in;
wire [15:0] out;

decoder_4to16 dut ( .in(in), .out(out) );

initial begin

    in = 4'b0000;#10;
    
    in = 4'b0001;#10;
    
    in = 4'b0010;#10;
    
    in = 4'b0011;#10;
    
    in = 4'b1111;#10;
    
    $finish;
end

always @(out) begin
    $display("Input: %b, Output: %b", in, out);
end

endmodule

```

## Schematics
![Alt Text](https://i.ibb.co/cvW1YVX/4-to-16-decoder.png)

## Simulation
![Alt Text](https://i.ibb.co/M5bKp69/4-to-16-decoder-simu.png)
