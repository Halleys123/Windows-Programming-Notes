When you press a key, Windows places either a WM_KEYDOWN or WM_SYSKEYDOWN message in the message queue of the window with the input focus. When you release a key, Windows places either a WM_KEYUP or WM_SYSKEYUP message in the message queue.

The message that is placed depends on whether they key pressed is system key or not.