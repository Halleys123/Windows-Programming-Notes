---
title: "Double Byte Character: precursor to Unicode"
tags:
  - history/text/dbcs
  - architecture/memory/variable_width
---
All the previous version of encoding no matter what they did could not support languages like Chinese, Japanese, Korean that had about $80,000$ characters, so a new system of encoding was required which still supported ASCII while having support for new characters

- A DBCS starts off with 256 codes, just like ASCII , that is first 8 bits are still reserved for normal ASCII, and as we know ASCII also had two versions 7 bit and 8 bit which were later used in [[3 - ANSI and Code Pages#^cd548a|strict ASCII/ASCII]] and implementation of [[3 - ANSI and Code Pages#The Invention of "Code Pages"|Code Pages]]. 
- That same trick was used to implement DBCS that is first 128 codes were used to implement normal ASCII, but then some of the next 128 codes are followed by another byte.
- The two bytes together (called a lead byte and a trail byte) define a single character, usually a complex ideograph.
- Windows supports four different double byte character sets: code page 932 (Japanese), 936 (Simplified Chinese), 949 (Korean), and 950 (Traditional Chinese).

> DBCS is also a [[3 - ANSI and Code Pages#The Invention of "Code Pages"|Code Page]], but more complex then the previous one, that is based on the code page used, it is windows Job to identify should it read next 1 byte/8 bits for a new character or is it a complex ideograph of language like Chinese/Japanese/Korean.

## Technical breakdown of how Windows handles these DBCS Code Pages (932, 936, 949, and 950)

**1. The "IsDBCSLeadByte" Test** When Windows reads an 8-bit string using a DBCS Code Page, it cannot just blindly print the characters. It must pass every single byte through a logic test. In the Win32 API, there is literally a function called `IsDBCSLeadByte()`.

- If the byte is in the normal range (usually `0x00` to `0x7F`), the test returns `FALSE`. Windows prints a standard 1-byte ASCII character.
- If the byte falls into the designated "high" range for that specific Code Page (e.g., `0x81` to `0x9F` in Shift-JIS), the test returns `TRUE`.

**2. The Trail Byte Trap** When `IsDBCSLeadByte()` returns `TRUE`, the OS immediately stops what it is doing. It grabs the _next_ byte in memory (the Trail Byte), regardless of what it is. It glues the Lead Byte and the Trail Byte together into a 16-bit value, looks up that specific 16-bit number in the Code Page dictionary, and prints the resulting complex ideograph (like a Japanese Kanji).

**3. The Fatal Flaw of the Trail Byte** This system had a massive vulnerability that caused countless bugs. _Trail bytes can sometimes have the same numerical value as standard ASCII characters._ If a programmer tried to search a DBCS string for the backslash character `\` (which is `0x5C` in hex) to find a file path like `C:\Folder`, they might accidentally find a Trail Byte that happened to be `0x5C`. The program would chop a Chinese character in half, thinking it found a folder separator, and completely corrupt the file path.

![[DBCS_Memory_Parsing.gif]]