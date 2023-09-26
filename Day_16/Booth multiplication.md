# Booth_Multiplier:

```verilog
`timescale 1ns / 1ps

module Booth_Multiplier(A, B, PRODUCT);
  input signed [3:0] A, B;
  output reg signed [7:0] PRODUCT;

  reg [1:0] temp;
  integer i;
  reg e;
  reg [3:0] B1;

  always @(A,B)
  begin
    PRODUCT = 8'd0;
    e = 1'b0;
    B1 = -B;
    
    for (i=0; i<4; i=i+1)
    begin
      temp = { A[i], e };
      case(temp)
        2'd2 : PRODUCT[7:4] = PRODUCT[7:4] + B1;
        2'd1 : PRODUCT[7:4] = PRODUCT[7:4] + B;
      endcase
      PRODUCT = PRODUCT >> 1;
      PRODUCT[7] = PRODUCT[6];
      e=A[i];
      
    end
  end
  
endmodule
```

# Booth Multiplication Test Bench:

```verilog
`timescale 1ns / 1ps

module testbench;

  reg signed [3:0] A;
  reg signed [3:0] B;

  wire signed [7:0] PRODUCT;

  Booth_Multiplier dut ( .A(A), .B(B), .PRODUCT(PRODUCT) );

  reg clk = 0;

  always begin
    #5 clk = ~clk;
  end

  initial begin
    A = 4'b0110;
    B = 4'b0011;
    #10; 
    $display("A = %d, B = %d, PRODUCT = %d", A, B, PRODUCT);
    if (PRODUCT === (A * B))
      $display("Test Case 1 PASSED");
    else
      $display("Test Case 1 FAILED");

    $finish;
  end
  always begin
    #5 clk = ~clk;
  end

endmodule
```

## Schematics
![Alt Text](https://i.ibb.co/GvNVY0t/booth-mul.png)

## Simulation
![Alt Text](https://i.ibb.co/7gtScHz/booth-mul-tb.png)
