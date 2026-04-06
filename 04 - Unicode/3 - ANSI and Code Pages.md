---
tags:
  - "#history/text/codepages"
  - "#api/legacy"
---

When Microsoft launched Windows 1.0, they had a major problem: the IBM PC extended character set (with its line-drawing boxes) was useless for a graphical OS that needed to support international languages. They needed a software-level standard.

## The "ANSI" Character Set (Windows Latin 1)

Microsoft officially side-lined the IBM hardware standard and adopted a draft of the **ISO 8859-1** standard, which they dubbed the "ANSI Character Set."

- **The Lower 128 (0x00 to 0x7F):** Remained identical to standard 7-bit ASCII.
- **The Upper 128 (0x80 to 0xFF):** Completely rewritten. They threw out the MS-DOS line-drawing characters and replaced them with actual typography for Western European languages (Latin 1).

> Note for your Win32 code: When you use the "A" version of a Windows API function like `MessageBoxA`, the "A" literally stands for this exact ANSI character set!

## The Invention of "Code Pages"

Even with Windows pushing the ANSI standard, millions of people were still using MS-DOS. Microsoft had to find a way to support both legacy IBM hardware and new international users without breaking the operating system.

In 1987, they introduced **Code Pages**. A Code Page is essentially a swappable dictionary that tells the computer how to interpret the upper 128 slots of an 8-bit byte.

- **The Golden Rule:** The lower 128 slots (0-127) are _always_ standard ASCII. They never change. Only the upper 128 slots (128-255) get swapped.
- **Code Page 437 ("MS-DOS Latin US"):** This was the original, legacy IBM PC character set with the smiley faces and line-drawing boxes. ^cd548a
- **Code Page 850 ("MS-DOS Latin 1"):** A new dictionary that replaced the line-drawing boxes with European accented letters.
- **Other Code Pages:** Dictionaries were created for Cyrillic, Greek, Arabic, etc.

### The Fatal Flaw of Code Pages 

While Code Pages were a clever band-aid, they were a nightmare for global networking. If a computer in Germany (using a German Code Page) saved a text file and emailed it to a computer in Russia (using a Cyrillic Code Page), the upper 128 characters would be interpreted using the wrong dictionary. The document would turn into complete gibberish!

This exact nightmare is what forced the entire tech industry to eventually unite and [[5 - Unicode#Unicode|create unicode]].

--- 
All the limitation that were faced during that time period gave rise to [[4 - Double Byte Character Set|Double Byte Character|Double Byte Character Sets]] and ultimately to [[5 - Unicode|unicode]]. 