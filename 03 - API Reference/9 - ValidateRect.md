
The **ValidateRect** function validates the client area within a rectangle by removing the rectangle from the update region of the specified window.

```c
BOOL ValidateRect(
  [in] HWND       hWnd,
  [in] const RECT *lpRect
);
```

## Parameters

`[in] hWnd`

Handle to the window whose update region is to be modified. If this parameter is **NULL**, the system invalidates and redraws all windows and sends the **WM_ERASEBKGND** and **WM_NCPAINT** messages to the window procedure before the function returns.

`[in] lpRect`

Pointer to a [RECT](https://learn.microsoft.com/en-us/windows/desktop/api/windef/ns-windef-rect) structure that contains the client coordinates of the rectangle to be removed from the update region. If this parameter is **NULL**, the entire client area is removed.