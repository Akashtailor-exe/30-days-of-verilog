# Custom Comparator:

```verilog
`timescale 1ns / 1ps

module custom_compare(input [3:0] input_a, input_b, 
             output reg equal, less_than, greater_than);
   
    always @(input_a, input_b) begin
        if (input_a == input_b) begin
            equal = 1'b1;
            less_than = 1'b0;
            greater_than = 1'b0;
        end
        else if (input_a > input_b) begin
            equal = 1'b0;
            less_than = 1'b0;
            greater_than = 1'b1;
        end
        else begin
            equal = 1'b0;
            less_than = 1'b1;
            greater_than = 1'b0;
        end
    end
endmodule
```

# Custom Comparator Test Bench:

```verilog
`timescale 1ns / 1ps

module tb_custom_compare();

    reg [3:0] a, b;
    wire equal, less_than, greater_than;

    custom_compare dut ( .input_a(a), .input_b(b), .equal(equal), .less_than(less_than), 
			.greater_than(greater_than) );

    reg clk;
    always begin
        #5 clk = ~clk;
    end

    initial begin
        clk = 0;
        a = 4'b0000; 
        b = 4'b0000;
        #10;

        a = 4'b0101;
        b = 4'b0010;
        #10;

        a = 4'b0010;
        b = 4'b0110;
        #10;

        $finish;
    end

    always @(posedge clk) begin
        $display("a = %b, b = %b", a, b);
        $display("Equal: %b, Less Than: %b, Greater Than: %b", 
					equal, less_than, greater_than);
    end

endmodule
```

## Schematics
![Alt Text](https://i.ibb.co/BBMXRxK/comparator.png)

## Simulation
![Alt Text](https://i.ibb.co/JCMB15S/comparator-Tb.png)
