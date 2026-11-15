# 06.asm — 32-bit PUSH and POP

## 1. Objective
This program demonstrates 32-bit stack operations using `EAX` and `EBX`, showing how the stack follows last-in, first-out (LIFO) order.

## 2. Execution flow
1. `BITS 32` specifies 32-bit instruction mode for NASM.
2. Execution begins at `_start`.
3. `mov eax, 0x11223344` loads `0x11223344` into `EAX`.
4. `mov ebx, 0x1234` loads `0x1234` into `EBX`.
5. `push eax` pushes the 32-bit value from `EAX` onto the stack.
6. `push ebx` pushes `0x1234` above the first value.
7. `nop` performs no operation.
8. `pop ebx` removes the top value, `0x1234`, into `EBX`.
9. `pop eax` removes the next value, `0x11223344`, into `EAX`.
10. The program exits using the Linux i386 exit system call (`EAX = 1`, `EBX = 0`, `int 0x80`).

## 3. Stack pointer changes
Let the initial `ESP` be `S`. In this 32-bit program, each push or pop of a 32-bit register changes `ESP` by 4 bytes.

| Instruction | ESP | Stack top |
|---|---:|---|
| Before pushes | `S` | Existing process stack |
| `push eax` | `S - 4` | `0x11223344` |
| `push ebx` | `S - 8` | `0x1234` |
| `pop ebx` | `S - 4` | First value remains |
| `pop eax` | `S` | Original stack position restored |

## 4. Stack layout
After both pushes:

```text
ESP -> 0x00001234
       0x11223344
```

The most recently pushed value, `0x1234`, is at the top and is therefore popped first into `EBX`. The next pop restores `0x11223344` into `EAX`.

## 5. Testing observations
The program was assembled, linked and run using the 32-bit commands:

```bash
nasm -f elf32 06.asm -o 06.o
ld -m elf_i386 06.o -o 06
./06
echo $?
```

Observed result: no text was printed, and the exit status was `0`. This is expected because the program performs stack operations and exits but has no output instructions.

## 6. Conclusion
This program demonstrates the 32-bit form of LIFO stack behaviour. Each push decreases `ESP` by 4 bytes, and each pop increases it by 4 bytes. The two pops restore the stack pointer to its original position.
