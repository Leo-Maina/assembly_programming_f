# Multiplication Directory (`03_arithmetic/mul`)

## Program 1: `mul1.asm`

- **Operation:** `100 * 20` (`AL * BL`, result stored in `AX`)
- **Result:** `0x07D0` (`2000` in decimal)
- **EFLAGS:** `CF = 1`, `OF = 1`

### EFLAGS Explanation

- **Carry Flag (CF = 1) and Overflow Flag (OF = 1): Set**
  - The `MUL` instruction performs an unsigned multiplication.
  - Since `AL` is an 8-bit register, multiplying `AL * BL` produces a 16-bit result in `AX`.
  - The result is `2000` (`0x07D0`).
  - The upper 8 bits of `AX` are `AH = 0x07`, which are non-zero.
  - Therefore, the result does not fit in 8 bits, so `CF` and `OF` are set.

- **Sign Flag (SF), Zero Flag (ZF), and Parity Flag (PF): Undefined**
  - These flags are not given a defined value by the `MUL` instruction.
  - Therefore, their values should not be used to determine the result of the multiplication.

---

## Program 2: `mul2.asm`

- **Operation:** `10 * 5` (`AL * BL`, result stored in `AX`)
- **Result:** `0x0032` (`50` in decimal)
- **EFLAGS:** `CF = 0`, `OF = 0`

### EFLAGS Explanation

- **Carry Flag (CF = 0) and Overflow Flag (OF = 0): Cleared**
  - The result of `10 * 5` is `50` (`0x0032`).
  - Since the multiplication is performed using 8-bit operands, the 16-bit result is stored in `AX`.
  - The upper 8 bits are `AH = 0x00`.
  - Because the upper half of the result is zero, the result fits within 8 bits.
  - Therefore, `CF` and `OF` are cleared.

- **Sign Flag (SF), Zero Flag (ZF), and Parity Flag (PF): Undefined**
  - These flags are not given a defined value by the `MUL` instruction.
  - Their values should not be used to determine the result of the multiplication.