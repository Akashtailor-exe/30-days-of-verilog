# 1010 Mealy Sequence Detector:

```verilog
`timescale 1ns / 1ps

module mealy_fsm_1010(
    input din, clk, rst,
    output reg dout );
    reg [1:0] current_state, next_state;
    
    parameter S0 = 2'b00,
              S1 = 2'b01,
              S2 = 2'b10,
              S3 = 2'b11;

    always @(posedge clk) begin
        if (rst == 1) begin
            current_state <= S0;
        end else begin
            current_state <= next_state;
        end
    end

    always @(current_state or din) begin
        case (current_state)
            S0: begin
                if (din == 1) begin
                    next_state = S1;
                    dout = 1'b0;
                end else begin
                    next_state = S0;
                    dout = 1'b0;
                end
            end

            S1: begin
                if (din == 0) begin
                    next_state = S2;
                    dout = 1'b0;
                end else begin
                    next_state = S1;
                    dout = 1'b0;
                end
            end

            S2: begin
                if (din == 1) begin
                    next_state = S3;
                    dout = 1'b0;
                end else begin
                    next_state = S0;
                    dout = 1'b0;
                end
            end

            S3: begin
                if (din == 0) begin
                    next_state = S0;
                    dout = 1'b1;
                end else begin
                    next_state = S1;
                    dout = 1'b0;
                end
            end

            default: next_state = S0;
        endcase
    end

endmodule
```

# 1010 Mealy Sequence Detector Test Bench:

```verilog
`timescale 1ns / 1ps

module mealy_fsm_tb;

    reg din;
    reg clk;
    reg rst;

    wire dout;

    mealy_fsm_1010 dut ( .din(din), .clk(clk), .rst(rst), .dout(dout) );

    always begin
        #5 clk = ~clk;
    end

    initial begin

        din = 0;
        clk = 0;
        rst = 1;
        #10 rst = 0;

        din = 1; #10;
        din = 0; #10;
        din = 1; #10;
			  din = 0; #10;
        din = 1; #10;

        $finish;
    end

    always @(posedge clk) begin
        $monitor("Time=%t, din=%b, dout=%b", $time, din, dout);
    end

endmodule
```

## Schematics
![Alt Text](https://i.ibb.co/m9vnz8g/mealy.png)

## Simulation
![Alt Text](https://i.ibb.co/wMVj0wv/mealy-tb.png)
