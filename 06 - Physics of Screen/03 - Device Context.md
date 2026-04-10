The device context handle is your window’s passport to the GDI functions. With that device context handle you are free to paint your client area.

>[!important]
>Device Context does _not_ allow **any** kind of drawing but only **GDI** function drawings. If you possess an `HDC`, you are strictly bound to the GDI subsystem (`GDI32.dll`).

To perform any graphical output using the GDI subsystem, a program must first acquire a handle to a Device Context (`HDC`), which is typically achieved using the [[01.2 - WM_PAINT#`BeginPaint` Function|BeginPaint]] function while processing a `WM_PAINT` message, or the `GetDC` function when drawing is required outside of the standard paint cycle.
Device Context also posses information about font that windows uses when you call `TextOut` or other GDI Text functions, Read more in [[04 - System Font and Character Size]]

## Begin and End paint

### `BeginPaint` Function

During the BeginPaint call, Windows erases the background of the client area if it hasn’t been erased already, it is erased with the brush that is specified in the [[02 - Window Class#^efdbd9|hbrBackground]] field of the [[02 - Window Class|WNDCLASS]].
Then this function validates the client area and returns a "Handle to Device Context", This handle that is returned **by BeginPaint** won't allow you to write/draw anything outside the client area.

>[!important]
>Begin Paint should always be called from [[01.2 - WM_PAINT|WM_PAINT]] message only

When the OS sends a `WM_PAINT` message, it is saying, _"A specific rectangular area of your window is dirty. Fix it."_

You **must** use `BeginPaint` here for one critical, mechanical reason: **Validation.** When you call `BeginPaint`, it does two things:

1. It hands you the Device Context (`HDC`).
2. It automatically clears the window's "dirty" flag (this is called "Validating the Update Region").

>[!important]
> If you try to use `GetDC` inside a `WM_PAINT` message, you will get an `HDC` and you will successfully draw to the screen. 
> However, `GetDC` does _not_ clear the dirty flag. So, your `WndProc` finishes and returns control to the OS. 
> The OS looks at your window, sees it is still flagged as "dirty," and instantly fires another `WM_PAINT` message. 
> You will trigger an **Infinite Loop of Death**, pegging your CPU thread at 100% and completely freezing your application because it is doing nothing but repainting the same pixels billions of times a second.


### `EndPaint` function

This function releases the [[03 - Device Context]] that means you can't use this handle of device context to draw on client area.

Both functions take the second parameter which is a PAINTSTRUCT, it contains some information that a window procedure can use for painting the client area.


## `GetDC` and `ReleaseDC`

You use `GetDC` when you want to draw something _immediately_, without waiting for the OS to tell you to paint, and without altering the window's "dirty" status.

You use this strictly outside of `WM_PAINT`. A perfect example is handling `WM_MOUSEMOVE`.

Let's say you are building your File Explorer, and the user clicks and drags the mouse to draw a blue selection box over several files.

1. You do not want to call `InvalidateRect` and wait for a `WM_PAINT` message every single time the mouse moves one pixel. That would be too slow and cause flickering.
2. Instead, inside `WM_MOUSEMOVE`, you call `GetDC(hwnd)`.
3. You draw the blue rectangle outline directly to the screen.
4. You immediately call `ReleaseDC(hwnd, hdc)`.

Anything you draw with `GetDC` is temporary. Because you didn't draw it inside `WM_PAINT`, the OS has no record of it. If the user minimizes the window and restores it, whatever you drew with `GetDC` will instantly vanish, because the OS will ask `WM_PAINT` to redraw the screen, and `WM_PAINT` only knows about your permanent data.

Sibling of `GetDC` is [[#GetWindowDC]] that is used to manually draw menu bar or borders of the window

Here are the other methods used to obtain or create a Device Context:

### 1. `GetWindowDC(hwnd)` — The Border Breaker

- **Purpose:** Standard `GetDC` only gives you a passport to the "Client Area" (the inside of your window). `GetWindowDC` gives you a passport to the _entire_ window, including the non-client areas like the title bar, the close button, and the resizing borders.
- **Use Case:** You use this when handling the `WM_NCPAINT` (Non-Client Paint) message if you want to draw a custom title bar or custom window borders instead of letting the OS do it.
- Standard `GetDC` stops at the client area (the inside of the window). If you want to bypass the OS completely and draw your own custom title bar, close buttons, or borders, you use `GetWindowDC`. This gives you a passport to the absolute edges of the application frame. However, stepping outside the client area means you must manually take over the `WM_NCPAINT` (Non-Client Paint) messages, stripping the OS of its default border-drawing duties.

### 2. `CreateCompatibleDC(hdc)` — The Invisible Canvas (Double Buffering)

- **Purpose:** This does not get a DC for the screen. Instead, it asks the OS to create a "Memory Device Context"—an invisible, virtual canvas in RAM that has the exact same color properties as your actual screen.
- **Use Case:** This is mandatory for high-performance graphics. If you draw 50 icons directly to the screen one by one, the user will see them flicker. Instead, you create a memory DC, draw all 50 icons invisibly in RAM, and then use `BitBlt()` to blast the finished image onto the physical screen in one massive, instantaneous transfer.

### 3. `CreateDC(lpszDriver, lpszDevice, ...)` — The Hardware Target

- **Purpose:** This creates a Device Context for a specific physical piece of hardware, bypassing the window entirely.
- **Use Case:** If you want to send a document to a printer, you use `CreateDC("WINSPOOL", "HP LaserJet", ...)` to get a printer DC. You then use standard GDI functions like `TextOut`, and the OS translates them into printer ink instead of screen pixels. You can also use this to get a DC for the entire physical monitor (displaying graphics across the whole screen, ignoring window boundaries).

### 4. `CreateIC(...)` — The Information Context

- **Purpose:** An Information Context is a highly optimized, read-only Device Context.
- **Use Case:** Sometimes you don't want to draw anything; you just need to ask the hardware a question. For example, _"Exactly how many pixels wide is the word 'File' in Arial font?"_ Creating a full DC is slow and uses a lot of memory. `CreateIC` gives you a lightweight passport that allows you to extract metrics and information from the hardware without wasting memory on drawing buffers.

## Why "Device Context" and not "Canvas"?

Modern developers use the word "Canvas" (like in HTML5) because they assume the output is always a grid of pixels on a screen. In the late 1980s and early 1990s, when Windows was engineered, that assumption was entirely false.

**The "Device" part:** Windows was designed to be hardware-independent. Microsoft wanted developers to write a single C function to draw a pie chart, and have that exact same code work on a VGA CRT Monitor, a 9-pin Dot Matrix Printer, and a mechanical pen Plotter.

Therefore, you aren't drawing to a "Canvas." You are issuing commands to a generic **Device**.

- If you call `GetDC(hwnd)`, you get a Device Context for the **Monitor Device**.
- If you call `CreateDC("WINSPOOL", ... )`, you get a Device Context for the **Printer Device**. You pass the exact same `Rectangle()` command to both, and the OS translates it into pixel voltages for the monitor, or ink-head motor steps for the printer.

**The "Context" part:** GDI is a strict **State Machine**. When you draw a line, you don't pass the color, thickness, and style into the `LineTo()` function. The function takes no styling parameters at all.

Instead, the `HDC` holds the "Context" (the current state) of your tools. Inside the memory of that `HDC`, the OS remembers:

- _What is the currently selected Pen?_ (e.g., 2px Solid Red)
- _What is the currently selected Brush?_ (e.g., Hatch Blue)
- _What is the current Font?_ (e.g., Arial 12pt)
- _What are the mapping coordinates?_

### The Golden Rule of GDI

Every Device Context you take from the operating system is a highly restricted system resource. You must strictly follow the paired release mechanisms, or your application will hemorrhage memory and eventually crash the Windows drawing subsystem:

- If you use `BeginPaint`, you **must** use `EndPaint`.
- If you use `GetDC` or `GetWindowDC`, you **must** use `ReleaseDC`.
- If you use `CreateDC`, `CreateCompatibleDC`, or `CreateIC`, you **must** use `DeleteDC`.