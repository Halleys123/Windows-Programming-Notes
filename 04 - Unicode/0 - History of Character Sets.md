---
tags:
---
Humans began speaking tens of thousands of years ago, but writing using alphabets only began about 3,000 years ago; before that, [[Glossary#^536d62|pictographs]] and symbols were used. Spoken language is actually estimated to be at least 50,000 to 150,000 years old. Early writing like Cuneiform started about 5,000 years ago, and alphabets started roughly 3,000 years ago

### The Precursor to Digital Encoding: Braille (1824)

While Morse Code was the first widespread electrical encoding, it was not the first instance of written language being represented by a codified system other than drawn or printed glyphs.

Between 1821 and 1824, inspired by a French military system for reading messages silently at night, a young Louis Braille developed a system of embossing raised dots into paper to be read by the blind. From a modern computer science perspective, the system Braille designed was incredibly ahead of its time:

- **A 6-Bit Architecture:** Braille is essentially a 6-bit binary code. It uses a 2x3 matrix of dots (where each dot is either raised or flat), allowing for exactly 64 possible combinations to encode letters, common letter combinations, entire words, and punctuation.
- **Escape Codes:** Because 64 combinations are not enough to represent every possible character variation, Braille utilizes a special "escape code" to indicate that the immediately following letter code should be interpreted as uppercase.    
- **Shift States:** Similar to the 'Shift' or 'Num Lock' key on a modern keyboard, Braille uses a special "shift code" that changes the reading state, allowing subsequent standard letter codes to be interpreted as numbers instead of letters.

## The Beginning of Digital Characters (1830)

As we know, computers and electrical wire systems cannot natively understand, read, or store human letters—they only understand binary physical states (the presence or absence of an electrical current). Therefore, to transmit human language over early machines, letters had to be translated into a mathematical format the hardware could process.

The earliest successful implementation of this was **Morse Code** in the 1830s. Long before modern computers, Morse Code was used to transfer data over telegraph wires by mapping every letter of the English alphabet to a specific combination of short and long electrical pulses (dots and dashes). In doing so, it became the world's first widely adopted digital character encoding system.

---

These were the initial encoding systems that were used to convert human spoken/written characters to computer versions, this also allowed the rise of more encoding systems that are currently used like [[1 - Early Character Encodings & The Rise of ASCII|ASCII]] and [[5 - Unicode|Unicode]]