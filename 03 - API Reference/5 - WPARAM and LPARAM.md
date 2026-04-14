The origin of these names requires a bit of history. When Windows was a 16-bit system, the third parameter to [[03 - Window Procedure|WndProc]] was defined as a WORD, which was a 16-bit unsigned short integer, and the fourth parameter was defined as a LONG, which was a 32-bit signed long integer.

That’s the reason for the “W” and “L” prefixes on the word “PARAM.”

In the 32-bit versions of Windows, however, WPARAM is defined as a UINT and LPARAM is defined as a LONG (which is still the C long data type), so both parameters to the window procedure are 32-bit values.

These parameters save some important information that is variable depending on the `UINT msg` that arrives in the Window Procedure for example The `lParam` parameter contains the new size of the window. The new width (a 16- bit value) and the new height (a 16-bit value) are stuck together in the 32-bit `lParam` if the message is [[01.6 - WM_SIZE|WM_SIZE]], `WM_RESTORED etc` (Read more about messages here [[01.1 - Some Message and Processing Messages]]).

## Macros

There are several handy macros that Microsoft have defined that help us extract these two values from `lParam`. More in detail later.