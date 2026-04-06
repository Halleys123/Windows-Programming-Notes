---
tags:
  - "#architecture/linking"
---

Windows has a concept of Dynamic Linking: a developer-written program can use a set of functions provided by another developer (often Microsoft itself). This allows developers to create Windows applications using the Win32 API, which is entirely `.dll` based.

These are mostly location in paths `\WINDOWS\SYSTEM` subdirectory under **Windows 98** and the `\WINNT\SYSTEM` and `\WINNT\SYSTEM32` sub directories under **Windows NT**.
**In modern 64-bit operating systems (like Windows 10 and 11), these core files are located in `C:\Windows\System32` (which actually holds 64-bit DLLs) and `C:\Windows\SysWOW64` (which holds 32-bit DLLs for backwards compatibility).**

- In earlier windows (windows 98 and windows NT) there were only three DLL files that control the windows named `KERNEL32.DLL` controls memory management, file I/O, and tasking, `USER32.DLL` controls the UI of the program and `GDI32.dll` controls the Graphics Device interface to display text and graphics.
- Windows 98 had several thousands of function calls which were used to control the behaviour of window created and several other aspects. this has grown to **tens of thousands** of documented functions (and many more undocumented ones) spanning modern graphics, networking, and security APIs.

## Usage

When a program is started it interfaces to windows through a process called Dynamic Linking, the final executable of the program you created will have reference to various DLL files given by Microsoft.

As for the working as we know to run a function/program/application we need that item in the memory of the computer, but in case of DLL it happens in following steps:

1. **The First Launch (Loading into Physical RAM):** When you turn on your PC and the first program asks for `USER32.DLL`, the OS goes to the hard drive, reads the file, and puts **one single copy** of it into the physical RAM chips.
2. **Memory Mapping (The Illusion):** When you launch your program, the OS gives your program its own private, "virtual" memory space. When your program looks for the `USER32.DLL` functions, the OS simply creates a "pointer" (a map) from your program's virtual memory directly to that single physical copy of `USER32.DLL` that is already sitting in the RAM.
3. **Sharing:** If you launch a second, completely different program, the OS doesn't load the DLL from the hard drive again. It just draws another map from the second program pointing to the exact same physical spot in RAM.

Internal working of DLL is much more complex including the things like handling stack, heap memory, local, global variables but that is not required for just using or creating a DLL and hence won't be discussed here. ([[Study Later#^bfceee|Although I tried understanding it but was not able to do so hence skip for now]])