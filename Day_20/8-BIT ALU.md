# 8-BIT ALU:

An 8-bit ALU is a core component of a processor. It handles arithmetic and logic operations on two 8-bit inputs (Operand1 and Operand2) using an opcode to determine the operation. The ALU produces a 16-bit Result and sets flags for carry-out (flagC) and zero result (flagZ).

```verilog
module ALU_8bit (
  input [2:0] operation,
  input [7:0] operand_A,
  input [7:0] operand_B,
  output reg [15:0] result = 16'b0,
  output reg carry_flag = 1'b0,
  output reg zero_flag = 1'b0
);

parameter [2:0] ADD = 3'b000,
               SUB = 3'b001,
               MUL = 3'b010,
               AND = 3'b011,
               OR  = 3'b100,
               NAND = 3'b101,
               NOR  = 3'b110,
               XOR  = 3'b111;

always @ (operation or operand_A or operand_B)
begin
  case (operation)
    ADD: begin
      result = operand_A + operand_B;
      carry_flag = result[8];
      zero_flag = (result == 16'b0);
    end

    SUB: begin
      result = operand_A - operand_B;
      carry_flag = result[8];
      zero_flag = (result == 16'b0);
    end

    MUL: begin
      result = operand_A * operand_B;
      zero_flag = (result == 16'b0);
    end

    AND: begin
      result = operand_A & operand_B;
      zero_flag = (result == 16'b0);
    end

    OR: begin
      result = operand_A | operand_B;
      zero_flag = (result == 16'b0);
    end

    NAND: begin
      result = ~(operand_A & operand_B);
      zero_flag = (result == 16'b0);
    end

    NOR: begin
      result = ~(operand_A | operand_B);
      zero_flag = (result == 16'b0);
    end

    XOR: begin
      result = operand_A ^ operand_B;
      zero_flag = (result == 16'b0);
    end

    default: begin
      result = 16'b0;
      carry_flag = 1'b0;
      zero_flag = 1'b0;
    end
  endcase
end

endmodule
```

# 8-Bit ALU Test Bench:

```verilog
`timescale 1ns / 1ps

module ALU_8bit_tb;

  // Inputs
  reg [2:0] operation;
  reg [7:0] operand_A;
  reg [7:0] operand_B;

  // Outputs
  wire [15:0] result;
  wire carry_flag;
  wire zero_flag;

  ALU_8bit dut (
    .operation(operation), .operand_A(operand_A), .operand_B(operand_B),
    .result(result), .carry_flag(carry_flag), .zero_flag(zero_flag) );

  reg clk = 0;
  always begin
    #5 clk = ~clk;
  end

  initial begin
    // Test ADD operation
    operation = 3'b000;
    operand_A = 8'b11011010;
    operand_B = 8'b00100111;
    #10;
    $display("ADD Result: %b, Carry: %b, Zero: %b", result, carry_flag, zero_flag);

    // Test SUB operation
    operation = 3'b001;
    operand_A = 8'b11011010;
    operand_B = 8'b00100111;
    #10;
    $display("SUB Result: %b, Carry: %b, Zero: %b", result, carry_flag, zero_flag);

    // Test MUL operation
    operation = 3'b010;
    operand_A = 8'b11011010;
    operand_B = 8'b00100111;
    #10;
    $display("MUL Result: %b, Zero: %b", result, zero_flag);

    // Test AND operation
    operation = 3'b011;
    operand_A = 8'b11011010;
    operand_B = 8'b00100111;
    #10;
    $display("AND Result: %b, Zero: %b", result, zero_flag);

    // Test OR operation
    operation = 3'b100;
    operand_A = 8'b11011010;
    operand_B = 8'b00100111;
    #10;
    $display("OR Result: %b, Zero: %b", result, zero_flag);

    // Test NAND operation
    operation = 3'b101;
    operand_A = 8'b11011010;
    operand_B = 8'b00100111;
    #10;
    $display("NAND Result: %b, Zero: %b", result, zero_flag);

    // Test NOR operation
    operation = 3'b110;
    operand_A = 8'b11011010;
    operand_B = 8'b00100111;
    #10;
    $display("NOR Result: %b, Zero: %b", result, zero_flag);

    // Test XOR operation
    operation = 3'b111;
    operand_A = 8'b11011010;
    operand_B = 8'b00100111;
    #10;
    $display("XOR Result: %b, Zero: %b", result, zero_flag);

    $finish;
  end

endmodule
```

## Schematics
![Alt Text](https://i.ibb.co/6Y3KHSK/alu.png)

## Simulation
![Alt Text](https://i.ibb.co/Q96XcXx/alu-tb.png)
