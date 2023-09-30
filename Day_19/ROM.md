# 1-Bit ROM:

ROM stands for Read-Only Memory. It is a type of non-volatile memory that is used to store data and programs that are not expected to change over time. As the name suggests, data can be read from a ROM, but it cannot be modified or written after it has been programmed.

```verilog 
`timescale 1ns / 1ps

module ROM_1Bit(
    input wire [1:0] addr,
    output wire data );
    
    assign data = (addr == 2'b00) ? 1'b0 :  // Define data for each memory location
                 (addr == 2'b01) ? 1'b1 :
                 (addr == 2'b10) ? 1'b0 :
                 (addr == 2'b11) ? 1'b1 : 1'b0;  // Default value for invalid addresses

endmodule
```

# 1-Bit ROM Test Bench:

```verilog
`timescale 1ns / 1ps

module ROM_1Bit_Testbench;

    reg [1:0] address;
    wire data;

    ROM_1Bit dut ( .addr(addr), .data(data) );

    initial begin

        addr = 2'b00;
        #10;

        addr = 2'b01;
        #10;

        if (data === 1'b1)
            $display("Test Passed: Read Data is Correct");
        else
            $display("Test Failed: Read Data is Incorrect");

        $finish;
    end

endmodule
```

## Schematics
![Alt Text](https://i.ibb.co/2d8yvcZ/1-bit-rom.png)

## Simulation
![Alt Text](https://i.ibb.co/cvj3WgS/1-bit-rom-tb.png)

# 4-Bit ROM:

```verilog
`timescale 1ns / 1ps

module ROM_4Bit(
    input wire [1:0] addr,
    output wire [3:0] data  );

    assign data = (addr == 2'b00) ? 4'b0000 :  // Define data for each memory location
                 (addr == 2'b01) ? 4'b1100 :
                 (addr == 2'b10) ? 4'b0011 :
                 (addr == 2'b11) ? 4'b1111 : 4'b0000;  // Default value for invalid addresses

endmodule
```

# 4-Bit ROM Test Bench:

```verilog
`timescale 1ns / 1ps

module ROM_4Bit_Testbench;

    reg [1:0] address;
    wire [3:0] data;

    ROM_4Bit dut ( .addr(address), .data(data) );

    initial begin
        address = 2'b00; 
        #10;

        address = 2'b10;
        #10;

        if (data === 4'b0011)
            $display("Test Passed: Read Data is Correct");
        else
            $display("Test Failed: Read Data is Incorrect");

        $finish;
    end

endmodule
```

## Schematics
![Alt Text](https://i.ibb.co/gWHpK84/4-bit-rom.png)

## Simulation
![Alt Text](https://i.ibb.co/nm7458F/4-bit-rom-tb.png)

# 8-Bit ROM:

```verilog
`timescale 1ns / 1ps

module ROM_8Bit(
    input wire [2:0] addr,
    output wire [7:0] data   ); 

    assign data = (addr == 3'b000) ? 8'b00110011 :  // Define data for each memory location
                 (addr == 3'b001) ? 8'b11001100 :
                 (addr == 3'b010) ? 8'b01010101 :
                 (addr == 3'b011) ? 8'b10101010 :
                 (addr == 3'b100) ? 8'b11110000 :
                 (addr == 3'b101) ? 8'b00001111 :
                 (addr == 3'b110) ? 8'b10000001 :
                 (addr == 3'b111) ? 8'b01111110 : 8'b00000000;  // Default value for invalid addresses

endmodule
```

# 8-Bit ROM Test Bench:

```verilog
`timescale 1ns / 1ps

module ROM_8Bit_Testbench;

    reg [2:0] address; 
    wire [7:0] data; 

    ROM_8Bit dut ( .addr(address), .data(data) );

    initial begin

        address = 3'b000;
        #10;

        address = 3'b010;  
        #10;

        if (data === 8'b01010101)
            $display("Test Passed: Read Data is Correct");
        else
            $display("Test Failed: Read Data is Incorrect");

        $finish;
    end

endmodule
```

## Schematics
![Alt Text](https://i.ibb.co/93f6p3H/8-bit-rom.png)

## Simulation
![Alt Text](https://i.ibb.co/4mVJYw6/8-bit-rom-tb.png)
