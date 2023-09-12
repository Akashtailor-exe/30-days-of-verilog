#  4 to 2 Encoder:
```verilog
`timescale 1ns / 1ps

module encoder_4_2(
    input [3:0] in,
    output reg [1:0] out
);

always @(*) begin
    case(in)
        4'b0001 : out = 2'b00;
        4'b0010 : out = 2'b01; 
        4'b0100 : out = 2'b10;
        4'b1000 : out = 2'b11;
        default : out = 2'bxx; 
    endcase
end

endmodule
```

# 4 to 2 Encoder Test Bench:

```verilog
module testbench;

reg [3:0] in;
wire [1:0] out;

encoder_4_2 dut(.in(in), .out(out) );

initial begin
    $monitor("in = %b, out = %b", in, out); 
    in = 4'b0001; #10;
    in = 4'b0010; #10;
    in = 4'b0100; #10;
    in = 4'b1000; #10;
    in = 4'b1111; #10;    
end
      
endmodule
```

## Schematics
![Alt Text](https://i.ibb.co/vktjZ64/4-to-2-encoder.png)

## Simulation
![Alt Text](https://i.ibb.co/rsrJvVT/4-to-2-encoder-simu.png)


# Octal to Binary Encoder (8 to 3 Encoder)
```verilog
`timescale 1ns / 1ps

module octal_to_binary_encoder (
    input [7:0] octal,
    output reg [2:0] binary
);

always @(*) begin
    case(octal)
        8'b0000_0001 : binary = 3'b000; 
        8'b0000_0010 : binary = 3'b001;
        8'b0000_0100 : binary = 3'b010;
        8'b0000_1000 : binary = 3'b011;
        8'b0001_0000 : binary = 3'b100;
        8'b0010_0000 : binary = 3'b101;
        8'b0100_0000 : binary = 3'b110;
        8'b1000_0000 : binary = 3'b111;
        default: binary = 3'bxxx;
    endcase
end

endmodule
```

# Octal to Binary Encoder (8 to 3 Encoder) Test Bench

```verilog
`timescale 1ns / 1ps

module testbench;

reg [7:0] octal;  
wire [2:0] binary;

octal_to_binary_encoder dut ( .octal(octal), .binary(binary) );

initial begin
  $monitor("octal = %b, binary = %b", octal, binary);

  octal = 8'b0000_0001; #10;
  octal = 8'b0000_0010; #10;
  octal = 8'b0000_0100; #10;
  octal = 8'b0000_1000; #10;
  octal = 8'b0001_0000; #10;
  octal = 8'b0010_0000; #10;  
  octal = 8'b0100_0000; #10;
  octal = 8'b1000_0000; #10;
  octal = 8'bXXXX_XXXX; #10;
end
      
endmodule

```

## Schematics
![Alt Text](https://i.ibb.co/wYNS9Np/8-to-3-encoder.png)

## Simulation
![Alt Text](https://i.ibb.co/nz2g3C9/8-to-3-encoder-simu.png)

