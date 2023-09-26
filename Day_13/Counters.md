# Ring Counter:

```verilog
`timescale 1ns / 1ps

module ring_counter(
    input clk, reset,
    output reg [3:0]  q
    );
    
    always @(posedge clk or posedge reset) begin
    if(reset) 
        q <= 4'b1000; 
    else
        q <= {q[2:0], q[3]};
end

    
endmodule
```

# Ring Counter Test Bench:

```verilog
`timescale 1ns / 1ps

module ring_counter_tb();

reg clk;
reg reset;
wire [3:0] q;
  
ring_counter dut( .clk(clk),  .reset(reset), .q(q) );
  
initial begin
  clk = 0;
  forever #5 clk = ~clk;
end

initial begin
  reset = 1; 
  #10 reset = 0;
  #200 $finish;
end
      
initial begin
  $monitor("%d : q = %b", $time, q);
end
  
endmodule
```
## Schematics
![Alt Text](https://i.ibb.co/FVV4czS/Ring-Counter.png)

## Simulation
![Alt Text](https://i.ibb.co/KstdGrn/Ring-counter-TB.png)

# Johnson Counter:

```verilog
module johnson_counter(
    input clk,
    input rst,
    output [3:0] q
);

reg [3:0] q_reg; 

always @(posedge clk or posedge rst) begin
  if (rst) 
    q_reg <= 4'b0000;
  else
    q_reg <= {q_reg[2:0], ~q_reg[3]};
end

assign q = q_reg;

endmodule
```

# Johnson Counter Test Bench:

```verilog
`timescale 1ns / 1ps

module johnson_counter_tb;

reg clk;
reg rst;

wire [3:0] q;

johnson_counter dut ( .clk(clk),  .rst(rst), .q(q) );

always #5 clk = ~clk; 

initial begin
  clk = 0;
  rst = 1;

  #15;
  rst = 0;
    
  #200;

  $finish;
end

always @(posedge clk) begin
  $display("At time %t, q = %b", $time, q);
end

endmodule
```

## Schematics
![Alt Text](https://i.ibb.co/z63B6j9/johnson-counter.png)

## Simulation
![Alt Text](https://i.ibb.co/QDzPMVc/johnson-counter-tb.png)
