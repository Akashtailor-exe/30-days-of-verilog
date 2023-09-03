# Binary to Gray code conversion:
```verilog
`timescale 1ns / 1ps

module binary_to_gray(bin,gray);
input[3:0]bin; 
output[3:0]gray; 
assign gray[3]=bin[3];
assign gray[2]=bin[3]^bin[2]; 
assign gray[1]=bin[2]^bin[1]; 
assign gray[0]=bin[1]^bin[0]; 
endmodule
```
# Binary to gray conversion

![Alt Text](https://media.geeksforgeeks.org/wp-content/uploads/20220420085103/Screenshot695-300x191.png)

# Binary to Gray code conversion: Testbench

```verilog
`timescale 1ns / 1ps

module testbench_binary_to_gray;
  reg [3:0] bin;
  wire [3:0] gray;
  integer i;
  binary_to_gray dut (.bin(bin),.gray(gray));

  initial begin
    bin = 4'b0000;
    $display("Input (Binary) | Output (Gray)");
    for (i = 0; i < 16; i = i + 1) begin
    $display("%4b     | %4b", bin, gray);
    bin = bin + 1;
    #10;
    end
    $finish;
  end

endmodule
```

## Schematics
![Alt Text](https://i.ibb.co/g9X3fjc/bin_to_gray.png)

## Simulation
![Alt Text](https://i.ibb.co/hyMRGr6/Bin_to_gray_simu.png)
