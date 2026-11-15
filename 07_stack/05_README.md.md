# 05.asm — 64-bit PUSH and POP with Larger Values

## 1. Objective
This program demonstrates 64-bit stack operations using two different values and shows that values are popped in last-in, first-out (LIFO) order.

## 2. Execution flow
1. Execution begins at `_start`.
2. `mov rax, 0x1122334455667788` loads the 64-bit value `0x1122334455667788` into `RAX`.
3. `mov rbx, 0x1234` loads `0x1234` into `RBX`.
4. `push rax` pushes the 64-bit value from `RAX` onto the stack.
5. `push rbx` pushes `0x1234` on top of the first value.
6. `nop` performs no operation.
7. `pop rbx` removes the top value, `0x1234`, and restores it to `RBX`.
8. `pop rax` removes the next value, `0x1122334455667788`, and restores it to `RAX`.
9. The program sets up and invokes the Linux 64-bit exit system call.

## 3. Stack pointer changes
Let the initial `RSP` be `S`. Each push or pop of a 64-bit register changes `RSP` by 8 bytes.

| Instruction | RSP | Stack top |
|---|---:|---|
| Before pushes | `S` | Existing process stack |
| `push rax` | `S - 8` | `0x1122334455667788` |
| `push rbx` | `S - 16` | `0x1234` |
| `pop rbx` | `S - 8` | First value remains |
| `pop rax` | `S` | Original stack position restored |

## 4. Stack layout
After both pushes:

```text
RSP -> 0x0000000000001234
       0x1122334455667788
```

The top value is `0x1234`, so it is popped first into `RBX`. The earlier value is then popped into `RAX`.

## 5. Testing observations
The program was assembled, linked and run using:

```bash
nasm -f elf64 05.asm -o 05.o
ld 05.o -o 05
./05
echo $?
```

Observed result: no text was printed, and the exit status was `0`. The program restores the two values through the matching pops but does not display them.

## 6. Conclusion
This program reinforces LIFO behaviour using 64-bit values. Each push decreases `RSP` by 8 bytes, and each pop increases it by 8 bytes. After both values are popped, `RSP` returns to its starting position.
