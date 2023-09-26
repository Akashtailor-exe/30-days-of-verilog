# Gray Counter:

```verilog
`timescale 1ns / 1ps

module gray_counter(
    input clk,rst,
    output reg [3:0] gray_count
    );

    reg [3:0] bin_count;
   
    always@(posedge clk)
    begin 
        if(rst)
            begin
                gray_count=4'b0000;
                bin_count=4'b0000;
            end
        else
            begin
                bin_count = bin_count + 1;
                gray_count = {bin_count[3],bin_count[3]^bin_count[2],
								bin_count[2]^bin_count[1],bin_count[1]^bin_count[0]};
            end
    end   
endmodule
```

# Gray Counter Test Bench:

```verilog   
`timescale 1ns / 1ps

module testbench;

reg clk,reset;
wire [3:0] gray_count;

gray_counter dut(clk, reset, gray_count);

initial begin
clk= 1'b0;
forever #5 clk= ~clk;
end

initial begin
reset= 1'b1;
#10;
reset= 1'b0;
end

initial begin
$monitor("\t\t counter: %d", gray_count);
#175 $finish;
end
endmodule
```

## Schematics
![Alt Text](https://i.ibb.co/JmsBT0c/gray-counter.png)

## Simulation
![Alt Text](https://i.ibb.co/R9Mx6XH/gray-counter-tb.png)
