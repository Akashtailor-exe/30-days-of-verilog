# Round-Robin Arbiter:

- A round robin arbiter allows each requesting input an equal share of access to a shared resource by cycling through the requests in a circular order.
- It allocates access to each requesting party in a cyclic manner, ensuring fairness by granting each party a turn in a sequential order, regardless of the priority or timing of their requests.
- Round-robin arbiter prevents starvation by fairly granting access to the shared resource in a circular fashion. It ensures that no request is consistently denied access, promoting statistical fairness among competing requests.

```verilog
`timescale 1ns / 1ps

module round_robin_arbiter(
    input clk, rst,
    input [3:0] req_sigs,
    output reg [3:0] grant_sigs );

reg [2:0] curr_state; 
reg [2:0] next_state;

parameter [2:0] IDLE = 3'b000;
parameter [2:0] STATE_0 = 3'b001;
parameter [2:0] STATE_1 = 3'b010; 
parameter [2:0] STATE_2 = 3'b011;
parameter [2:0] STATE_3 = 3'b100;


always @(posedge clk or posedge rst) begin
  if(rst)
    curr_state <= IDLE;
  else 
    curr_state <= next_state;
end

always @(*) begin
  
  case(curr_state)

    IDLE: begin
            if(req_sigs[0])
              next_state = STATE_0;
            else if(req_sigs[1])
              next_state = STATE_1;  
            else if(req_sigs[2])
              next_state = STATE_2;
            else if(req_sigs[3])
              next_state = STATE_3;
            else
              next_state = IDLE;
          end

    STATE_0: begin
               if(req_sigs[1])
                 next_state = STATE_1;  
               else if(req_sigs[2])
                 next_state = STATE_2;
               else if(req_sigs[3])
                 next_state = STATE_3;
               else if(req_sigs[0])
                 next_state = STATE_0;
               else
                 next_state = IDLE;
             end

    STATE_1: begin
               if(req_sigs[2])
                 next_state = STATE_2;
               else if(req_sigs[3])
                 next_state = STATE_3;
               else if(req_sigs[0])
                 next_state = STATE_0;
               else if(req_sigs[1])
                 next_state = STATE_1;
               else
                 next_state = IDLE;
             end

    STATE_2: begin
               if(req_sigs[3])
                 next_state = STATE_3;
               else if(req_sigs[0])
                 next_state = STATE_0;
               else if(req_sigs[1])
                 next_state = STATE_1;
               else if(req_sigs[2])
                 next_state = STATE_2;
               else
                 next_state = IDLE;
             end

    STATE_3: begin
               if(req_sigs[0])
                 next_state = STATE_0;  
               else if(req_sigs[1])
                 next_state = STATE_1;
               else if(req_sigs[2])
                 next_state = STATE_2;
               else if(req_sigs[3])
                 next_state = STATE_3;
               else
                 next_state = IDLE;
             end

   default: begin
             if(req_sigs[0])
               next_state = STATE_0;
             else if(req_sigs[1])
               next_state = STATE_1;
             else if(req_sigs[2])
               next_state = STATE_2;
             else if(req_sigs[3])
               next_state = STATE_3;
             else
               next_state = IDLE;
           end

  endcase
end

always @(*) begin
  
  case(curr_state)

    STATE_0 : grant_sigs = 4'b0001;
    STATE_1 : grant_sigs = 4'b0010; 
    STATE_2 : grant_sigs = 4'b0100;
    STATE_3 : grant_sigs = 4'b1000;

    default : grant_sigs = 4'b0000; 

  endcase

end

endmodule

```

# Round-Robin Arbiter Test Bench:

```verilog
`timescale 1ns / 1ps

module round_robin_arbiter_tb();

reg clk;
reg rst;
reg [3:0] req_sigs;
wire [3:0] grant_sigs; 

round_robin_arbiter dut ( .clk(clk), .rst(rst), .req_sigs(req_sigs), 
.grant_sigs(grant_sigs) );

always #5 clk = ~clk;

initial begin
  clk = 0;
  rst = 0; 
  req_sigs = 0;

  req_sigs = 4'b1011;
  #20;
  req_sigs = 4'b1111;
  #20;
  req_sigs = 4'b1000;
  #20;
  req_sigs = 4'b1001;
  #20;
  req_sigs = 4'b1100;
  #20;
  $finish;
end

initial begin
  $monitor("req_sigs = %b, grant_sigs = %b", req_sigs, grant_sigs);
end

endmodule
```

## Schematics
![Alt Text](https://i.ibb.co/WfK5F7G/RR-Arbiter.png)

## Simulation
![Alt Text](https://i.ibb.co/qMpNjb8/RR-Arbiter-tb.png)
