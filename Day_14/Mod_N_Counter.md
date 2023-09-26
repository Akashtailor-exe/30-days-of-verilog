# Mod-N Counter:

```verilog
`timescale 1ns / 1ps

module modn_counter #(parameter N = 15, parameter Width = 4)(
    input clk,
    input rstn,
    output reg [Width-1:0] out );
    
    always @(posedge clk)begin 
    
    if(!rstn)
    begin
    out <= 0;
    end
    else begin
    if (out == N-1)
    out <= 0;
    
    else
    out <= out + 1;
    
    end
    end
   
    endmodule
```

# Mod-N Counter Test Bench:

```verilog
`timescale 1ns / 1ps

module testbench;
parameter N = 15;
parameter Width = 4;

reg clk;
reg rstn;
wire [Width-1:0] out;

modn_counter dut (.clk(clk), .rstn(rstn), .out(out));

always #10 clk = ~clk;

initial begin
{clk, rstn} <= 0;

$monitor("T=%0t rstn=%0b out=0x%0h", $time, rstn, out);
repeat(2) @(posedge clk);
rstn <= 1;
repeat (20) @ (posedge clk);
$finish;
end

endmodule
```

## Schematics
![Alt Text](https://i.ibb.co/44X9b5n/Mod-N-Counter.png)

## Simulation
![Alt Text](https://i.ibb.co/hW3R9mw/Mod-N-Counter-TB.png)
