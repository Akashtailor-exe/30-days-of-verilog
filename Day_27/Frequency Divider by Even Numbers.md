# Frequency Divider by Even Numbers:

- Even Freq/Clk divider:
    - Divide the input clock by an even integer (2, 4, 6, etc)
    - Output clock toggles when the counter reaches exactly divisor/2
    - Gives 50% duty cycle output clock

```verilog
module clk_divider(
    input clk_in,
    input [7:0] divisor,
    output reg clk_out
);

reg [7:0] counter = 8'd0;

always @(posedge clk_in) begin
    counter <= counter + 1;
    if (counter >= (divisor - 1)) begin
        counter <= 0;
    end
    clk_out <= (counter < (divisor/2))?1'b1:1'b0;
end

endmodule
```

# Frequency Divider by Even Numbers Test Bench:

```verilog
`timescale 1ns / 1ps

module clk_divider_tb;

reg clk_in;
reg [7:0] divisor;
wire clk_out;

clk_divider dut ( .clk_in(clk_in), .divisor(divisor), .clk_out(clk_out) );

initial begin
    clk_in = 1'b0;
    divisor = 8'd4;
    #10;
    repeat (20) begin
        #5 clk_in = ~clk_in;
    end
    #10 $finish;
end

always @(posedge clk_out) begin
    $display("clk_out = %b", clk_out);
end

endmodule
```

## Schematics
![Alt Text](https://i.ibb.co/59Sz1qV/freq-divide.png)

## Simulation
![Alt Text](https://i.ibb.co/b2KZ7ZF/freq-divide-tb.png)
