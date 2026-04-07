---
tags:
---
C was invented in 1972. Back then, computers were the Wild West. Some computers had 8-bit bytes, some had 9-bit bytes, and some had 36-bit words. Because C was designed to run on _literally any hardware_, the C Standard is intentionally extremely loose.

## The C Standard Specifications

### 1. The `char` Rule: Always 1 Byte

The C standard defines the `char` type not strictly as a "character," but as the smallest addressable unit of memory on the machine.

- **The Law:** By absolute definition, `sizeof(char)` is **always exactly 1**.
- In C, the word "byte" literally just means "the size of a `char`".

### 2. The "8-Bit Byte" Myth (`CHAR_BIT`)

Does the C specification say 1 byte is 8 bits? **No, it does not!**

The C standard dictates that a byte must be **at least 8 bits** to be able to hold the basic English execution character set, but it has no strict maximum. To find out how many bits are in a byte on your specific machine, C provides a macro called `CHAR_BIT` inside the `<limits.h>` header file.

- On modern Windows, Linux, and Macs, `CHAR_BIT` is strictly **8**.
- However, on some older IBM mainframes or specialized DSP (Digital Signal Processing) chips, `CHAR_BIT` could be 16 or 32! On those machines, 1 byte = 32 bits.

### 3. The Relative Size Hierarchy (Integers)

When it comes to `short`, `int`, and `long`, the C standard **does not** give exact bit sizes. It only provides _minimums_ and a strict relative size hierarchy.

The only rule the compiler must follow is this mathematical inequality:

- `1 == sizeof(char) <= sizeof(short) <= sizeof(int) <= sizeof(long) <= sizeof(long long)`

Actual minimums guaranteed by the ISO C standard:

| **Data Type** | **C Standard Minimum** | **Modern Reality (Windows/Intel)** |
| ------------- | ---------------------- | ---------------------------------- |
| `char`        | At least 8 bits        | Exactly 8 bits                     |
| `short`       | At least 16 bits       | Exactly 16 bits                    |
| `int`         | At least 16 bits       | **Exactly 32 bits**                |
| `long`        | At least 32 bits       | Exactly 32 bits                    |
| `long long`   | At least 64 bits       | Exactly 64 bits                    |

## The Win32 Solution (Why Petzold uses weird types)

Now that you know the C standard is incredibly vague, you can understand exactly why Microsoft created the Win32 API the way they did.

If you are writing the Windows Kernel, you cannot rely on a vague "at least 16 bits" rule. When Windows talks to a graphics card, it needs to know with 100% mathematical certainty that a variable is exactly 32 bits wide. If the compiler decides to change the size of `int` tomorrow, the entire operating system would instantly crash.

To fix this, Microsoft bypassed the loose C standard completely. They created the `<windows.h>` header file and defined their own strict, hardware-level data types using `typedef`:

- **`BYTE`**: Strictly defined as `unsigned char` (Exactly 8 bits).
- **`WORD`**: Strictly defined as `unsigned short` (Exactly 16 bits).
- **`DWORD`**: Strictly defined as `unsigned long` (Exactly 32 bits - "Double Word").
- **`QWORD`**: Strictly defined as `unsigned long long` (Exactly 64 bits - "Quad Word").

>[!question]
>Based on the information till now, no matter how you define the data types you still are depending on ISO C's defined data structure which are not strictly defined, so how did Microsoft solved this?

## Enforcement and Modern Solutions

### 1. The Compiler as the Law (The MSVC Dictatorship)

A `typedef` is just an alias; it doesn't create a new size. However, because Microsoft owns both the **Windows OS** and the **MSVC Compiler**, they have hardcoded a "Contract of Sizes."

- On the MSVC compiler, `long` is mathematically locked at 32 bits.
- This allows `typedef unsigned long DWORD;` to be a physical guarantee, not just a label.

### 2. The ABI: The "Contract" of the OS

The **Application Binary Interface (ABI)** is the set of rules that tells a compiler how to behave so it can live on Windows.

- If a 3rd-party compiler (like Clang or GCC) wants to build a Windows app, it _must_ agree to make its `long` 32 bits to match Microsoft's `DWORD`.
- If it doesn't follow the ABI "Contract," the program won't be able to talk to the Windows Kernel.

### 3. The Modern Solution: `<stdint.h>`

To solve the "vague size" problem globally, the C99 standard introduced fixed-width types. Instead of guessing how big a `long` is, the compiler-maker provides a header that maps specific bits to the best available type.

- `uint8_t` → Maps to 8 bits.
- `uint32_t` → Maps to 32 bits.

>[!important]
>This does not mean that C99 don't have vague types like `int` it still have the older data types with original C definition, this means that new data types with fixed bit length were introduced in C99.


## Relation of CPU Bus Size and Data type

There is no strict _mathematical_ relation forced by the language, but there is a massive **performance relationship** forced by physics. This relationship is governed by a concept called the **CPU Word Size**.

#### What is a "Word"?

In hardware, a "Word" is the natural size of data that a CPU's internal architecture (its registers and ALU) is built to process in a single clock cycle.

- On a 16-bit processor (like the MS-DOS days), the Word is 16 bits.
- On a 32-bit processor, the Word is 32 bits.
- On a 64-bit processor, the Word is 64 bits.

#### Why Compiler Authors care about the Word

When a compiler author sits down to write the C compiler for a new CPU, they have a choice: _How big should I make the standard `int`?_

Historically, **the unwritten rule of C is that `int` should equal the CPU's native Word Size.** Why? Because that is the size the CPU calculates the fastest. If you make `int` 32 bits on a 32-bit CPU, adding two `int`s takes exactly 1 clock cycle. If you make `int` 64 bits on a 32-bit CPU, the compiler has to break the math into multiple steps, slowing down the entire program.

This brings us to the physical Memory Bus—the wires connecting your RAM to your CPU.

If you have a 32-bit CPU, your Memory Bus is usually designed to fetch exactly 32 bits (4 bytes) of data from RAM at a time. The CPU does not read RAM byte-by-byte; it reads it in 4-byte "chunks."

If you create an `int` (which is 4 bytes), where you place it in RAM matters physically.

- **Aligned Data:** If your `int` starts at a clean memory address (like `0x00` or `0x04`), it fits perfectly inside one of the Memory Bus's chunks. The CPU fetches it in **1 cycle**.
- **Unaligned Data:** If you cram an 8-bit `char` into memory, and put your 32-bit `int` immediately after it (starting at address `0x01`), the `int` crosses the physical hardware boundary. The CPU's Memory Bus is forced to fetch chunk 1, then fetch chunk 2, and use bit-math to glue them together. It takes **2 cycles**.
---
## Summary section

As this and [[6.1 - char, wchar_t, WCHAR and more]] are linked very much so I will write the summary in [[6.2 - Wide Characters and ISO Standard (Summary)]]