---
tags:
  - "#history/api_design"
---
# The History of Windows Calling Conventions (`WINAPI` & `CALLBACK`)

To understand why Windows uses `WINAPI` and `CALLBACK`, you have to look at the severe hardware limitations of the 1980s and the massive architectural shifts of the 1990s and 2000s.

## 1. The Origins: The 16-bit Era (1985 – 1993)

**Operating Systems:** Windows 1.0 through Windows 3.1 **Processors:** Intel 8086, 80286 (Real Mode and early Protected Mode)

In the 16-bit era, RAM and hard drive space were incredibly scarce. The standard C calling convention (`__cdecl`) was considered too "fat" for an operating system because it required the **Caller** to clean the stack. If a program called `MessageBox` 1,000 times, the compiler had to generate the stack-cleanup assembly instruction 1,000 times, bloating the size of the `.exe` file.

**The Solution: `PASCAL`** Microsoft decided that the Windows API would not use standard C rules. Instead, they used the calling convention of the **Pascal** programming language. In Pascal, the **Callee** (the function itself) cleans the stack. This meant the cleanup code was written exactly once (inside the Windows DLLs), keeping user programs tiny.

During this era, if you wrote a Windows program, your functions looked like this:

```c
int PASCAL WinMain(...) 
```

## 2. The 32-bit Transition: The Birth of `WINAPI` (1993 – 2005)

**Operating Systems:** Windows NT 3.1, Windows 95 **Processors:** Intel 80386, Pentium

When Microsoft transitioned to 32-bit architecture, the old `PASCAL` convention broke down because it couldn't handle C's "variadic functions" (functions that take a variable number of arguments, like `printf`).

Microsoft needed a new standard for 32-bit Windows that:

1. Kept the file-size benefits of Callee-cleanup.
2. Supported modern C/C++ development.
3. Allowed other languages (Visual Basic, Delphi) to easily call Windows functions.

**The Solution: `__stdcall`** Microsoft invented `__stdcall` (Standard Call). However, they faced a massive problem: millions of lines of 16-bit code already had the word `PASCAL` written everywhere. If they just told everyone to replace `PASCAL` with `__stdcall`, it would break compatibility.

So, Microsoft created **Abstraction Macros**. They told developers: _"Stop using hardware-specific words. Use our new macros. We will define them based on whatever compiler you are using."_

Thus, `WINDEF.H` was born with these definitions:

```C++
// If compiling for modern 32-bit Windows:
#define WINAPI    __stdcall
#define CALLBACK  __stdcall

// (Hidden away for legacy 16-bit compilers, they still equaled PASCAL)
```

- **`WINAPI`** was designated for functions the OS provides to you.
- **`CALLBACK`** was designated for functions you provide to the OS.

## 3. The 64-bit Obsolescence (2005 – Present)

**Operating Systems:** Windows XP Professional x64, Windows 7, 10, 11 **Processors:** AMD64, Intel x86-64

By the mid-2000s, RAM and hard drive space were no longer the primary bottlenecks; **CPU speed** was. Hardware engineers realized that pushing variables into the RAM Stack (which `__cdecl` and `__stdcall` both did heavily) was incredibly slow compared to the blazing speed of the CPU's internal hardware registers. Modern 64-bit processors had plenty of these registers to spare.

**The Solution: The x64 Calling Convention** Microsoft completely abandoned the concept of Callee-cleans vs. Caller-cleans at the hardware level. They created a single, unified rule for 64-bit Windows:

1. The first four parameters of any function are passed directly into the CPU's ultra-fast registers (`RCX`, `RDX`, `R8`, `R9`).
2. Only if there is a 5th parameter does the system resort to using the RAM stack.
3. The Caller is strictly responsible for allocating and cleaning a "Shadow Space" on the stack.

> **The "Aha!" Connection:** > When you call a standard function like `wWinMain(A, B, C, D)`, the OS shoves all four parameters directly into the CPU's registers (its hands). **If the parameters never go onto the RAM Stack, the RAM Stack does not need to be cleaned!** The whole debate over who cleans the stack (`__cdecl` vs `__stdcall`) completely vanished because the stack isn't holding the parameters anymore.

### Why do the macros still exist today?

If the 64-bit hardware completely ignores `__stdcall`, why are `WINAPI` and `CALLBACK` still required in modern C++ code?

- **The C++ Type System:** The C++ language standard dictates that `__cdecl` and `__stdcall` are fundamentally different data types. Even if the hardware treats them identically, the compiler will throw a "Type Mismatch Error" if Windows asks for a `__stdcall` function pointer and you provide a `__cdecl` one.
- **Cross-Architecture Portability:** By forcing developers to keep the macros in their code, Microsoft ensures that a program written on Windows 11 (64-bit) can be recompiled for an older 32-bit machine without changing a single line of text. The compiler simply looks at the target architecture and decides whether to use the modern x64 register rules or fall back to the old 32-bit stack rules.

---

### Summary Timeline

| Era        | Architecture | Standard OS Convention | Who Cleans the Stack? | Primary Motivation                                            |
| ---------- | ------------ | ---------------------- | --------------------- | ------------------------------------------------------------- |
| **1980s**  | 16-bit (x86) | `PASCAL`               | Callee (Function)     | Save RAM and Disk Space (Smaller `.exe`).                     |
| **1990s**  | 32-bit (x86) | `__stdcall`            | Callee (Function)     | Standardize Windows APIs; allow VB/Delphi interop.            |
| **2000s+** | 64-bit (x64) | **x64 Convention**     | Caller                | Pure Execution Speed (Use CPU registers, skip the RAM stack). |
|            |              |                        |                       |                                                               |

### External Links

1. [Argument Passing and Naming Convention](https://learn.microsoft.com/en-us/cpp/cpp/calling-conventions?view=msvc-170)
2. https://learn.microsoft.com/en-us/cpp/cpp/argument-passing-and-naming-conventions?view=msvc-170
3. [x64 Calling convention](https://learn.microsoft.com/en-us/cpp/build/x64-calling-convention?view=msvc-170)
4. [Calling conventions by osdev](https://wiki.osdev.org/Calling_Conventions)
5. [Software Optimization](https://www.agner.org/optimize/)
6. https://learn.microsoft.com/en-us/sysinternals/resources/windows-internals

---

> Personal Note 
> This section needs extensive understand as it was kind of confusing to Chronology of how everything changed (I think someone at Mircrosoft is also not very clear about how everything is working.). (Maybe I can ask on `stack overflow` about CALLBACK and APIENTRY and how were they used earlier and how are they currently used)