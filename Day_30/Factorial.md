# Factorial:

The factorial of a number is the product of all positive integers from 1 to that number, denoted as "n!" (n factorial).

```verilog

module factorial(
    input [7:0] num,
    output reg [31:0] result
);

integer i; 

always @(*) begin
    result = 1;
    for (i=1; i<=num; i=i+1) 
        result = result * i;
end

endmodule
```

# Factorial Test Bench:

```verilog
module testbench;
reg [7:0] num;
wire [31:0] result;

factorial dut(.num(num), .result(result));

initial begin
    num = 5; #10; 
    num = 7; #10;
end

initial begin
    $monitor("num = %0d, result = %0d", num, result);
end
      
endmodule
```

## Schematics
![Alt Text](https://i.ibb.co/FxpZLYf/fact.png)

## Simulation
![Alt Text](https://i.ibb.co/n7Pmb3w/fact-tb.png)
