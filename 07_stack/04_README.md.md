# 04.asm — 32-bit Procedure and Stack Frame

## 1. Objective
This program demonstrates a procedure using a 32-bit stack frame, with `ESP` as the stack pointer and `EBP` as the frame/base pointer.

## 2. Execution flow
1. `BITS 32` tells NASM to assemble 32-bit instructions.
2. Execution begins at `_start`.
3. `call work` pushes the return address onto the 32-bit stack and transfers execution to `work`.
4. `push ebp` saves the previous base pointer.
5. `mov ebp, esp` establishes the procedure's stack-frame base.
6. `sub esp, 8` reserves 8 bytes for two 4-byte local variables.
7. `mov dword [ebp - 4], 100` stores `100` in the first local-variable slot.
8. `mov dword [ebp - 8], 200` stores `200` in the second local-variable slot.
9. The values are read into `EAX` and `EBX`.
10. `nop` performs no operation.
11. `mov esp, ebp` releases the local-variable space.
12. `pop ebp` restores the previous base pointer.
13. `ret` pops the return address and returns to `_start`.
14. `_start` exits using the Linux i386 exit system call (`EAX = 1`, `EBX = 0`, `int 0x80`).

## 3. Stack pointer changes
Let the initial `ESP` before `call work` be `S`. In this 32-bit program, each stack item involved here is 4 bytes.

| Instruction | ESP | Explanation |
|---|---:|---|
| Before `call` | `S` | Initial stack pointer |
| `call work` | `S - 4` | 4-byte return address is pushed |
| `push ebp` | `S - 8` | Previous `EBP` is saved |
| `mov ebp, esp` | `S - 8` | `EBP` marks the frame |
| `sub esp, 8` | `S - 16` | Space for two local variables |
| `mov esp, ebp` | `S - 8` | Local-variable space is released |
| `pop ebp` | `S - 4` | Previous `EBP` is restored |
| `ret` | `S` | Return address is popped |

## 4. Stack frame layout
After `sub esp, 8`:

```text
Higher addresses
[return address]        <- pushed by CALL
[saved EBP]             <- EBP points here
[100]                   <- [EBP - 4]
[200]                   <- [EBP - 8] <- ESP
Lower addresses
```

The stack grows toward lower memory addresses. `EBP` remains the frame reference while `ESP` moves down to reserve local storage.

## 5. Testing observations
The first test used 64-bit output format and linking commands:

```bash
nasm -f elf64 04.asm -o 04.o
ld 04.o -o 04
./04
```

Observed result: `Segmentation fault` and exit status `139`. The source is explicitly 32-bit (`BITS 32`) and uses the i386 `int 0x80` system-call interface, so it must be assembled and linked as a 32-bit program.

The corrected test was:

```bash
nasm -f elf32 04.asm -o 04.o
ld -m elf_i386 04.o -o 04
./04
echo $?
```

Observed result: no text was printed, and the exit status was `0`. This is expected because the program does not contain output instructions.

## 6. Conclusion
This program demonstrates a 32-bit procedure stack frame. `CALL` saves the return address, `PUSH EBP` saves the previous frame pointer, `ESP` reserves space for local variables, and the epilogue restores the stack before `RET` returns to the caller. Correct 32-bit assembly and linking settings are necessary for this source.
