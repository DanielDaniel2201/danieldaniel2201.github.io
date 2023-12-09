---
title: "csapp lab1: datalab"
last_modified_at: 2023-12-09T16:20:02-05:00
categories:
  - Blog
tags:
  - csapp
  - computer system
  - lab
---

# introduction

This lab is bit-centric. There are tons of use of bit-wise operations and it's important to know how to use bit-wise operations to simuate normal operations we use. Review the bit-level operations, two's complement encoding and floating point encoding if necessary.
My solutions at Github can be found [here](https://github.com/DanielDaniel2201/15213-CSAPP-labs/blob/main/datalab-handout/bits.c).

# Integer part

## bitXor

### Hint

Just follow the [truth table](https://en.wikipedia.org/wiki/XOR_gate) of xor.

### Implementation

```c
int bitXor(int x, int y) {
  return (~x & y) | (x & ~y);
}
```

## tmin

### Hint

Be familiar with two's complement encoding.

### Implementation

```c
int tmin(void) {
  return 1 << 31;
}
```

## isTmax

### Hint

Encode tmax and return comparison result. Know how to perform "==" operation.

### Implementation

```c
int isTmax(int x) {
  int min = 1 << 31;
  return !(~min ^ x);
}
```

To perform "==" operation, take advantage of xor property:  a ^ b is equal to zero if a == b, otherwise nonzero.

## allOddBits

### Hint

Encode a all-odd mask for comparison.

### Implementation

```c
int allOddBits(int x) {
  int AAAA = (0b10101010 << 8) + 0b10101010;
  int AAAAAAAA = (AAAA << 16) + AAAA;
  return !(AAAAAAAA ^ (AAAAAAAA & x));
}
```

## Negate

### Hint

Be familiar with two's complement encoding.

### Implementation

```c
int negate(int x) {
  return ~x + 1;
}
```

## isAsciiDigit

### Hint

Compare bits pattern seperately and perform substraction by addition with negation (don't call directly).

### Implementation

```c
int isAsciiDigit(int x) {
  int second_four = !(0b0011 ^ (x >> 4)); // check these bits are identical
  int first_four = 0b1111 & x;
  int neg = ~first_four + 1;
  return second_four & !(((9 + neg) >> 31) ^ 0); // check 9 - first-four bits >= 0 by checking the most significant bit is 0 or not
}
```

## conditional

### Hint

Convert x into 0x00000001 or 0x00000000.

### Implementation

```c
int conditional(int x, int y, int z) {
  x = !(!(x)); // convert it to 0 or 1
  return (y & (x << 31 >> 31)) | (z & ~(x << 31 >> 31)); // convert 0x00000001 to 0xffffffff, 0x00000000 to 0x00000000
}
```

## isLessOrEqual

### Hint

Compare sign first: if different, return by whether x is negative; if the same, use substraction and comparison with 0 (like in `isAsciiDigit`).

### Implementation

```c
int isLessOrEqual(int x, int y) {
  int neg_x = ~x + 1;
  int same_sign = !((x >> 31) ^ (y >> 31));
  int same_sigh_rst = !(((y + neg_x) >> 31) ^ 0); // subtraction & comparison
  int diff_sign = ~same_sign;
  int diff_sign_rst = !(!(x >> 31));
  return ((~diff_sign + 1) & diff_sign_rst) | ((~same_sign + 1) & same_sigh_rst);
}
```

## logicalNeg

### Hint

0 ^ negate(0) = 0 and be aware of that tmin = negate(tmin)

### Implementation

```c
int logicalNeg(int x) {
  int neg = ~x + 1;
  return ((x | neg) >> 31) + 1;
}
```

## howManyBits

### Hint

Bit-wise Binary Search! Very intereting!
Check validity for a certain length by `(var << len >> len) == original_var` and perform if statement without if statement

### Implementation

```c
int howManyBits(int x) {
  int copy = x; // keep the origional value
  int tmp = 16;

  int tmp_plus = tmp + 8; // to expand
  int tmp_minus = tmp + ~8 + 1; // or too shrink
  int eql = !(~(~(x << tmp >> tmp) ^ copy)) << 31 >> 31; // 0b111..11 or 0b000..00 (check validity)
  tmp = (tmp_plus & eql) | (tmp_minus & (~eql)); // bit wise if statement
  
  tmp_plus = tmp + 4;
  tmp_minus = tmp + ~4 + 1;
  eql = !(~(~(x << tmp >> tmp) ^ copy)) << 31 >> 31; // 0b111..11 or 0b000..00
  tmp = (tmp_plus & eql) | (tmp_minus & (~eql));

  tmp_plus = tmp + 2;
  tmp_minus = tmp + ~2 + 1;
  eql = !(~(~(x << tmp >> tmp) ^ copy)) << 31 >> 31; // 0b111..11 or 0b000..00
  tmp = (tmp_plus & eql) | (tmp_minus & (~eql));

  tmp_plus = tmp + 1;
  tmp_minus = tmp + ~0;
  eql = !(~(~(x << tmp >> tmp) ^ copy)) << 31 >> 31; // 0b111..11 or 0b000..00
  tmp = (tmp_plus & eql) | (tmp_minus & (~eql));

  eql = !(~(~(x << tmp >> tmp) ^ copy)) << 31 >> 31; // 0b111..11 or 0b000..00
  int rst = ((32 + ~tmp + 1) & eql) | ((32 + ~tmp + 1 + 1) & (~eql));

  return rst;
}
```

# Floating point part

## floatScale2

### Hint

Seperate different cases: return original `uf` value or return a modifiled value and watch out for sign bit.

### Implementation

```c
unsigned floatScale2(unsigned uf) {
  unsigned expMask = 0b011111111 << 23;
  unsigned fracMask = (1 << 23) - 1; // 0b0 00000000 1111..11
  unsigned signMask = 1 << 31;

  unsigned expField = (expMask & uf) >> 23;
  unsigned expAllOnes = expField == 0b11111111;
  unsigned expAllZeros = expField == 0;
  unsigned fracAllZeros = (fracMask & uf) == 0;

  unsigned isZero = expAllZeros && fracAllZeros;
  unsigned isInf = expAllOnes && fracAllZeros;
  unsigned isNaN = expAllOnes && !fracAllZeros;
  unsigned isNorm = !expAllZeros && !expAllOnes;

  if (isZero || isInf || isNaN) {
    return uf;
  } else if (isNorm) {
    return uf + (1 << 23);
  } else { // is denormalized number
    unsigned isNeg = (signMask & uf);
    return (uf << 1) + isNeg;
  }
}
```

## floatFloat2Int

### Hint

Alse seperate different cases and be familiar about float rounding to integer rules.

### Implementation

```c
int floatFloat2Int(unsigned uf) {
  unsigned fracMask = (1 << 23) - 1; // 0b0 00000000 1111..11
  unsigned expMask = 0b011111111 << 23;
  unsigned signMask = 1 << 31;

  unsigned expField = (expMask & uf) >> 23;
  unsigned expAllOnes = expField == 0b11111111;
  unsigned fracAllZeros = (fracMask & uf) == 0;

  unsigned isInf = expAllOnes && fracAllZeros;
  unsigned isNaN = expAllOnes && !fracAllZeros;

  int E = expField - 127;

  unsigned lessThan1 = E < 0;

  if (lessThan1) {
    return 0;
  } else if (isNaN || isInf || E > 31) {
    return 0x80000000u;
  } else { // is normalized number
    unsigned isNeg = (signMask & uf) != 0;
    int num = 1 << E;
    return isNeg ? -num : num;
  }
}
```

## floatPower2

### Hint

Determine the valid range of numbers and seperate cases in which just use shift operations

### Implementation

```c
unsigned floatPower2(int x) {
  if (x < -23 - 126) { // too small for denorm
    return 0;
  } else if (x > 127) { // too large
    return 0b011111111u << 23;
  } else if (x >= -126) { // norm: 127 >= x >= -126
    unsigned expField = x + 127;
    return expField << 23;
  } else { // denorm:  -126 > x >= -23 - 126
    return 1 << (23 + 126 + x);
  }
}
```
