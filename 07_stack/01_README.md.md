# 01.asm — Basic PUSH and POP Operations

## 1. Objective
This program demonstrates how the stack stores values and retrieves them in last-in, first-out (LIFO) order.

## 2. Execution flow
1. Execution begins at `_start`.
2. `mov rax, 100` places `100` in `RAX`.
3. `mov rbx, 200` places `200` in `RBX`.
4. `push rax` places the value `100` on the stack.
5. `push rbx` places `200` above `100`.
6. `nop` performs no operation.
7. `pop rcx` removes the top value, `200`, and places it in `RCX`.
8. `pop rdx` removes the next value, `100`, and places it in `RDX`.
9. The program sets up Linux's 64-bit exit system call (`RAX = 60`, `RDI = 0`) and executes `syscall`.

## 3. Stack pointer changes
Let the initial stack pointer be `S`. In this 64-bit program, each push or pop of a 64-bit register changes `RSP` by 8 bytes.

| Instruction | RSP | Stack top |
|---|---:|---|
| Before pushes | `S` | Existing process stack |
| `push rax` | `S - 8` | `100` |
| `push rbx` | `S - 16` | `200` |
| `pop rcx` | `S - 8` | `100` |
| `pop rdx` | `S` | Previous stack contents |

The stack grows toward lower memory addresses. A push decreases `RSP`; a pop increases it.

## 4. Stack layout
After both pushes, from the top of the stack downward:

```text
RSP -> 200  (pushed from RBX)
       100  (pushed from RAX)
```

The first value popped is `200`, because it was pushed last. The second is `100`.

## 5. Testing observations
The program was assembled, linked and run using:

```bash
nasm -f elf64 01.asm -o 01.o
ld 01.o -o 01
./01
echo $?
```

Observed result: no text was printed, and the exit status was `0`. This is expected because the program contains no output instruction; it only performs register and stack operations before exiting successfully.

## 6. Conclusion
This program demonstrates the stack's LIFO behaviour. Values are retrieved in the reverse order in which they were pushed. After the two pops, `RSP` returns to its original value.
