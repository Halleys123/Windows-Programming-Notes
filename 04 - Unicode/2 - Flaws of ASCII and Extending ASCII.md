---
tags:
  - "#history/text/extended"
---

One of the primary problems that [[1 - Early Character Encodings & The Rise of ASCII#ASCII|ASCII]] encoding system face is that it is made for American standards including the currency, characters and had no support of characters from other languages which later gave rise to [[5 - Unicode|Unicode]].

America use English language, but English is one of the few **unusual language** which use Latin language as base but still don't have [[Glossary#^8709bf|diacritics]] in it, but most of the language even in the North and South American continent most use [[Glossary#^8709bf|diacritics]]. If travelled to east languages have there own base system like Sanskrit, Chinese, Hindi, Arabic, Russian, they don't even use Latin language as base and are not supported by the ASCII character set at all.

## Extending ASCII

### The 8-Bit Era (1981)

By the late 1970s and early 1980s, the hardware industry standardized the **8-bit byte**. Because standard ASCII only used 7 bits (128 characters), every byte of text now had one completely unused bit. This meant there were 128 empty slots available ($2^{8}$=$256$ total slots). Computer manufacturers suddenly had to decide what to put in those extra 128 slots.

### The IBM PC Extended Character Set

When IBM released the original PC in 1981, they filled those 128 empty slots with a custom, hardware-level character set burned directly into the video card and printer ROMs.
Because MS-DOS was entirely text-based, programmers couldn't draw actual graphics or windows. IBM solved this by adding **Line-Drawing Characters** into the extended slots.

- **The "Text-Mode" GUI:** Instead of drawing a real box, a C programmer would print specific text characters like `╔`, `═`, `╗`, and `║` to make it _look_ like a menu box on the screen.
- **Hijacking Control Codes:** IBM also realized that most of the 33 original ASCII control codes (like "Acknowledge" or "Device Control") were completely useless on a PC. So, they overrode those invisible codes with visible symbols like smiley faces (`☻`) and card suits (`♠`, `♣`).
- **Math and Accents:** They filled the remaining slots with lowercase Greek letters for math and a few accented characters for European languages.

### Why Windows Rejected the IBM Standard

This IBM set became a massive global standard for MS-DOS, but when Microsoft started building Windows, they realized the IBM extended set was practically useless for a modern operating system.

1. **Windows had Real Graphics (GDI)** Windows didn't need fake text-based line-drawing characters (`╔`, `═`) to build menus. Windows had the **Graphics Device Interface (GDI)**. It could draw actual, pixel-perfect borders, buttons, and scrollbars. Those line-drawing characters were suddenly a complete waste of character slots.
2. **The International Problem** Because IBM wasted so many slots on line-drawing shapes, they didn't have enough room left over to include all the accented letters required for standard Western European languages (like French, German, and Spanish). Windows needed to be an international operating system, so Microsoft needed those slots back for actual human language.

---

This gave rise to windows own character encoding system, which was called [[3 - ANSI and Code Pages|ANSI character set]] 