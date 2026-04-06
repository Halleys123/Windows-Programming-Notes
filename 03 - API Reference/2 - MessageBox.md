---
tags:
  - "#api/functions/ui"
---

```c++
MessageBox(HWND, "MESSAGE_STRING", "HEADING_STRING", UINT);
```

This is the structure of the `MessageBox` function used to generate a Windows-style default message box. It takes 4 parameters:

## **`HWND`** 

This is the first parameter defining who is the owner (or parent) of this message box. This can either be `NULL` or a valid `HWND` value of a currently existing window. If the `HWND` value given is illegal, destroyed, or not found in the system, **the function will fail immediately, no message box will be drawn, and it will return `0` (You can then use `GetLastError()` to find error code 1400: Invalid Window Handle).**

Based on the value provided (if it is `NULL` or a valid `HWND` value), the following two core properties of the Operating System are affected:

**1. Modality (Input Locking)**

- **If `HWND` is provided:** The message box becomes **Modal**. The Operating System completely disables mouse and keyboard input for the parent window. The user is locked out of the main application until they click a button (like OK or Cancel) on the message box to close it. Clicking the background window will produce an error sound.
- **If `NULL` is provided:** The message box is **Modeless** (unowned). The user can freely click back into the main application, type, and interact with it while the message box safely sits in the background.

**2. Z-Ordering (Stacking Behaviour)**
- **If `HWND` is provided:** The Z-order (3D depth) of the message box is permanently glued to the parent window. The message box is guaranteed to always float directly on top of the parent window. If the parent window is brought to the front of the screen, the message box comes with it.
- **If `NULL` is provided:** The message box acts as an independent window. If the user clicks the main application, the main application will move to the front and physically cover up the message box, potentially hiding it from the user.

## `MESSAGE_STRING`

**MESSAGE_STRING (`LPCSTR` / `LPCWSTR lpText`)** This is the second parameter, which defines the actual body text of the message box.

- **Formatting:** It is a standard null-terminated string. If you want to format the text into multiple paragraphs or lines, you can use standard C escape sequences like `\n` (newline) or `\t` (tab) directly inside the string.
    
- **Unicode Note:** If you compiled your program using `wWinMain`, Windows expects a 16-bit wide string here. You should prefix your string literal with an `L` (e.g., `L"Operation completed successfully."`).
    
## `HEADING_STRING`

**HEADING_STRING (`LPCSTR` / `LPCWSTR lpCaption`)** This is the third parameter, which defines the text displayed in the title bar at the very top of the message box.

- **The `NULL` Quirk:** If you do not want a title, you might be tempted to pass `NULL`. However, if you pass `NULL` here, Windows will automatically default the title bar text to **"Error"**. If you want a truly blank title bar, you must pass an empty string (`""` or `L""`) instead of `NULL`.

## `UINT uType`

**UINT (`UINT uType`)** This is the fourth and most versatile parameter. It is an unsigned integer that determines the contents and behavior of the dialog box.

Because it is a bitmask, you do not just pass one value. You combine multiple flags together using the **Bitwise OR operator (`|`)** to build the exact box you need. The flags are divided into several categories:

**1. Button Flags (Choose exactly one)** These determine which clickable buttons appear at the bottom of the box.

- `MB_OK`: The default. Shows one "OK" button.
- `MB_OKCANCEL`: Shows "OK" and "Cancel".
- `MB_YESNO`: Shows "Yes" and "No".
- `MB_RETRYCANCEL`: Shows "Retry" and "Cancel".
- `MB_ABORTRETRYIGNORE`: Shows "Abort", "Retry", and "Ignore".

**2. Icon Flags (Choose one, optional)** These display a standard system icon on the left side of the message text, which also triggers a specific Windows notification sound.

- `MB_ICONINFORMATION` (or `MB_ICONASTERISK`): Displays a blue 'i' in a circle. Used for successful operations.
- `MB_ICONWARNING` (or `MB_ICONEXCLAMATION`): Displays a yellow triangle with an exclamation mark. Used for warnings.
- `MB_ICONERROR` (or `MB_ICONHAND`, `MB_ICONSTOP`): Displays a red circle with a white X. Used for critical failures.
- `MB_ICONQUESTION`: Displays a blue question mark. _(Note: Microsoft officially deprecates this icon in modern UI guidelines, recommending you just use text to ask the question)._

**3. Default Button Flags (Choose one, optional)** If your box has multiple buttons, this dictates which button is highlighted by default (the one that gets clicked if the user simply presses the "Enter" key on their keyboard).

- `MB_DEFBUTTON1`: The first button is default (Standard behavior).
- `MB_DEFBUTTON2`: The second button is default (e.g., highlighting "No" in a YES/NO box to prevent accidental deletions).
- `MB_DEFBUTTON3`: The third button is default.