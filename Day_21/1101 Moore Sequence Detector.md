# 1101 Moore Sequence Detector:

In Moore machine, output only depends on the present state. It is independent of current input.

```verilog
module FSM_Moore_Detector(
    input Din, 
    input Clock,
    input Reset,
    output reg Y );

parameter SIN = 3'd0, S1 = 3'd1, S11 = 3'd2, S110 = 3'd3, S1101 = 3'd4; 

reg [2:0] state, next_state;

always @(posedge Clock, posedge Reset) begin
  if(Reset) 
    state <= SIN;
  else
    state <= next_state; 
end

always @(*) begin
  case(state)
    SIN: begin
      next_state = Din ? S1 : SIN;
      Y = 1'b0;
    end
    
    S1: begin
      next_state = Din ? S11 : SIN;
      Y = 1'b0;  
    end
    
    S11: begin
      next_state = Din ? S11 : S110;
      Y = 1'b0;
    end
    
    S110: begin
      next_state = Din ? S1101 : SIN;
      Y = 1'b0;
    end
    
    S1101: begin
      next_state = Din ? S11 : SIN;
      Y = 1'b1;
    end
  endcase
end

endmodule
```

# 1101 Moore Sequence Detector Test Bench:

```verilog
`timescale 1ns / 1ps

module tb_FSM_Moore_Detector();

reg Din, Clock, Reset;
wire Y;

FSM_Moore_Detector dut( .Din(Din), .Clock(Clock), .Reset(Reset), .Y(Y) );

always #5 Clock = ~Clock; 

initial begin
  Clock = 0;
  Reset = 1; 
  #10 Reset = 0;
  
  // Input sequence
  Din = 1; #10
  Din = 0; #10
  Din = 1; #10
  Din = 1; #10
  Din = 0; #10
  Din = 1; #10
  
  #100
  $finish;
end

initial begin
  $monitor("Time=%0t Din=%b State=%b Y=%b", $time, Din, dut.state, Y);
end
      
endmodule
```

## Schematics
![Alt Text](https://i.ibb.co/GWLb9LQ/moore.png)

## Simulation
![Alt Text](https://i.ibb.co/kxFPgHj/moore-tb.png)
