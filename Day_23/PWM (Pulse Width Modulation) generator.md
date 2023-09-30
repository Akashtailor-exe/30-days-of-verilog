# PWM (Pulse Width Modulation) generator:

A PWM (Pulse Width Modulation) generator is a circuit that produces a digital signal with a varying pulse width. It is commonly used in applications such as motor speed control, LED dimming, and audio synthesis.

The percentage of time in which the PWM signal remains HIGH (on time) is called as duty cycle. If the signal is always ON it is in 100% duty cycle and if it is always off it is 0% duty cycle.

**`Duty Cycle =Turn ON time/ (Turn ON time + Turn OFF time)`**

```verilog
`timescale 1ns / 1ps

module PWM(
    input clk,
    output [3:0] out  );
    
    reg [7:0] count = 0 ;
    
always@(posedge clk ) begin

if (count < 100) count <= count + 1;
else count <=0;

end

assign out[0] = (count < 20) ? 1:0;
assign out[1] = (count < 40) ? 1:0;
assign out[2] = (count < 60) ? 1:0;
assign out[3] = (count < 80) ? 1:0;

endmodule
```

# PWM Generator Test Bench:

```verilog
`timescale 1ns / 1ps

module Testbench;

reg clk;
wire [3:0] out;

PWM dut (.clk(clk), .out(out));

always begin
	clk=1;
	forever #5 clk= ~clk;
   end	
endmodule
```

## Schematics
![Alt Text](https://i.ibb.co/PctsFSh/pwm.png)

## Simulation
![Alt Text](https://i.ibb.co/86GT3HQ/pwm-tb.png)
