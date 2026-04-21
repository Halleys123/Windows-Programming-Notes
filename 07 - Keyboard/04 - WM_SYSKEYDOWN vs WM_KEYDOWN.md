When you press a key, Windows places either a WM_KEYDOWN or WM_SYSKEYDOWN message in the message queue of the window with the input focus. When you release a key, Windows places either a WM_KEYUP or WM_SYSKEYUP message in the message queue.

The message that is placed depends on whether they key pressed is system key or not.

## Keyboard Input: Message Timestamps (`GetMessageTime`)

When a hardware interrupt occurs (you press a key), the OS immediately places that message into the System Message Queue. At that exact millisecond, Windows "stamps" the message with the current system time (uptime in milliseconds). 

**The Critical Distinction:** There is a difference between *Event Time* and *Processing Time*. 
If your application's main thread is temporarily busy (e.g., allocating memory for a large document in your Markdown editor), messages will sit in the queue waiting. 
* If you call `GetTickCount()` inside your `WM_KEYDOWN` handler, you get the time you *processed* the message.
* If you call `GetMessageTime()` inside your `WM_KEYDOWN` handler, you get the exact time the hardware was *physically struck*.

## How to Use It
`GetMessageTime()` is a Win32 API function that takes no parameters. It simply returns a `LONG` representing the timestamp of the last message retrieved by `GetMessage`.

```cpp
LRESULT CALLBACK WndProc(HWND hwnd, UINT message, WPARAM wParam, LPARAM lParam)
{
    static LONG lastShiftTime = 0;

    switch (message)
    {
        case WM_KEYDOWN:
        {
            if (wParam == VK_SHIFT) 
            {
                LONG currentPressTime = GetMessageTime();
                
                // Check if this is a double-tap within 300 milliseconds
                if ((currentPressTime - lastShiftTime) < 300) 
                {
                    // Execute double-tap logic (e.g., open command palette)
                }
                
                lastShiftTime = currentPressTime;
            }
            return 0;
        }
        // ... other messages
    }
    return DefWindowProc(hwnd, message, wParam, lParam);
}
```

>[!note] 
>Because the time wraps around to zero if the system runs continuously for ~49.7 days, always subtract the previous time from the current time rather than comparing absolute values.

---

Read more about system messages and their use cases [[04.1 - SYS Messages|here]].
