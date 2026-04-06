---
tags:
  - "#architecture/system_files"
---
# Headers

1. [[#WINDEF.h]]
2. [[#`WINNT.H` Architecture & Unicode Foundation|WINNT.h]]
3. [[#`WINBASE.H` The Kernel Services (Engine Room)|WINBASE.h]]
4. [[#`WINUSER.H` The User Interface Manager|WINUSER.h]]
5. [[#`WINGDI.H` The Graphics Device Interface|WINGDI.h]]

## WINDEF.h

**Core Purpose:** `WINDEF.h` serves as the fundamental dictionary for the Win32 API. It is a massive list of `typedef` statements that translate standard C data types into strict, immutable Windows-specific data types.

### The Historical Context: Why reinvent standard C types?

> **The Size Problem of the 80s & 90s** Standard C variables (like `int` and `long`) do not have a guaranteed fixed size; their byte size changes depending on the compiler and the CPU architecture (e.g., 16-bit vs. 32-bit processors). Microsoft could not risk a standard C `int` behaving unpredictably and crashing the operating system. To ensure absolute stability across all hardware, they created their own strict data types that never change size.

### Key Components Reference

#### 1. The Building Blocks (Fixed-Size Types)

These are the absolute foundation of Windows memory management and messaging.

|Windows Type|Standard C Equivalent|Size|Description|
|---|---|---|---|
|**`BYTE`**|`unsigned char`|8 bits|The smallest addressable unit of data.|
|**`WORD`**|`unsigned short`|16 bits|Historically represented a 16-bit processor's "word" size.|
|**`DWORD`**|`unsigned long`|32 bits|"Double Word." Crucial for 32-bit memory addresses and colors.|

#### 2. The Handles (Opaque Pointers)

A "Handle" is Windows' way of giving you an ID tag for an object without letting you directly access that object's memory. `WINDEF.h` is where these are defined.

- **`HWND`**: Handle to a Window (Used to identify a specific window on the screen).
- **`HINSTANCE`**: Handle to an Instance (Used to identify your running program/executable in memory).

#### 3. The Boolean Truths (C Language Quirks)

Because the original C language did not have a native boolean type (until C99), Windows had to invent its own.

- **`BOOL`**: Actually `typedef`'d as a standard `int`.    
- **`TRUE`**: Defined as `1`.
- **`FALSE`**: Defined as `0`.

## `WINNT.H`: Architecture & Unicode Foundation

**Core Purpose:** `WINNT.h` defines the fundamental, lowest-level concepts of the "New Technology" (NT) architecture. It is the bridge between the operating system and the raw physical hardware (CPU registers), as well as the center for international language support.

### The Historical Context: The NT Leap

> **Security and Global Reach** When Microsoft moved from the consumer-grade Windows 9x to the professional Windows NT, they needed an OS that could run on multiple types of processors (Intel, AMD, ARM) and support secure file permissions. `WINNT.h` was created to hold the deep architectural definitions for CPU states and security. Additionally, to support non-English languages, it introduced macros that allowed developers to write code that could seamlessly switch between standard ASCII and international Unicode.

#### Key Components Reference

|Category|Component|Description|
|---|---|---|
|**Text & Strings**|`TCHAR` & `_T()`|Macros that automatically switch your text between 8-bit `char` (ANSI) and 16-bit `wchar_t` (Unicode) depending on your compiler settings.|
|**Hardware State**|`CONTEXT`|A structure that holds the exact, freeze-framed state of the CPU's physical registers (EAX, EBX, etc.) during a thread Context Switch.|
|**Security**|Security Descriptors|Low-level structures used to check if a specific user/process has permission to open a file or kill a task.|

## `WINBASE.H`: The Kernel Services (Engine Room)

**Core Purpose:** `WINBASE.h` is your C++ bridge to `KERNEL32.DLL`. It handles all the "invisible," structural tasks of the operating system. If a function does not touch the screen or the mouse, it likely lives here.

#### The Historical Context: The Headless OS

> **Separating Brains from Beauty** Microsoft designed Windows so the core engine (Kernel) was completely separated from the graphics (User/GDI). This allowed Windows to be run on heavy-duty servers without wasting memory drawing a user interface. `WINBASE.h` gives developers access to this raw engine power.

#### Key Components Reference

| Category          | Component                                                                                                                                     | Description                                                                                                                  |
| ----------------- | --------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| **Execution**     | `CreateProcess` / `CreateThread` [[Windows NT and Windows 98#Concurrency and Parallelism in Windows\|Details on Concurrency and Parallelism]] | Functions to launch new programs or split your current program into multiple concurrent tasks.                               |
| **File I/O**      | `CreateFile` / `ReadFile`                                                                                                                     | Functions to interact with the hard drive. _(Note: `CreateFile` is used to open existing files as well as create new ones)._ |
| **Memory**        | `HeapAlloc` / `GlobalAlloc`                                                                                                                   | Functions for dynamically requesting chunks of RAM from the Operating System.                                                |
| **Timing & Sync** | `Sleep` / Mutexes                                                                                                                             | Functions to pause threads or safely lock data so multiple threads don't crash into each other.                              |

## `WINUSER.H`: The User Interface Manager

**Core Purpose:** `WINUSER.h` is your C++ bridge to `USER32.DLL`. This is the Window Manager. Anything related to the existence of a window or the user physically interacting with the computer lives here.

#### The Historical Context: The Blueprint Era

> **Building from Scratch** Before modern drag-and-drop UI frameworks existed, developers had to build every button, scrollbar, and window pixel-by-pixel using the operating system's standard blueprints. This header contains the structures to register those blueprints and the loop required to listen for the user's mouse and keyboard.

#### Key Components Reference

| Category             | Component                        | Description                                                                                            |
| -------------------- | -------------------------------- | ------------------------------------------------------------------------------------------------------ |
| **Window Creation**  | `CreateWindowEx` / `WNDCLASS`    | The core function to spawn a window, and the structure used to define its style, cursor, and behavior. |
| **The Message Loop** | `GetMessage` / `DispatchMessage` | The infinite loop that keeps a Windows program alive, constantly checking for new user interactions.   |
| **Hardware Input**   | `WM_LBUTTONDOWN` / `WM_KEYDOWN`  | The specific message codes sent to your program when a mouse is clicked or a key is pressed.           |
| **Dialogs**          | [[2 - MessageBox\|MessageBox]]   | The standard Windows pop-up dialog function (Warning, Error, Info).                                    |
|                      |                                  |                                                                                                        |

## `WINGDI.H`: The Graphics Device Interface

**Core Purpose:** `WINGDI.h` is your C++ bridge to `GDI32.DLL`. While `WINUSER` creates the window borders, `WINGDI` is responsible for actually drawing the pixels, shapes, and text _inside_ that window.

#### The Historical Context: The Universal Canvas

> **Hardware Abstraction** In the MS-DOS days, a developer had to write custom driver code for every single brand of monitor and printer on the market. Windows solved this by introducing the Graphics Device Interface (GDI). The developer draws their shapes to a generic "Device Context" (a virtual canvas), and Windows translates that canvas into the specific electronic signals required by whatever monitor the user owns.

#### Key Components Reference

|Category|Component|Description|
|---|---|---|
|**The Canvas**|`HDC` (Device Context)|The "Handle to a Device Context." You cannot draw anything in Windows without first acquiring an HDC.|
|**Drawing Tools**|`HPEN` / `HBRUSH`|Digital tools used to draw the outlines of shapes (Pen) or fill the insides of shapes with color (Brush).|
|**Typography**|`TextOut`|The core function for drawing strings of text onto the screen using TrueType fonts.|
|**Pixels**|`BitBlt`|"Bit-Boundary Block Transfer." A high-speed function used to copy massive chunks of image data/pixels to the screen instantly.|