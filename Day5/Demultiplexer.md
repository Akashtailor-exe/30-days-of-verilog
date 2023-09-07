#  1 X 2 Demux:
```verilog
module demux_1x2 (
    input wire din,
    input wire sel,
    output wire dout_0,
    output wire dout_1
);

assign dout_0 = (sel) ? 1'b0 : din;
assign dout_1 = (sel) ? din : 1'b0;

endmodule
```

# 1 X 2 Demux Testbench

```verilog
module testbench_demux_1x2;

reg din;
reg sel;
wire dout_0;
wire dout_1;

demux_1x2 dut ( .din(din), .sel(sel), .dout_0(dout_0), .dout_1(dout_1) );

initial begin
    din = 1'b0; sel = 0;
    #10;
    din = 1'b1; sel = 0;
    #10;
    din = 1'b1; sel = 1;
    #10;
    din = 1'b0; sel = 1;
    #10;
    $finish;
end
always @(dout_0, dout_1) begin
    $display("sel = %b, din = %b, dout_0 = %b, dout_1 = %b", sel, din, dout_0, dout_1);
end
endmodule
```

## Schematics
![Alt Text](https://i.ibb.co/vCW1r3K/Demux_1X2.png)

## Simulation
![Alt Text](https://i.ibb.co/wyksGxV/Demux_1X2_Simu.png)


# 1 X 4 Demux:
```verilog
module demux_1x4 (
    input wire din,
    input wire [1:0] sel,
    output wire dout_0,
    output wire dout_1,
    output wire dout_2,
    output wire dout_3 );

assign dout_0 = (sel == 2'b00) ? din : 1'b0;
assign dout_1 = (sel == 2'b01) ? din : 1'b0;
assign dout_2 = (sel == 2'b10) ? din : 1'b0;
assign dout_3 = (sel == 2'b11) ? din : 1'b0;

endmodule
```

# 1 X 4 Demux Test Bench: 

```verilog
module testbench_demux_1x4;

reg din;
reg [1:0] sel;
wire dout_0;
wire dout_1;
wire dout_2;
wire dout_3;

demux_1x4 uut ( .din(din), .sel(sel), .dout_0(dout_0), .dout_1(dout_1), .dout_2(dout_2), 
.dout_3(dout_3) );

initial begin
    din = 1'b0; sel = 2'b00; #10;

    din = 1'b1; sel = 2'b00; #10;

    din = 1'b1; sel = 2'b01; #10;

    din = 1'b0; sel = 2'b10; #10;

    din = 1'b1; sel = 2'b11; #10;
    $finish;
end

always @(dout_0, dout_1, dout_2, dout_3) begin
    $display("sel = %b, din = %b, dout_0 = %b, dout_1 = %b, dout_2 = %b, dout_3 = %b", 
						sel, din, dout_0, dout_1, dout_2, dout_3);
end

endmodule
```

## Schematics
![Alt Text](https://i.ibb.co/N9VD11h/Demux_1X4.png)

## Simulation
![Alt Text](https://i.ibb.co/8zrrXCH/Demux_1X4_Simu.png)


# 1 X 8 Demux:
```verilog
module demux_1x8 (
    input wire din,
    input wire [2:0] sel,
    output wire dout_0,
    output wire dout_1,
    output wire dout_2,
    output wire dout_3,
    output wire dout_4,
    output wire dout_5,
    output wire dout_6,
    output wire dout_7
);

assign dout_0 = (sel == 3'b000) ? din : 1'b0;
assign dout_1 = (sel == 3'b001) ? din : 1'b0;
assign dout_2 = (sel == 3'b010) ? din : 1'b0;
assign dout_3 = (sel == 3'b011) ? din : 1'b0;
assign dout_4 = (sel == 3'b100) ? din : 1'b0;
assign dout_5 = (sel == 3'b101) ? din : 1'b0;
assign dout_6 = (sel == 3'b110) ? din : 1'b0;
assign dout_7 = (sel == 3'b111) ? din : 1'b0;

endmodule
```

# 1 X 8 Demux TestBench:

```verilog
module testbench_demux_1x8;

reg din;
reg [2:0] sel;
wire dout_0;
wire dout_1;
wire dout_2;
wire dout_3;
wire dout_4;
wire dout_5;
wire dout_6;
wire dout_7;

demux_1x8 dut ( .din(din), .sel(sel), .dout_0(dout_0), .dout_1(dout_1), .dout_2(dout_2),
    .dout_3(dout_3), .dout_4(dout_4), .dout_5(dout_5), .dout_6(dout_6), .dout_7(dout_7));

initial begin
    din = 1'b0; sel = 3'b000; #10;

    din = 1'b1; sel = 3'b000; #10;

    din = 1'b1; sel = 3'b001; #10;

    din = 1'b0; sel = 3'b010; #10;

    din = 1'b1; sel = 3'b111; #10;

    $finish;
end

always @(dout_0, dout_1, dout_2, dout_3, dout_4, dout_5, dout_6, dout_7) begin
    $display("sel = %b, din = %b, dout_0 = %b, dout_1 = %b, dout_2 = %b, dout_3 = %b, dout_4 = %b, dout_5 = %b, dout_6 = %b, dout_7 = %b", sel, din, dout_0, dout_1, dout_2, dout_3, dout_4, dout_5, dout_6, dout_7);
end

endmodule
```

## Schematics
![Alt Text](https://i.ibb.co/F3CdXzs/Demux_1X8.png)

## Simulation
![Alt Text](https://i.ibb.co/qJSWSzy/Demux_1X8_Simu.png)
