# 02.asm — Stack Frames and Local Variables

## 1. Objective
This program demonstrates how a procedure uses the stack to save a base pointer and reserve space for local variables.

## 2. Execution flow
1. Execution begins at `_start`.
2. `call work` transfers execution to `work` and pushes the return address onto the stack.
3. `push rbp` saves the caller's base pointer.
4. `mov rbp, rsp` establishes a base pointer for the procedure's stack frame.
5. `sub rsp, 16` reserves 16 bytes for local storage.
6. `mov qword [rbp - 8], 100` stores `100` in an 8-byte local-variable slot.
7. `mov qword [rbp - 16], 200` stores `200` in another 8-byte slot.
8. The two `mov` instructions read those values into `RAX` and `RBX`.
9. `nop` does nothing.
10. `mov rsp, rbp` releases the local-variable area by restoring the stack pointer to the saved frame position.
11. `pop rbp` restores the caller's base pointer.
12. `ret` removes the return address from the stack and returns to the instruction after `call work`.
13. `_start` exits using the 64-bit Linux exit system call.

## 3. Stack pointer changes
Let the initial `RSP` before `call work` be `S`.

| Instruction | RSP | Explanation |
|---|---:|---|
| Before `call` | `S` | Initial stack pointer |
| `call work` | `S - 8` | 8-byte return address is pushed |
| `push rbp` | `S - 16` | Previous `RBP` is saved |
| `mov rbp, rsp` | `S - 16` | `RBP` marks the stack frame |
| `sub rsp, 16` | `S - 32` | 16 bytes reserved for locals |
| `mov rsp, rbp` | `S - 16` | Local storage is released |
| `pop rbp` | `S - 8` | Saved `RBP` is restored |
| `ret` | `S` | Return address is removed |

`mov`, `nop` and the local-variable reads/writes do not themselves change `RSP`.

## 4. Stack frame layout
After the procedure reserves its local storage:

```text
Higher addresses
[return address]       <- originally pushed by CALL
[saved RBP]            <- RBP points here
[local variable: 100]  <- [RBP - 8]
[local variable: 200]  <- [RBP - 16] <- RSP
Lower addresses
```

The stack grows toward lower addresses. The local variables are accessed using negative offsets from `RBP`, so their locations remain easy to refer to while `RSP` is used for stack allocation.

## 5. Testing observations
The program was assembled, linked and run using:

```bash
nasm -f elf64 02.asm -o 02.o
ld 02.o -o 02
./02
echo $?
```

Observed result: no text was printed, and the exit status was `0`. The program performs its procedure and stack-frame operations but does not contain code to display the local-variable values.

## 6. Conclusion
This program demonstrates a typical stack-frame pattern: save the old base pointer, establish a new frame, reserve local storage, use the local variables, release the storage, restore the old base pointer and return.
