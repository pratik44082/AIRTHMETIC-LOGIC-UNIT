// Basic ALU supporting Addition, Subtraction, AND, OR, and NOT operations
module basic_alu(
    input  [3:0] A,        // 4-bit input A
    input  [3:0] B,        // 4-bit input B
    input  [2:0] opcode,   // 3-bit opcode to select operation
    output reg [3:0] result, // 4-bit ALU result
    output reg carry_out   // Carry out for addition and subtraction
);

// Opcodes
// 000 - Addition
// 001 - Subtraction
// 010 - AND
// 011 - OR
// 100 - NOT (applies only to A, ignores B)

always @(*) begin
    carry_out = 0;
    case(opcode)
        3'b000: {carry_out, result} = A + B;       // Addition
        3'b001: {carry_out, result} = A - B;       // Subtraction (carry_out as borrow)
        3'b010: result = A & B;                    // AND
        3'b011: result = A | B;                    // OR
        3'b100: result = ~A;                       // NOT (on A only)
        default: result = 4'b0000;
    endcase
end

endmodule# AIRTHMETIC-LOGIC-UNIT
