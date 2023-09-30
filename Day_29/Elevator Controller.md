# Elevator Controller:

 A basic elevator controller using a finite state machine to handle floor requests and elevator movement. It includes states such as IDLE, MOVING_UP, MOVING_DOWN, and STOPPED to control elevator behavior.

 ```verilog
module elevator(
    input clk, reset,
    input [4:0] floor_req, // floor request signals
    output reg [4:0] floor_pos // signal indicating elevator position
);

parameter IDLE = 3'b000;
parameter MOVING_UP = 3'b001;
parameter MOVING_DOWN = 3'b010;
parameter STOPPED = 3'b011;

reg [2:0] state; 
reg [4:0] floor_req_reg; // register to hold floor requests
reg [4:0] floor_pos_reg; // register to hold elevator position

always @(posedge clk, posedge reset) begin
    if (reset) begin
        state <= IDLE; // reset to idle state
        floor_req_reg <= 5'b00000;
        floor_pos_reg <= 5'b00000;
        floor_pos <= floor_pos_reg;
    end else begin
        case (state)
            IDLE: begin
                if (floor_req != 5'b00000) begin
                    if (floor_req > floor_pos_reg) begin
                        state <= MOVING_UP; // move up to requested floor
                    end else if (floor_req < floor_pos_reg) begin
                        state <= MOVING_DOWN; // move down to requested floor
                    end else begin
                        state <= STOPPED; // already at requested floor
                    end
                end
            end
            MOVING_UP: begin
                if (floor_pos_reg == floor_req) begin
                    state <= STOPPED; // reached requested floor
                end else begin
                    floor_pos_reg <= floor_pos_reg + 1; // move up one floor
                end
            end
            MOVING_DOWN: begin
                if (floor_pos_reg == floor_req) begin
                    state <= STOPPED; // reached requested floor
                end else begin
                    floor_pos_reg <= floor_pos_reg - 1; // move down one floor
                end
            end
            STOPPED: begin
                floor_req_reg[floor_pos_reg] <= 1'b0; // clear request for current floor
                if (floor_req != 5'b00000) begin
                    if (floor_req > floor_pos_reg) begin
                        state <= MOVING_UP; // move up to requested floor
                    end else if (floor_req < floor_pos_reg) begin
                        state <= MOVING_DOWN; // move down to requested floor
                    end else begin
                        state <= STOPPED; // already at requested floor
                    end
                end else begin
                    state <= IDLE; // no more requests, go back to idle state
                end
            end
        endcase
        floor_pos <= floor_pos_reg;
        floor_req_reg[floor_req] <= 1'b1; // set request for requested floor
    end
end

endmodule
```

# Elevator Controller Test Bench:

```verilog
module elevator_tb;

reg clk;
reg reset;
reg [4:0] floor_req;
wire [4:0] floor_pos;

elevator dut ( .clk(clk), .reset(reset), .floor_req(floor_req), .floor_pos(floor_pos));

initial begin
    clk = 1'b0;
    reset = 1'b1;
    floor_req = 5'b00000;
    #10 reset = 1'b0; 
    #10 floor_req = 5'b00100; // request floor 3
    #100 floor_req = 5'b00010; // request floor 2
    #100 floor_req = 5'b01000; // request floor 4
    #100 floor_req = 5'b00001; // request floor 1
    #100 floor_req = 5'b01010; // request floor 5
    #100 $finish; 
end

always #5 clk = ~clk;

endmodule
```
## Schematics
![Alt Text](https://i.ibb.co/ZMkvcBw/elevator.png)

## Simulation
![Alt Text](https://i.ibb.co/7XLDJCK/elevator-tb.png)
