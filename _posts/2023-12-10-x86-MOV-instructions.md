---
title: "x86 MOV/MOVS/MOVZ instruction class"
last_modified_at: 2023-12-10T16:20:02-05:00
categories:
  - Blog
tags:
  - csapp
  - computer system
  - notes
  - assembly
---

# Introduction

Among the most heavily used instructions are the data movement instructions, which copy data from one location to another. In this blog several simplest data movement instructions (including `MOV`, `MOVS` and `MOVZ` instruction classes) will be introduced.

# `MOV` class

`MOV` instructions copy data from a source location to a destination location, without any transformation.

## The instructions

The following instructions differ primarily in that they operate on data of different sizes

|Instruction|Description|
|---|---|
|movb|move byte|
|movw|move word|
|movl|move double word|
|movq|move quad word|
|movabsq|move absolute quad word|

Usage: `MOV S, D`\
Effect: the data stored in S is copied to D without any transformation.

Example:\
Instruction `movl $0x4050, %eax` copies the constant data 0x4050 into the register `%eax`.

## Elaboration

### Operands

- Operands for `MOV` instructions can be an ***immediate*** value (a constant value like 0, 1, 2), a value stored in a ***register*** or a value stored in ***memory***.
- However there are no 3 * 3 = 9 matchings. First, it doesn't make sense using immediate as destination( i.e. putting a value in a constant). And it's also ***prohibited to set both operands referred to memory*** locations, which actually requires 2 operations: move data from source memory to a register and move this data from register to destination memory.
- Five valid formats are as follows:
  |source|destination|
  |---|---|
  |Immediate|Register|
  |Immediate|Memory|
  |Register|Register|
  |Register|Memory|
  |Memory|Register|

- Register operands size ***must match*** the size designated by the instruction suffix(`b`,`w`,`l` or `q`). For example:
  ```asm
  movl $0x4050, %eax       // Immediate -- Register 4 bytes
  movw %dp, %sp            // Register -- Register 2 byter
  movq %rax, -12(%rbp)     // Register -- Memory 8 bytes
  ```

### Special cases

- The last instruction `movabsq` is for dealing with ***64-bit immediate data***.\
The `movq` instruction can move 4 words or 64 bits amount of data but when it comes to operate on an immediate, `movq` can only deal with an immediate represented as 32-bit two's complement number which later will be sign-extended to 64-bit value.\
`movabsq` can directly take in a 64-bit immediate value as its source operand but can only have a register as a destination.

- For most cases, the `MOV` instructions will only update the specific register bytes or memory locations indicated by the instruction suffix, leaving the rest unchanged.\
The only exception is when ***`movl`*** has a register as destination, it will also ***set the upper 4 bytes of the register to 0***.\
Here is an example:\
  ```asm
  movabsq $0x0011223344556677, %rax    // %rax = 0011223344556677
  movb $-1 %al                         // %rax = 00112233445566FF
  movw $-1 %al                         // %rax = 001122334455FFFF
  movl $-1 %al                         // %rax = 00000000FFFFFFFF, note it's movl instruction
  movq $-1 %al                         // %rax = FFFFFFFFFFFFFFFF
  ```

# `MOVZ`, `MOVS` class

There two class of instructions have a similar function but they are useful when copying a smaller source value to a larger destination.

## The instructions

The following instructions differ primarily in the way they extend the sourcs value and the data size of source value and destination location.

|`MOVZ` Instruction|Description|
|---|---|
|movzbw|move zero-extended byte to word|
|movzbl|move zero-extended byte to double word|
|movzwl|move zero-extended word to double word|
|movzbq|move zero-extended byte to quad word|
|movzwq|move zero-extended word to quad word|

|`MOVS` Instruction|Description|
|---|---|
|movsbw|move sigh-extended byte to word|
|movsbl|move sigh-extended byte to double word|
|movswl|move sigh-extended word to double word|
|movsbq|move sigh-extended byte to quad word|
|movswq|move sigh-extended word to quad word|
|movslq|move sigh-extended double word to quad word|

## Elaboration

- Instructions in `MOVZ` class fill out the remaining bytes of destination with zeros.
- Instructions in `MOVS` class fill out the remaining bytes of destination with the most significant bit of the source operand.
- Note that source operand can only be ***register or memory address*** and the destination can only be a ***register***.
- You may wonder why there doesn't exit an instruction in `MOVZ` class that performs zero-extension to a double word source value to a quad word destination. Well, this operation can be achieved by utilizing the function `movl` with a register as destination location, taking advantage that this operation fills the upper 4 bytes of the destination resgiter with zeros