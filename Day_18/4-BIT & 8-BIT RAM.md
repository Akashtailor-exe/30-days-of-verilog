# 4-Bit RAM:

A 4-Bit RAM module is a digital memory component capable of storing and retrieving 4 bits of binary data. It typically has 16 memory locations, each capable of holding a 4-bit binary value. It supports read and write operations controlled by address selection and write/read enable signals, making it suitable for various applications requiring moderate storage capacity.

```verilog
`timescale 1ns / 1ps

module RAM_4Bit(
    input wire [3:0] address, 
    input wire write_enable,
    input wire [3:0] write_data, 
    input wire read_enable,  
    output reg [3:0] read_data
);

    reg [3:0] memory [0:15];

    always @(posedge write_enable or posedge read_enable)
    begin
        if (write_enable)
            memory[address] <= write_data;
        else if (read_enable)
            read_data <= memory[address];
    end

endmodule
```

# 4-Bit RAM Test Bench:

```verilog
`timescale 1ns / 1ps

module RAM_4Bit_Testbench;

    reg [3:0] address;
    reg write_enable;
    reg [3:0] write_data;
    reg read_enable;
    wire [3:0] read_data;

    RAM_4Bit dut ( .address(address), .write_enable(write_enable),
        .write_data(write_data), .read_enable(read_enable), .read_data(read_data) );

    reg clock = 0;

    always #5 clock = ~clock;

    initial begin
        address = 0;
        write_enable = 0;
        write_data = 4'b0000;
        read_enable = 0;

        write_enable = 1;
        address = 2;    
        write_data = 4'b1101; 
        #10;

        write_enable = 0;
        read_enable = 1;
        address = 2;  
        #10;

        if (read_data === 4'b1101)
            $display("Test Passed: Read Data is Correct");
        else
            $display("Test Failed: Read Data is Incorrect");

        $finish;
    end

endmodule
```

## Schematics
![Alt Text](https://i.ibb.co/ggxD6S5/4-bit-ram.png)

## Simulation
![Alt Text](https://i.ibb.co/DpzS3BN/4-bit-ram-tb.png)

# 8-Bit RAM:

An 8-Bit RAM module is an expanded version of a digital memory unit, capable of storing and retrieving 8 bits of binary data. It offers a larger memory space with 256 memory locations, each accommodating an 8-bit binary value. Similar to the 4-bit RAM, it supports read and write operations, making it suitable for applications requiring greater storage capacity, such as microcontrollers and data storage systems.

```verilog
`timescale 1ns / 1ps

module RAM_8Bit(
    input wire [7:0] address,
    input wire write_enable,
    input wire [7:0] write_data,
    input wire read_enable,
    output reg [7:0] read_data );

    reg [7:0] memory [0:255];

    always @(posedge write_enable or posedge read_enable)
    begin
        if (write_enable)
            memory[address] <= write_data;
        else if (read_enable)
            read_data <= memory[address];
    end

endmodule
```

# 8-Bit RAM Test Bench:

```verilog
`timescale 1ns / 1ps

module RAM_8Bit_Testbench;

    reg [7:0] address;
    reg write_enable;
    reg [7:0] write_data;
    reg read_enable;
    wire [7:0] read_data;

    RAM_8Bit dut ( .address(address), .write_enable(write_enable),
				 .write_data(write_data), .read_enable(read_enable), .read_data(read_data) );

    reg clock = 0;

    always #5 clock = ~clock; 

    initial begin
        address = 8'b0000_0000;
        write_enable = 0;
        write_data = 8'b0000_0000;
        read_enable = 0;

        write_enable = 1;
        address = 8'b0000_0010;
        write_data = 8'b1101_1010;
        #10;

        write_enable = 0;
        read_enable = 1;
        address = 8'b0000_0010;
        #10;

        if (read_data === 8'b1101_1010)
            $display("Test Passed: Read Data is Correct");
        else
            $display("Test Failed: Read Data is Incorrect");

        $finish;
    end

endmodule
```

## Schematics
![Alt Text](https://i.ibb.co/TLSrdH8/8-bit-ram.png)

## Simulation
![Alt Text](https://i.ibb.co/g9bGfNs/8-bit-ram-tb.png)
