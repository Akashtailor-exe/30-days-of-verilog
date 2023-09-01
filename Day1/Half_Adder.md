# Half Adder using Dataflow modeling
```verilog
`timescale 1ns / 1ps

module half_adder_dataflow( input A,B, output Sum,Carry);

assign Sum = A^B;
assign Carry = A&B;

endmodule
```

# truthtable

| Input |   | Output  |           |
|-------|---|---------|-----------|
| A     | B | S (Sum) | C (Carry) |
| 0     | 0 | 0       | 0         |
| 0     | 1 | 1       | 0         |
| 1     | 0 | 1       | 0         |
| 1     | 1 | 0       | 1         |


# half adder Testbench (dataflow model)

```verilog
`timescale 1ns / 1ps

module half_adder_dataflow_tb;
reg A_tb;
reg B_tb;

wire Carry_tb;
wire Sum_tb;
half_adder_dataflow dut (.A(A_tb), .B(B_tb), .Sum(Sum_tb), .Carry(Carry_tb));

initial begin
A_tb = 1'b0; B_tb = 1'b0;
#10 A_tb = 1'b0; B_tb = 1'b1;
#10 A_tb = 1'b1; B_tb = 1'b0;
#10 A_tb = 1'b1; B_tb = 1'b1;
#10$stop;

end

endmodule

```

## Schematics
![Alt Text](https://i.ibb.co/CJ91XVS/Half_Adder.png)

## Simulation
![Alt Text](https://i.ibb.co/tLLPdM8/Half_adder_sim.png)
