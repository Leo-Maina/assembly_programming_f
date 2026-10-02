# Division Directory (`03_arithmetic/div`)

## Program 1: `div1.asm`
- **Operation:** `100 / 3` (`AX = 100`, `BL = 3`, `DIV BL`)
- **Result:** `AL = 33` (`0x21`, Quotient), `AH = 1` (`0x01`, Remainder)
- **EFLAGS Observed:** `[ IF ]` (Status flags remain undefined or unchanged)

### EFLAGS Explanation
- **Status Flags (CF, OF, SF, ZF, AF, PF): Undefined**
  - *Why:* According to the x86 Intel architecture specification, unsigned division (`DIV`) and signed division (`IDIV`) leave all condition status flags undefined. They do not convey valid post-execution status.

---

## Program 2: `div2.asm`
- **Operation:** `500 / 10` (`AX = 500`, `BX = 10`, `DIV BX`)
- **Result:** `AX = 50` (`0x0032`, Quotient), `DX = 0` (`0x0000`, Remainder)
- **EFLAGS Observed:** `[ IF ]` (Status flags remain undefined or unchanged)

### EFLAGS Explanation
- **Status Flags (CF, OF, SF, ZF, AF, PF): Undefined**
  - *Why:* Just like 8-bit division, 16-bit division leaves status flags undefined upon completion.