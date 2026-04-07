---
tags:
---
[[5 - Unicode|Unicode]] is just a giant list of numbers. **It does not dictate how a computer should physically save those numbers in RAM.**

- A Wide Character is a specific C/C++ data type
- Just like `int`, `float`, or standard 8-bit `char`, a `wchar_t` is a physical container in the computer's memory.
- The C++ standard simply defines it as _"a character type that is larger than the standard 8-bit char."_

## The Cross-Platform Trap

C++ language never explicitly told compiler developers _how big_ the `wchar_t` bucket should be. They left it up to the Operating System creators.

### The Windows Way (16-bit):

When Microsoft built Windows NT, they defined `wchar_t` as exactly **16 bits (2 bytes)**. They used these 16-bit buckets to implement a specific version of Unicode called **UTF-16**.

- _This is why in Win32 programming, "Wide Character" and "Unicode" mean the exact same thing. Microsoft tied them together at the hip._

### Linux / macOS Way (32-bit): 

When the creators of Linux and macOS implemented C++, they realized that Unicode was growing so fast that 16 bits wouldn't be enough to hold all the emojis and dead languages. So, they defined `wchar_t` as **32 bits (4 bytes)**. They use these 32-bit buckets to implement a different version of Unicode called **UTF-32**.

>[!question]- Gap between memory and storage
But simply looking it at in this way cause problems if we think about it, say I write two Japanese character in windows that will occupy 4 bytes in windows and windows will save it like that on hard disk, now if I transfer that to Linux, it will try to read all four bytes at same time?

>[!solution]-
>If operating systems actually worked the way you described—just blindly dumping the raw `wchar_t` memory from RAM straight onto the hard drive—it would instantly cause exactly the catastrophic crash you are predicting.
>This is not how storage and RAM works, when storing something in RAM the data is converted to a form that is most easily processable by that OS but when saving to storage, it is saved in some universally available format or method.
>
>More details below.

## The Bridge Between Systems: RAM vs. Disk

The key to understanding cross-platform text is realizing that an Operating System treats text differently depending on where it lives:

1. **In-Memory (Execution):** When your C++ program is actively running, it uses `wchar_t`. This is highly optimized for the specific CPU and OS (16-bit on Windows, 32-bit on Linux).
2. **On-Disk / Network (Storage):** When you hit "Save" or send an email, the OS _never_ just dumps raw `wchar_t` memory to the hard drive. It translates (serializes) the memory into a universally agreed-upon **File Encoding** format.

### How the Transfer Actually Works

You write two Japanese Kanji characters (`漢字`) in Notepad on Windows, save the file, and email it to a Linux machine.

#### Step 1: Windows RAM (UTF-16)

While you are typing in Notepad, Windows is holding those two characters in RAM using its native 16-bit `wchar_t` buckets. It takes exactly 4 bytes of RAM.

#### Step 2: Saving to Disk (The UTF-8 Translation)

When you click "Save," Windows does not save those raw 4 bytes. Today, almost all modern text editors default to saving files as **UTF-8**. Windows runs a translation algorithm: it takes the 16-bit RAM buckets, breaks them down, and repacks them into standard 8-bit UTF-8 blocks. Japanese characters take 3 bytes each in UTF-8, so the file on your hard drive becomes **6 bytes** long.

#### Step 3: Opening on Linux (The UTF-32 Translation)

When Linux receives the file, it looks at the hard drive and sees a standard 6-byte UTF-8 file. It reads the UTF-8 file into memory. As it loads, Linux runs a translation algorithm: it unpacks the 8-bit blocks from the file and pours them into its native 32-bit `wchar_t` buckets for the text editor to use.

**The Result:** The file format (UTF-8) acts as a universal bridge. The two operating systems never actually see each other's native memory size.

---
More on char data type and wide character data type in:
1. [[6.1 - char, wchar_t, WCHAR and more]]
2. [[6.2 - Wide Characters and ISO Standard (Summary)]]