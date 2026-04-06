---
tags:
  - architecture/data_serialization
---

>[!info]
>Four pillar architecture helps to better remember what are Unicode, ASCII, ANSI, Code Pages, Wide Characters and how are they related.

## The 4 Pillars of Text Architecture

### Pillar 1: The Abstract Dictionary (Character Sets)

This is just a mathematical concept. It is a giant list published by a world organization that assigns a unique ID number to a human letter. **It does not exist in memory or on a disk.**

- [[1 - Early Character Encodings & The Rise of ASCII#ASCII|ASCII:]] The original dictionary (128 items).
- [[5 - Unicode|Unicode:]] The modern, universal dictionary (149,000+ items).
    - _Example:_ Unicode dictates that the ID for the letter 'A' is `U+0041`.

### Pillar 2: The Translation Rulebook (Character Encodings)

This is the algorithm the computer uses to translate that abstract ID number into actual binary 1s and 0s.
**An Encoding (like UTF-8 or ANSI) is a mathematical rulebook.** A rulebook can be used to organize data in RAM while a program is running, _and_ it can be used to organize data on a hard drive.

- **UTF-8:** A rulebook that says, _"Use 8-bit blocks. English gets 1 block, Japanese gets 3 blocks."_
- **UTF-16:** A rulebook that says, _"Use 16-bit blocks for almost everything."_
- **Code Pages:** _This is the piece you were missing!_ A Code Page is simply **Microsoft's legacy 8-bit rulebook.** Before UTF-8 existed, if Windows wanted to translate the abstract concept of a Russian letter into binary, it looked up the rule in "Code Page 1251". DBCS (like Shift-JIS) is just a highly complex Code Page rulebook.
- "ANSI" (in the Windows programming world) is a **Character Encoding Rulebook** used by legacy Windows.
- DBCS was a messy encoding system that used to rulebooks one was [[1 - Early Character Encodings & The Rise of ASCII#ASCII|ASCII]] and other was [[5 - Unicode|Unicode]]

### Pillar 3: The RAM Container (C++ Data Types)

This is the physical bucket the C++ compiler carves out of your RAM to hold the binary created by Pillar 2 while your program is actively running.

- **`char` (8-bit bucket):** Used to hold ASCII, Code Pages, and UTF-8.
- **`wchar_t` (16-bit bucket):** Used by Windows to hold UTF-16. _(Note: "Wide Character" strictly refers to this physical C++ RAM bucket, not the abstract dictionary)._

### Pillar 4: The Disk Storage (Serialization)

This is how the OS writes the buckets from RAM onto the physical hard drive while using some encoding algorithm from [[#Pillar 2 The Translation Rulebook (Character Encodings)|Pillar 2]].

- As we discussed, a modern OS almost always takes the data from Pillar 3 and serializes it into a **UTF-8 File** on the hard drive, because it is the safest universal bridge between different computers.