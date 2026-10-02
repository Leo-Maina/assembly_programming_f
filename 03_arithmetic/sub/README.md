# Subtraction (SUB) and EFLAGS

## Program 1: sub1.asm

**Operation:** 50 − 80

* The values are stored in 8-bit registers.
* The result is 226 (`0xE2`) in unsigned representation, equivalent to −30 in signed 8-bit representation.

### Flags after SUB

| Flag | State       | Explanation                                                             |
| ---- | ----------- | ----------------------------------------------------------------------- |
| CF   | Set (1)     | A borrow is required because 50 is less than 80 in unsigned arithmetic. |
| ZF   | Cleared (0) | The result is not zero.                                                 |
| SF   | Set (1)     | The most significant bit of `0xE2` is 1.                                |
| OF   | Cleared (0) | −30 fits within the signed 8-bit range.                                 |
| PF   | Set (1)     | `0xE2` has four set bits, giving even parity.                           |

## Program 2: sub2.asm

**Operation:** 1000 − 2000

* The values are stored in 16-bit registers.
* The result is −1000, represented as `0xFC18` in 16-bit arithmetic.

### Flags after SUB

| Flag | State       | Explanation                                               |
| ---- | ----------- | --------------------------------------------------------- |
| CF   | Set (1)     | An unsigned borrow is required.                           |
| ZF   | Cleared (0) | The result is not zero.                                   |
| SF   | Set (1)     | The most significant bit of `0xFC18` is 1.                |
| OF   | Cleared (0) | −1000 fits within the signed 16-bit range.                |
| PF   | Set (1)     | The low byte `0x18` has two set bits, giving even parity. |

**Note:** Inspect flags immediately after the subtraction instruction because later arithmetic or logical instructions can change them.
