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


module alu #(
    parameter WIDTH = 8
)(
    input  [WIDTH-1:0] a,
    input  [WIDTH-1:0] b,
    input  [2:0]       op,
    output reg [WIDTH-1:0] result,
    output reg          zero,
    output reg          carry,
    output reg          overflow
);
    always @(*) begin
        case(op)
            3'b000: {carry, result} = a + b;                         // Addition
            3'b001: {carry, result} = a - b;                         // Subtraction
            3'b010: result = a & b;                                  // AND
            3'b011: result = a | b;                                  // OR
            3'b100: result = a ^ b;                                  // XOR
            3'b101: result = ~(a | b);                               // NOR
            3'b110: result = (a < b) ? 1 : 0;                        // Set Less Than
            default: result = 0;
        endcase
        // Set zero flag
        zero = (result == 0);
        // Set overflow flag for add/sub
        if (op == 3'b000) // Add
            overflow = (~a[WIDTH-1] & ~b[WIDTH-1] & result[WIDTH-1]) | (a[WIDTH-1] & b[WIDTH-1] & ~result[WIDTH-1]);
        else if (op == 3'b001) // Sub
            overflow = (~a[WIDTH-1] & b[WIDTH-1] & result[WIDTH-1]) | (a[WIDTH-1] & ~b[WIDTH-1] & ~result[WIDTH-1]);
        else
            overflow = 0;
        if (!(op == 3'b000 || op == 3'b001)) carry = 0;
    end
endmodule

