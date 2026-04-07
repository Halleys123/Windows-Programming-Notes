---
aliases:
---
In the context of Windows Programming, the term `Window` carries specific architectural weight. A window is a defined rectangular area on the screen that receives user input and displays visual output in the form of text and graphics.

The `Microsoft Windows OS` is fundamentally built upon this concept; nearly every graphical element ultimately resolves to a window.

[[2 - MessageBox|MessageBox]] was also a special kind of Window which was created with a function called [[2 - MessageBox#^605e95|MessageBox]].

Every window is instantiated based on a [[02 - Window Class]]. The window class acts as a blueprint that strictly identifies which [[03 - Window Procedure]] will process messages for that specific window. This architectural design allows multiple, separate windows to share the same underlying logic.