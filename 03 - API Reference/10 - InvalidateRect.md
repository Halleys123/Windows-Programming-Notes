The **InvalidateRect** function adds a rectangle to the specified window's update region. The update region represents the portion of the window's client area that must be redrawn.

If the message queue already contains a WM_PAINT message, Windows calculates a new invalid rectangle. Otherwise, it places a WM_PAINT message in the message queue.

```c
BOOL InvalidateRect(
  [in] HWND       hWnd,
  [in] const RECT *lpRect,
  [in] BOOL       bErase
);
```

`[in] hWnd`

A handle to the window whose update region has changed. If this parameter is **NULL**, the system invalidates and redraws all windows, not just the windows for this application, and sends the `WM_ERASEBKGND` and `WM_NCPAINT` messages before the function returns. Setting this parameter to **NULL** is not recommended.

`[in] lpRect`

A pointer to a [[6 - Client Area & Window Area#3. The `RECT` Structure|RECT]] structure that contains the client coordinates of the rectangle to be added to the update region. If this parameter is **NULL**, the entire client area is added to the update region.

`[in] bErase`

Specifies whether the background within the update region is to be erased when the update region is processed. If this parameter is **TRUE**, the background is erased when the [[01.2 - WM_PAINT#`BeginPaint` Function|BeginPaint]] function is called. If this parameter is **FALSE**, the background remains unchanged.