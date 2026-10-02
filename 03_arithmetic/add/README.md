# Addition Directory (`03_arithmetic/add`)

## Program 1: `add1.asm`
- **Operation:** `120 + 10` (`AL = 120 + 10`)
- **Result:** `0x82` (`10000010b`, or `-126` in two's complement)
- **EFLAGS Observed:** `[ AF SF PF IF ]`

### EFLAGS Explanation
- **Carry Flag (CF = 0): Cleared**
  - *Why:* Unsigned sum (120 + 10 = 130) fits within an 8-bit unsigned integer (0 to 255), so no carry out occurred.
- **Overflow Flag (OF = 1): Set**
  - *Why:* Adding two positive signed numbers (+120 and +10) produced a negative signed result (-126), indicating signed overflow (range is -128 to +127).
- **Sign Flag (SF = 1): Set**
  - *Why:* The Most Significant Bit (MSB, bit 7) of `10000010b` is `1`.
- **Zero Flag (ZF = 0): Cleared**
  - *Why:* The result (130 / `0x82`) is non-zero.
- **Parity Flag (PF = 1): Set**
  - *Why:* The result byte `10000010b` contains exactly two `1` bits (an even number of bits).

---

## Program 2: `add2.asm`
- **Operation:** `32000 + 500` (`AX = 32000 + 500`)
- **Result:** `0x7F24` (`32500` in decimal)
- **EFLAGS Observed:** `[ IF ]` (all condition flags cleared)

### EFLAGS Explanation
- **Carry Flag (CF = 0): Cleared**
  - *Why:* 32500 easily fits within a 16-bit unsigned integer (0 to 65535).
- **Overflow Flag (OF = 0): Cleared**
  - *Why:* 32500 fits within a 16-bit signed integer range (-32768 to +32767).
- **Sign Flag (SF = 0): Cleared**
  - *Why:* The MSB (bit 15) of `0x7F24` (`0111111100100100b`) is `0` (positive).
- **Zero Flag (ZF = 0): Cleared**
  - *Why:* The result is non-zero.
- **Parity Flag (PF = 1): Set**
  - *Why:* The lower byte `0x24` (`00100100b`) contains two `1` bits (even count).