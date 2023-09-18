#  SR Flip Flop

| Clock Edge | S | R | Q | Description |
| --- | --- | --- | --- | --- |
| ↓ | X | X | Q | No Change (Store previous input) |
| ↑ | 0 | 0 | Q | No Change (Store previous input) |
| ↑ | 1 | 0 | 1 | Set Q to 1 |
| ↑ | 0 | 1 | 0 | Reset Q to 0 |
| ↑ | 1 | 1 | X | Invalid state |

```verilog
module sr_ff_sync_reset (
  input clk,
  input reset,
  input s,
  input r,
  output reg q
);

always @(posedge clk) begin
  if (reset) 
    q <= 1'b0;
  else
    case ({s,r})
      2'b10: q <= 1'b1;
      2'b01: q <= 1'b0;
      default: q <= q; 
    endcase
end

endmodule
```

# SR Flip Flop Test Bench:

```verilog
`timescale 1ns/1ps

module sr_ff_sync_reset_tb();

reg clk;
reg reset;  
reg s;
reg r;
wire q;

sr_ff_sync_reset dut (.clk(clk), .reset(reset), .s(s), .r(r), .q(q) );

initial begin
  clk = 0;
  forever #5 clk = ~clk; 
end

initial begin
  reset = 1; s = 0; r = 0; 
  #15 reset = 0;
  #10 s = 1; r = 0;
  #10 s = 0; r = 1;
  #10 s = 0; r = 0;
  #10 $finish;
end
      
initial begin
  $monitor("At %t: s = %b, r = %b, reset = %b, q = %b", $time, s, r, reset, q);
end
      
endmodule
```

## Schematics
![Alt Text](https://i.ibb.co/YT1ZgDF/SR-FF.png)

## Simulation
![Alt Text](https://i.ibb.co/pZgX1gp/SR-FF-TB.png)


# JK Flip Flop:

| J | K | Q(n+1) | State |
| --- | --- | --- | --- |
| 0 | 0 | Qn | No Change |
| 0 | 1 | 0 | RESET |
| 1 | 0 | 1 | SET |
| 1 | 1 | Qn’ | TOGGLE |

```verilog
module jk_ff(
  input clk,
  input j,
  input k,
  output reg q
);

always @(posedge clk) begin
  case ({j,k})
    2'b00: q <= q;
    2'b01: q <= 1'b0;
    2'b10: q <= 1'b1;
    2'b11: q <= ~q;
  endcase
end

endmodule
```

# JK Flip Flop Test Bench:

```verilog
`timescale 1ns/1ps

module jk_ff_tb;

reg clk;
reg j;
reg k;  
wire q;

jk_ff dut (.clk(clk), .j(j), .k(k), .q(q));

always #5 clk = ~clk; 

initial begin
  clk = 0; 
  j = 0; k = 0; #10;
  j = 0; k = 1; #10;
  j = 1; k = 0; #10;
  j = 1; k = 1; #10;

  $finish; 
end

initial begin
  $monitor("At %t: j = %b, k = %b, q = %b", $time, j, k, q); 
end

endmodule
```

## Schematics
![Alt Text](https://i.ibb.co/hFNCF77/JK-FF-TB.png)
## Simulation
![Alt Text](https://i.ibb.co/pLShQ6r/JK-FF.png)


# D Flip Flop:

| CLOCK | D | Q | Qbar |
| --- | --- | --- | --- |
| 0 | 0 | NO CHANGE | NO CHANGE |
| 0 | 1 | NO CHANGE | NO CHANGE |
| 1 | 0 | 0 | 1 |
| 1 | 1 | 1 | 0 |

```verilog
module d_ff (
  input clk,
  input d, 
  output reg q,
  output qbar
);

always @(posedge clk) begin
  q <= d; 
end

assign qbar = ~q;

endmodule
```

# D Flip Flop Test Bench:

```verilog
`timescale 1ns/1ps

module d_ff_tb;

reg clk;
reg d;
wire q;
wire qbar;

d_ff dut ( .clk(clk), .d(d), .q(q), .qbar(qbar) );

always #5 clk = ~clk;

initial begin
  clk = 0;
  d = 0; #10;
  d = 1; #10;
  d = 0; #10;
  d = 1; #10;

  $finish;
end  

initial begin
  $monitor("At %t: d = %b, q = %b, qbar = %b", $time, d, q, qbar);
end

endmodule
```
## Schematics
![Alt Text](https://i.ibb.co/DRkY2WL/D-FF.png)
## Simulation
![Alt Text](https://i.ibb.co/f931Q7H/D-FF-TB.png)

# T Flip Flop:

| T | Qn | Qn+1 |  |
| --- | --- | --- | --- |
| 0 | 0 | 0 | Unchanged/hold |
| 0 | 1 | 1 | Unchanged/hold |
| 1 | 0 | 1 | Toggle |
| 1 | 1 | 0 | Toggle |

```verilog
module t_ff_sync_reset (
  input clk,
  input reset, 
  input t,
  output reg q
);

always @(posedge clk) begin
  if (reset) 
    q <= 1'b0;
  else 
    q <= ~q;
    
  if (t)
    q <= ~q;
end

endmodule
```
# T Flip Flop Test Bench:

```verilog

`timescale 1ns / 1ps 

module t_flipflop_tb;

  reg T;
  reg clk;
  reg rst;
  wire Q;

  t_flipflop dut ( .T(T), .clk(clk), .rst(rst), .Q(Q) );

  always begin
    clk = 0;
    #5; 
    clk = 1;
    #5;
  end

  initial begin
    T = 0;
    rst = 0;

    rst = 1;
    #10; 
    rst = 0;
    $display("Time T clk rst Q");
    $monitor("%2t %b %b %b %b", $time, T, clk, rst, Q);

    T = 1; #10; 
    T = 0; #10;

    rst = 1; #10;
    rst = 0;

    T = 1; #10;
    T = 0; #10;

    $finish;
  end

endmodule
```

## Schematics
![Alt Text](https://i.ibb.co/KwNW7kr/T-FF.png)
## Simulation
![Alt Text](https://i.ibb.co/ZVgQFd3/T-FF-TB.png)
