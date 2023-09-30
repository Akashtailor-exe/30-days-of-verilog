# 1-Bit RAM Cell: 
A 1-bit RAM cell is the fundamental building block of memory in digital circuits. It stores a single binary value (0 or 1) and supports read and write operations controlled by write and read enable signals. 
This essential component plays a crucial role in creating larger memory arrays and sequential logic circuits.


```verilog
`timescale 1ns / 1ps

module RAM_Cell(
    input wire write_enable, write_data, read_enable,
    output reg read_data );

    reg memory_bit;

    always @(posedge write_enable or posedge read_enable)
    begin
        if (write_enable)
            memory_bit <= write_data;
        else if (read_enable)
            read_data <= memory_bit;
    end

endmodule
```

# 1-Bit RAM Cell Test Bench:

```verilog
`timescale 1ns / 1ps

module RAM_Cell_Testbench;

    reg write_enable;
    reg write_data;
    reg read_enable;
    wire read_data;

    RAM_Cell uut ( .write_enable(write_enable), .write_data(write_data),
     .read_enable(read_enable), .read_data(read_data) );

    reg clock = 0;

    always #5 clock = ~clock; 

    initial begin

        write_enable = 0;
        write_data = 0;
        read_enable = 0;

        write_enable = 1; write_data = 1; #10;

        write_enable = 0;  read_enable = 1; #10; 

        if (read_data === 1)
            $display("Test Passed: Read Data is Correct");
        else
            $display("Test Failed: Read Data is Incorrect");

        $finish;
    end

endmodule
```

## Schematics
![Alt Text](https://i.ibb.co/S6pYVtK/1-bit-ram-cell.png)

## Simulation
![Alt Text](https://i.ibb.co/pWjctH0/1-bit-ram-cell-tb.png)
