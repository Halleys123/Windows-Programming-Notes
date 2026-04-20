This page documents the "modern" Win32 scrolling architecture introduced in Windows 95. While visually dated today, this is the strict mathematical standard required for any raw C/C++ Win32 application that uses OS-provided scroll bars.

### The Architectural Shift: The Viewport (`nPage`)

This API was created to solve the fatal flaw of the legacy API by introducing the concept of the **Viewport** (the `nPage` variable). By telling the OS exactly how much of the document is currently visible on the glass monitor, the Windows Kernel automatically handles:

1. **Proportional Thumbs:** The thumb dynamically grows and shrinks as the user resizes the window.
2. **Mathematical Clamping:** The OS strictly prevents the user from scrolling past `(nMax - nPage) + 1`, completely eliminating "empty space" scrolling bugs.

### The Core Structure: `SCROLLINFO`

Instead of calling multiple functions to set range and position, you populate this single struct and pass it to the OS.

```C
typedef struct tagSCROLLINFO {
  UINT cbSize;    // Must be set to sizeof(SCROLLINFO)
  UINT fMask;     // Flags telling the OS which variables below you actually want to update
  int  nMin;      // Minimum scrolling position (usually 0)
  int  nMax;      // Maximum scrolling position (Total document length)
  UINT nPage;     // The Viewport: How much fits on the screen right now
  int  nPos;      // The current position of the thumb
  int  nTrackPos; // Used during SB_THUMBTRACK to get real-time dragging coordinates
} SCROLLINFO;
```

The attributes inside `SCROLLINFO` are completely **abstract mathematical units**.

If you decide `1` means "one line of text," then the scroll bar operates in lines. If you decide `1` means "one pixel," then it operates in pixels. If you decide `1` means "one entire page of a PDF," it operates in pages.
### The Core Functions

#### 1. `SetScrollInfo`

The universal setter.

```C
int SetScrollInfo(
  HWND            hwnd,
  int             nBar, // SB_VERT or SB_HORZ
  LPCSCROLLINFO   lpsi, // Pointer to your configured struct
  BOOL            redraw
);
```

**Important `fMask` Flags:**

- `SIF_RANGE`: Tells the OS to read your `nMin` and `nMax` variables.
- `SIF_PAGE`: Tells the OS to read your `nPage` variable and recalculate thumb size.
- `SIF_POS`: Tells the OS to physically move the thumb to your `nPos` variable.
- `SIF_ALL`: Reads Range, Page, and Position all at once.

#### 2. `GetScrollInfo`

The universal getter. You must set the `cbSize` and `fMask` before calling this, so the OS knows exactly which pieces of data you are asking it to retrieve.

```C
BOOL GetScrollInfo(
  HWND          hwnd,
  int           nBar,
  LPSCROLLINFO  lpsi
);
```