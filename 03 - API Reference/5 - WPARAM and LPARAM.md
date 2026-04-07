The origin of these names requires a bit of history. When Windows was a 16-bit system, the third parameter to [[03 - Window Procedure|WndProc]] was defined as a WORD, which was a 16-bit unsigned short integer, and the fourth parameter was defined as a LONG, which was a 32-bit signed long integer.

That’s the reason for the “W” and “L” prefixes on the word “PARAM.”

In the 32-bit versions of Windows, however, WPARAM is defined as a UINT and LPARAM is defined as a LONG (which is still the C long data type), so both parameters to the window procedure are 32-bit values