Similar to to how web developers or mobile developers work, Windows developers also can't assume that size of window will be fixed. Just like how mobile device may change, or browser window size may alter on the users whim, similarly Windows in Microsoft windows also can change their size. 

So your program needs to be ready for how to handle that change, how to move the layout and change elements accordingly.

So in windows programming or any low level programming you can't work on assumptions as even the strongest of assumptions break. 
>[!fact] for example recently
 **the widespread adoption of 4K, 8K, and High-DPI (Dots Per Inch) monitors completely shattered the decades-old assumption that a "pixel" represents a fixed physical measurement.** In the 1990s and early 2000s, developers assumed all monitors ran at a standard 96 DPI. They hardcoded dialog boxes to be exactly 400x300 pixels, and buttons to be 80x25 pixels. That assumption held for twenty years. But if you drag that exact same Win32 program onto a modern 4K laptop screen today—which packs four times as many pixels into the exact same physical glass—those hardcoded buttons shrink to the size of microscopic specks, rendering the application entirely unusable. The program failed because it trusted a fixed coordinate system instead of calculating relative scale.


Using a system of flex or grid is one of the ways people normally handle the layout on screen.

## Painting and Repainting

Normally when a windows program is written the flow of program can either of the following

### Automatic WM_PAINT message

This flow occurs when the application relies entirely on the operating system's idle state to manage rendering. It is the asynchronous, low-priority pathway.

1. **Invalidation:** An event occurs that damages the window's visual state. This could be the initial `ShowWindow` call, the user resizing the window, or another application moving out of the way and exposing your window.
2. **The Update Region:** The operating system calculates the exact rectangular coordinates of the damaged area and adds them to the window's internal "Update Region." The window is now flagged as "dirty," but nothing is drawn yet.
3. **Queue Exhaustion:** The application's primary thread continues executing its `while (GetMessage(...))` loop, processing high-priority hardware events like mouse clicks and keystrokes first.
4. **Synthesis:** When the message queue is completely empty and the thread is about to go to sleep, Windows checks the Update Region. Seeing that it is dirty, the OS automatically synthesizes a `WM_PAINT` message and places it at the very bottom of the queue.
5. **Execution:** `GetMessage` pulls this synthesized message, dispatches it, and the `WndProc` finally paints the screen.
### Manual WM_PAINT message

This flow occurs when the programmer demands immediate visual updates, bypassing the standard queue mechanism. It is the synchronous, high-priority pathway.

1. **Invalidation:** Just like the automatic flow, a region of the [[02 - Valid and Invalid Rectangles|window is invalidated]] (for example, by the programmer explicitly calling `InvalidateRect(hwnd, NULL, TRUE)`). The [[02 - Valid and Invalid Rectangles|Update region]] is marked as dirty.    
2. **The Direct Command:** Instead of waiting for the message loop to eventually find the dirty region, the programmer immediately calls the `UpdateWindow(hwnd)` function.
3. **The Queue Bypass:** [[8 - UpdateWindow|UpdateWindow]] looks at the Update Region. Seeing that it is dirty, it completely ignores the `GetMessage` loop and the message queue.
4. **Direct Invocation:** The operating system Kernel forces a synchronous function call directly into your `WndProc`, passing the `WM_PAINT` message.
5. **Execution:** Your `WndProc` paints the window instantly and clears the Update Region. When `UpdateWindow` finally returns control to the next line of your C code, you have an absolute mathematical guarantee that the pixels on the monitor have been updated.

Read more about [[01.2 - WM_PAINT|WM_PAINT here]]