# Full Adder using Dataflow modeling
```verilog
`timescale 1ns / 1ps

module Full_Adder_DF(
    input A,B,Cin,
    output Sum,Carry
    );
    assign Sum = A^B^Cin;
    assign Carry = (A&B) | Cin & (A^B);
endmodule
```

# truthtable

| Inputs |   |     | Outputs |              |
|--------|---|-----|---------|--------------|
| A      | B | Cin | S (Sum) | Cout (Carry) |
| 0      | 0 | 0   | 0       | 0            |
| 0      | 0 | 1   | 1       | 0            |
| 0      | 1 | 0   | 1       | 0            |
| 0      | 1 | 1   | 0       | 1            |
| 1      | 0 | 0   | 1       | 0            |
| 1      | 0 | 1   | 0       | 1            |
| 1      | 1 | 0   | 0       | 1            |
| 1      | 1 | 1   | 1       | 1            |


# Full adder Testbench (dataflow model)

```verilog
`timescale 1ns / 1ps

module full_adder_tb;

reg A_tb,B_tb,Cin_tb;
wire Sum_tb,Carry_tb;

Full_Adder_DF dut (.A(A_tb), .B(B_tb), .Cin(Cin_tb), .Sum(Sum_tb), .Carry(Carry_tb) );

initial begin

A_tb = 1'b0; B_tb = 1'b0; Cin_tb = 1'b0;
#10 A_tb = 1'b0; B_tb = 1'b0; Cin_tb = 1'b1;
#10 A_tb = 1'b0; B_tb = 1'b1; Cin_tb = 1'b0;
#10 A_tb = 1'b0; B_tb = 1'b1; Cin_tb = 1'b1;
#10 A_tb = 1'b1; B_tb = 1'b0; Cin_tb = 1'b0;
#10 A_tb = 1'b1; B_tb = 1'b0; Cin_tb = 1'b1;
#10 A_tb = 1'b1; B_tb = 1'b1; Cin_tb = 1'b0;
#10 A_tb = 1'b1; B_tb = 1'b1; Cin_tb = 1'b1;
#10 $stop;
end

endmodule

```

## Schematics
![Alt Text](https://i.ibb.co/hgQ01V1/FA.png)

## Simulation
![Alt Text](https://i.ibb.co/6BF4BMC/FA_Sim.png)
