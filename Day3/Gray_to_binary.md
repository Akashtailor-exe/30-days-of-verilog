# Gray to binary code conversion:
```verilog
`timescale 1ns / 1ps

module gray_to_binary(gray, bin);
  input [3:0] gray;
  output reg [3:0] bin;

  always @(gray) begin
    bin[3] = gray[3];
    bin[2] = gray[3] ^ gray[2];
    bin[1] = bin[2] ^ gray[1];
    bin[0] = bin[1] ^ gray[0];
  end
endmodule
```
# Gray to Binary conversion

![Alt Text](https://media.geeksforgeeks.org/wp-content/uploads/20220420085103/Screenshot696-300x206.png)

#  Gray to Binary code conversion: Testbench

```verilog
`timescale 1ns / 1ps

module testbench_gray_to_binary;

  reg [3:0] gray;
  wire [3:0] bin;
  integer i;
  gray_to_binary dut (.gray(gray),.bin(bin));
  initial begin
    $display("Input (Gray) | Output (Binary)");

    for (i = 0; i < 16; i = i + 1) begin
    gray = i;
      
    $display("%4b          | %4b", gray, bin);
    #10;
    end
    $finish;
  end

endmodule
```

## Schematics
![Alt Text](https://i.ibb.co/KXvyssC/gray_to_bin.png)

## Simulation
![Alt Text](https://i.ibb.co/FqDkBLC/gray_to_bin_simu.png)
