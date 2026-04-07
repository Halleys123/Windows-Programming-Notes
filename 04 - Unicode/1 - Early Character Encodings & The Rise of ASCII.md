---
title: Early Character Encodings & The Rise of ASCII
tags:
---
Before modern Unicode existed, early computers had to agree on how to map binary numbers (0s and 1s) to human letters. This evolution was driven entirely by the hardware limitations of the time.

## 1. The Pre-ASCII Era (IBM's Legacy)

- **Hollerith Cards (1890):** The earliest physical data encoding, invented for the US Census using punched paper cards.
- **BCDIC (Binary-Coded Decimal Interchange Code):** A 6-bit code developed by IBM based on the old punch cards.
    - _The Flaw:_ It was incredibly messy. For example, the character code for `"0"` was placed _after_ the code for `"9"`.
- **EBCDIC (Extended BCDIC):** In the 1960s, IBM upgraded BCDIC to an 8-bit standard.
    - _The Legacy:_ Because it was so clunky (letters were not placed in contiguous alphabetical order), it never caught on globally. Today, it only exists on legacy IBM mainframes.

## ASCII
### 2. The Creation of ASCII (1950s – 1967)

The American Standard Code for Information Interchange (ASCII) was created to be a universal, logical standard. During its creation, engineers had a massive debate over how many bits a character should take up:

- **Why not 6-bit?** 6 bits only gives 64 characters. To fit lowercase, uppercase, and symbols, they would have needed a "Shift" character (like Braille). Hardware engineers argued this was too unreliable. If the computer missed the "Shift" signal, the rest of the document would read as gibberish.    
- **Why not 8-bit?** In the 1960s, memory was astronomically expensive. Adding an 8th bit to every single character in a database was deemed too costly.
- **The Winner: 7-bit.** 7 bits provided exactly 128 slots. This was the perfect compromise between memory cost and character capacity.

#### **The ASCII 128-Character Breakdown:**

- 26 Uppercase Letters
- 26 Lowercase Letters
- 10 Digits
- 32 Symbols (punctuation, math)
- 33 Control Codes (invisible hardware commands like "Carriage Return" or "Beep")
- 1 Space character

### 3. Why ASCII is a Masterpiece of Engineering

ASCII didn't just win because it was standard; it won because the layout of the characters was mathematically brilliant for programmers.

1. **Contiguous Letters:** Unlike EBCDIC, `A` through `Z` are in perfect numerical order. You can easily check if a character is a capital letter in C by writing: `if (c >= 'A' && c <= 'Z')`.
2. **Easy Number Parsing:** The codes for `0` through `9` are logically mapped. To convert the text character `'5'` into the actual integer `5`, a computer just subtracts the value of `'0'`.
3. **The "Bit-Flip" Case Conversion:** This is ASCII's greatest trick. The binary code for uppercase and lowercase letters is identical, except for exactly **one bit**.
    
    - `'A'` is `0100 0001` (Decimal 65)
    - `'a'` is `0110 0001` (Decimal 97)
    - A C programmer can convert a letter from uppercase to lowercase instantly just by adding `32` or doing a bitwise `OR` operation.

Although was great for that time period but it also had its own flaws, [[2 - Flaws of ASCII and Extending ASCII|read about flaws of ASCII]]
