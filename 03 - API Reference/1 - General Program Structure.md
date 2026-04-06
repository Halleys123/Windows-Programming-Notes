```C
#include <stdio.h>
#include <windows.h>

int WINAPI wWinMain(HINSTANCE hInstance, HINSTANCE hPrevInstance, PWSTR cmdLn, int nCmdShow) {
	MessageBox(NULL, "Window was created successfully", "Success", MB_OK | MB_ICONINFROMATION);
	return 0;
}
```

Above is a valid Windows program structure which primarily intakes four parameters which are:

1. **HINSTANCE hInstance** This parameter takes the Handle to the instance of the current program. It is the unique ID the operating system uses to keep track of your specific running `.exe` loaded in memory.

2. **HINSTANCE hPrevInstance** This parameter takes the Handle to the previous instance of the program. In modern 32-bit and 64-bit Windows, this parameter is completely obsolete and will always be NULL (0). It is a leftover from 16-bit Windows, where it was used to check if another copy of the app was already running to share memory.

3. **PSTR cmdLn** This parameter takes the command-line arguments passed to the program when it is started, in the form of a single string. Unlike the standard C `main(int argc, char* argv[])` function, this string does not include the name of the executable itself, only the arguments passed after it. _(Note: Since you are using `wWinMain` which implies Unicode, the official type here is usually `PWSTR` or `LPWSTR` for wide strings, but the purpose is identical)._

4. **int nCmdShow** This parameter takes an integer flag that dictates how the window should initially be displayed on the screen. The OS passes standard values here (like `SW_SHOWNORMAL`, `SW_SHOWMAXIMIZED`, or `SW_SHOWMINIMIZED`) so your program knows whether to open normally, take up the whole screen, or start minimized in the taskbar.

## Alternate structure of program

### 1. The Alternatives to `wWinMain`

In Windows programming, there are exactly **four** standard entry points you can use. Which one you choose tells the compiler two things:

1. Should this program open a black Command Prompt window? (Subsystem)
2. Does this program use old 8-bit text or modern 16-bit Unicode text? (Character Set)

Here is how they differ:

| Entry Point    | Subsystem     | Text Type      | Command Line Parameter Type |
| -------------- | ------------- | -------------- | --------------------------- |
| **`main`**     | Console       | 8-bit ANSI     | `char* argv[]`              |
| **`wmain`**    | Console       | 16-bit Unicode | `wchar_t* argv[]`           |
| **`WinMain`**  | Windows (GUI) | 8-bit ANSI     | `LPSTR cmdLn` (or `PSTR`)   |
| **`wWinMain`** | Windows (GUI) | 16-bit Unicode | `LPWSTR cmdLn` (or `PWSTR`) |

> Note - These options are for MSVC not for default version of MinGW (although supported by MinGW-x64)

## Importance of WINAPI

WINAPI is a macro defined by Microsoft which under the hood is same as `CALLBACK` macro, which is also an alias of WINAPI and loosely speaking are ways to tell the function that it itself is responsible for cleaning the stack memory it used.

```c
#define WINAPI __stdcall
#define CALLBACK __stdcall
```

They both tell the compiler to use the `__stdcall` (Standard Call) rule. These are defined in [[0 - Important Windows Headers and DLL's#WINDEF.h|WINDEF.H]]

### What is `__stdcall`?

When one function calls another function, it passes variables (parameters) by pushing them into the thread's memory Stack. But when the function is done, **who is responsible for cleaning up that memory?**

- **`__cdecl` (C Declaration):** This is the default rule in C and C++. It states that the **Caller** (the one who asked for the function) must clean up the stack.
- **`__stdcall` (Standard Call):** This is the strict rule Microsoft created for the Windows Operating System. It states that the **Callee** (the function that was just executed) must clean up the stack before it returns.

### When to Use CALLBACK or WINAPI

You only need to use `CALLBACK` or `WINAPI` when the function crosses the boundary between your program and the Windows Operating System. That means the functions that are called by the OS like the entry point.

If writing simple procedure we don't need to use `__cdecl or __stdcall` by default all function are `__cdecl` that means the caller is responsible for cleaning the stack

However the Nuance of all this is much more than stated above, due to upgrades in CPU design and presence of ultra fast registers in CPU... [[02 - Architecture & Mechanics/Future Reads/History of CALLBACK and APIENTRY (FUTURE_READ)|Read More it may confuse you]]

## Summary

### Calling Conventions: `__cdecl` vs `__stdcall`

| Feature             | `__cdecl` (Standard C/C++)                                                   | `__stdcall` (`WINAPI` / `CALLBACK`)                                                       |
| ------------------- | ---------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------- |
| **Default Usage**   | Standard internal C/C++ functions.                                           | Win32 API functions & OS Callbacks.                                                       |
| **Stack Cleanup**   | **Caller** (The function that asked for the work).                           | **Callee** (The function actually doing the work).                                        |
| **Executable Size** | Slightly larger (cleanup code is written every time the function is called). | Slightly smaller (cleanup code is only written once, inside the function itself).         |
| **Compatibility**   | Specific to the C/C++ compiler.                                              | Universal standard (Safe to share across different languages like Python or C# via DLLs). |
|                     |                                                                              |                                                                                           |

### The Golden Rule of Windows Functions

Whenever you write a new function in your Windows program, ask yourself: **"Who is calling this function?"**

- **If the answer is "Me" (Your Code):** Do nothing. Write a normal C++ function. The compiler will automatically use `__cdecl` and handle the stack for you.
- **If the answer is "Windows" (The OS):** You **must** put `CALLBACK` (or `WINAPI`) in front of the return type. Because the OS is crossing the boundary into your program, it strictly expects the `__stdcall` rule.
