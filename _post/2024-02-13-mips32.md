# This project creact for sherlongqu learn MIPS32 architecture

I will use a game(Turing Complete) to complete relevant practices.

This is some documents of the project.

## Register:

| Name    | Number | Use                                                   |
| ------- | ------ | ----------------------------------------------------- |
| $zero   | 0      | constant 0                                            |
| $at     | 1      | assembler temporary                                   |
| $v0-$v1 | 2-3    | values for function returns and expression evaluation |
| $a0-$a3 | 4-7    | function arguments                                    |
| $t0-$t7 | 8-15   | temporaries                                           |
| $s0-$s7 | 16-23  | saved temporaries                                     |
| $t8-$t9 | 24-25  | temporaries                                           |
| $k0-$k1 | 26-27  | reserved for OS kernel                                |
| $gp     | 28     | global pointer                                        |
| $sp     | 29     | stack pointer                                         |
| $fp     | 30     | frame pointer                                         |
| $ra     | 31     | return address                                        |

## MIPS Opcode Reference

### Format Code

| Type | format(bits)(32) |             |       |               |          |          |
| ---- | ---------------- | ----------- | ----- | ------------- | -------- | -------- |
| R    | opcode(6)        | rs(5)       | rt(5) | rd(5)         | shamt(5) | funct(6) |
| I    | opcode(6)        | rs(5)       | rt(5) | immediate(16) | <--      | <--      |
| J    | opcode(6)        | address(26) | <--   | <--           | <--      | <--      |


### Code Reference

| OPcode                    | Name                            | Action                        | Fields                             |
| ------------------------- | ------------------------------- | ----------------------------- | ---------------------------------- |
| **Arithmetic Logic Unit** |                                 |                               |                                    |
| ADD rd,rs,rt              | Add                             | rd=rs+rt                      | 000000 rs rt rd 00000 100000       |
| ADDI rt,rs,imm            | Add Immediate                   | rt=rs+imm                     | 001000 rs rt imm                   |
| ADDIU rt,rs,imm           | Add Immediate Unsigned          | rt=rs+imm                     | 001001 rs rt imm                   |
| ADDU rd,rs,rt             | Add Unsigned                    | rd=rs+rt                      | 000000 rs rt rd 00000 100001       |
| AND rd,rs,rt              | And                             | rd=rs$\land$rt                | 000000 rs rt rd 00000 100100       |
| ANDI rt,rs,imm            | And Immediate                   | rt=rs$\land$imm               | 001100 rs rt imm                   |
| LUI rt,imm                | Load Upper Immediate            | rt=imm<<16                    | 001111 rs rt imm                   |
| NOR rd,rs,rt              | Nor                             | rd=$\neg$(rs$\lor$rt)         | 000000 rs rt rd 00000 100111       |
| OR rd,rs,rt               | Or                              | rd=$\lor$rt                   | 000000 rs rt rd 00000 100101       |
| ORI rt,rs,imm             | Or Immediate                    | rt=rs$\lor$imm                | 001101 rs rt imm                   |
| SLT rd,rs,rt              | Set On Less Than                | rd=rs<rt                      | 000000 rs rt rd 00000 101010       |
| SLTI rt,rs,imm            | Set On Less Than Immediate      | rt=rs<imm                     | 001010 rs rt imm                   |
| SLTIU rt,rs,imm           | Set On Less Immediate Unsigned  | rt=rs<imm                     | 001011 rs rt imm                   |
| SLTU rd,rs,rt             | Set On Less Than Unsigned       | rd=rs<rt                      | 000000 rs rt rd 00000 101011       |
| SUB rd,rs,rt              | Subtract                        | rd-rs-rt                      | 000000 rs rt rd 00000 100010       |
| SUBU rd,rs,rt             | Subtract Unsigned               | rd=rs-rt                      | 000000 rs rt rd 00000 100011       |
| XOR rd,rs,rt              | Exclusive Or                    | rd=rs$\oplus$rt               | 000000 rs rt rd 00000 100110       |
| XORI rt,rs,imm            | Exclusive Or Immediate          | rt=rs$\oplus$imm              | 001110 rs rt imm                   |
| **Shifter**               |                                 |                               |                                    |
| SLL rd,rt,sa              | Shift Left Logical              | rd=rt<<sa                     | 000000 rs rt rd sa 000000          |
| SLLV rd,rt,rs             | Shift Left Logical Variable     | rd=rt<<rs                     | 000000 rs rt rd 00000 000100       |
| SRA rd,rt sa              | Shift Right Arithmetic          | rd=rt>>sa                     | 000000 00000 rt rd sa 000011       |
| SRAV rd,rt,rs             | Shift Right Arithmetic Variable | rd=rt>>rs                     | 000000 rs rt rd 00000 000111       |
| SRL rd,rt,sa              | Shift Right Logical             | rd=rt>>sa                     | 000000 rs rt rd sa 000010          |
| SRLV rd,rt,rs             | Shift Right Logical Variable    | rd=rt>>rs                     | 000000 rs rt rd 00000 000110       |
| **Multiply**              |                                 |                               |                                    |
| DIV rs,rt                 | Divide                          | HI=rs%rt;LO=rs/rt             | 000000 rs rt 0000000000 011010     |
| DIVU rs,rt                | Divide Unsigned                 | HI=rs%rt;LO=rs/rt             | 000000 rs rt 0000000000 011011     |
| MFHI rd                   | Move From HI                    | rd=HI                         | 000000 0000000000 rd 00000 010000  |
| MFLO rd                   | Move From LO                    | rd=LO                         | 000000 0000000000 rd 00000 010010  |
| MTHI rs                   | Move To HI                      | HI=rs                         | 000000 rs 000000000000000 010001   |
| MTLO rd                   | Move To LO                      | LO=rs                         | 000000 rs 000000000000000 010011   |
| MULT rs,rt                | Multiply                        | HI,LO=rs*rt                   | 000000 rs rt 0000000000 011000     |
| MULTU rs,rt               | Multiply Unsigned               | HI,LO=rs*rt                   | 000000 rs rt 0000000000 011001     |
| **Branch**                |                                 |                               |                                    |
| BEQ rs,rt,offset          | Branch On Equal                 | if(rs==rt) pc+=offset*4       | 000100 rs rt offset                |
| BGEZ rs,offset            | Branch On >= 0                  | if(rs>=0) pc+=offset*4        | 000001 rs 00001 offset             |
| BGEZAL rs,offset          | Branch On >= 0 And Link         | r31=pc;if(rs>=0) pc+=offset*4 | 000001 rs 10001 offset             |
| BGTZ rs,offset            | Branch On > 0                   | if(rs>0) pc+=offset*4         | 000111 rs 00000 offset             |
| BLEZ rs,offset            | Branch On                       | if(rs<=0) pc+=offset*4        | 000110 rs 00000 offset             |
| BLTZ rs,offset            | Branch On < 0                   | if(rs<0) pc+=offset*4         | 000001 rs 00000 offset             |
| BLTZAL rs,offset          | Branch On < 0 And Link          | r31=pc;if(rs<0)pc+=offset*4   | 000001 rs 10000 offset             |
| BNE rs,rt offset          | Branch On Not Equal             | if(rs!=rt) pc+=offset*4       | 000101 rs rt offset                |
| BREAK                     | Breakpoint                      | epc=pc;pc=0x3c                | 000000 code 001101                 |
| J target                  | Jump                            | pc=pc_upper$\lor$(target<<2)  | 000010 target                      |
| JAL target                | Jump And Link                   | r31=pc;pc=target<<2           | 000011 target                      |
| JALR rs                   | Jump And Link Register          | rd=pc;pc=rs                   | 000000 rs 00000 rd 00000 001001    |
| JR rs                     | Jump Register                   | pc=rs                         | 000000 rs 000000000000000 001000   |
| MFC0 rt,rd                | Move From Coprocessor           | rt=CPR[0,rd]                  | 010000 00000 rt rd 00000000000     |
| MTC0 rt,rd                | Move To Coprocessor             | CPR[0,rd]=rt                  | 010000 00100 rt rd 00000000000     |
| SYSCALL                   | System Call                     | epc=pc;pc=0x3c                | 000000 00000000000000000000 001100 |
| **Memory Access**         |                                 |                               |                                    |
| LB rt,offset(rs)          | Load Byte                       | rt=\*(char\*)(offset+rs)      | 100000 rs rt offset                |
| LBU rt,offset(rs)         | Load Byte Unsigned              | rt=\*(Uchar\*)(offset+rs)     | 100100 rs rt offset                |
| LH rt,offset(rs)          | Load Halfword                   | rt=\*(short\*)(offset+rs)     | 100001 rs rt offset                |
| LHU rt,offset(rs)         | Load Halfword Unsigned          | rt=\*(Ushort\*)(offset+rs)    | 100101 rs rt offset                |
| LW rt,offset(rs)          | Load Word                       | rt=\*(int\*)(offset+rs)       | 100011 rs rt offset                |
| SB rt,offset(rs)          | Store Byte                      | \*(char\*)(offset+rs)=rt      | 101000 rs rt offset                |
| SH rt,offset(rs)          | Store Halfword                  | \*(short\*)(offset+rs)=rt     | 101001 rs rt offset                |
| SW rt,offset(rs)          | Store Word                      | \*(int\*)(offset+rs)=rt       | 101011 rs rt offset                |