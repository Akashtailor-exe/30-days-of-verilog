# Traffic Light Controller:

```verilog
`timescale 1ns / 1ps

module traffic_light_controller(
  input clk, reset, 
  output reg red, yellow, green );

localparam RED = 3'b001; 
localparam YELLOW = 3'b010;
localparam GREEN = 3'b100;

reg [2:0] state; 

always @(posedge clk)
begin
  if (reset) 
    state <= RED;
  else
    case (state)
      RED: begin
        red <= 1'b1;
        yellow <= 1'b0;
        green <= 1'b0;
        state <= YELLOW;
      end

      YELLOW: begin
        red <= 1'b0;
        yellow <= 1'b1; 
        green <= 1'b0;
        state <= GREEN;
      end

      GREEN: begin
        red <= 1'b0;
        yellow <= 1'b0;
        green <= 1'b1;
        state <= RED;
      end
    endcase
end

endmodule
```

# Traffic Light Controller Test Bench:

```verilog
`timescale 1ns / 1ps

module traffic_light_tb();

reg clk;
reg reset;

wire red;
wire yellow;
wire green;

traffic_light_controller dut( .clk(clk), .reset(reset), .red(red), .yellow(yellow),
  .green(green) );

always #5 clk = ~clk; 

initial begin
  clk = 0;
  reset = 1;
  #10 reset = 0;

  #100 $finish;
end

always @(posedge clk) begin
  $display("RED=%b YELLOW=%b GREEN=%b", red, yellow, green);
end

endmodule
```

## Schematics
![Alt Text](https://i.ibb.co/BwJVGVg/traffic.png)

## Simulation
![Alt Text](https://i.ibb.co/3NfyC6v/traffic-tb.png)
