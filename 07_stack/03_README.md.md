# 03.asm — CALL and RET

## 1. Objective
This program demonstrates how `CALL` and `RET` use the stack to support a procedure call and return.

## 2. Execution flow
1. Execution begins at `_start`.
2. `mov rax, 10` places `10` in `RAX`.
3. `call calculate` pushes the address of `after_call` onto the stack and transfers execution to `calculate`.
4. `add rax, 5` adds 5 to the value in `RAX`, making it `15`.
5. `ret` takes the saved return address from the top of the stack and transfers execution back to `after_call`.
6. At `after_call`, the comment indicates that `RAX` should now be `15`; `nop` performs no operation.
7. The program then sets `RAX` to 60, clears `RDI`, and invokes the Linux 64-bit exit system call.

## 3. Stack pointer changes
Let the initial `RSP` be `S`. A near `CALL` in 64-bit mode pushes an 8-byte return address.

| Instruction | RSP | Explanation |
|---|---:|---|
| Before `call calculate` | `S` | Initial stack pointer |
| `call calculate` | `S - 8` | Return address is pushed |
| `add rax, 5` | `S - 8` | No stack-pointer change |
| `ret` | `S` | Return address is popped |

The arithmetic instruction changes `RAX`, not `RSP`.

## 4. Stack layout
While `calculate` is executing:

```text
RSP -> address of after_call
```

The return address is temporary stack data. `RET` uses it to resume execution at the correct point in `_start`.

## 5. Testing observations
The program was assembled, linked and run using:

```bash
nasm -f elf64 03.asm -o 03.o
ld 03.o -o 03
./03
echo $?
```

Observed result: no text was printed, and the exit status was `0`. The program does not print the calculated result. The source comment states that `RAX` should be `15` immediately after the procedure returns; the later `mov rax, 60` replaces that value to prepare for exit.

## 6. Conclusion
`CALL` uses the stack to save where execution must resume. `RET` retrieves that return address. This allows a procedure to run and then return to the instruction following the call.
