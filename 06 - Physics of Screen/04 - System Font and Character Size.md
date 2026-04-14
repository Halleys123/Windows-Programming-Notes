The device context also defines the font that Windows uses when you call [[01 - TextOut|TextOut]] to display text. The default is a font called the “system font”.

The system font is the font that Windows uses by default for text strings in title bars, menus, and dialog boxes. In the early days of Windows, the system font was a **fixed-pitch font**, which means that all the characters had the same width.

**Variable pitch font** was introduced in Windows 3.0, the system font became a variable-pitch font, which means that different characters have different widths. 

<iframe src="http://localhost:82/fixed_variable_pitch_font.html" width=100% height=400px></iframe>

[View full simulation here](http://localhost:82/fixed_variable_pitch_font.html)

## Size of a Character

To display multiple lines of code we need to know the exact size of each character, but that is not possible, because it all depends on the pixel size of the video display.

To know about the size of font, user can use [[12 - GetTextMetrics|GetTextMetrics]] similar to how a user can use [[13 - GetSystemMetrics|GetSystemMetrics]] to get information about interface.

