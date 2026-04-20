Scroll bars are positioned either vertically (for up and down movement) or horizontally (for left and right movement).

### The Terminology Trap

> _"Programmers sometimes have problems with scrolling terminology because their perspective is different from the user’s. A user who scrolls down wants to bring a lower part of the document into view."_ — Charles Petzold

To maintain strict architectural clarity:

- **Scroll Up:** Moving toward the _beginning_ of the document.
- **Scroll Down:** Moving toward the _end_ of the document.

$$
\frac {Thumb\space Size} {Scroll\space Length} = \frac {Client\space Height} {Range} = \frac {Visible Length\space of\space document} {Total\space Length\space of\space document}
$$

![[ScrollBar.png]]

## Custom Scroll Bars (The "Borderless" Approach)

The standard Win32 scroll bars invoked by the `WS_VSCROLL` and `WS_HSCROLL` window styles are rigidly controlled by the operating system. You cannot change their width, color, or hover states.

To build a modern, auto-hiding, or custom-themed scroll bar (like those seen in VS Code or modern browsers), you must completely abandon the OS-provided scroll bars and render your own.

### 1. Removing the OS Dependency

Do not pass `WS_VSCROLL` or `WS_HSCROLL` into `CreateWindowEx`. The window must be completely clean. ^a18948

### 2. The Retained Mode State

Your custom scroll bar is no longer an OS concept; it is just another geometric UI node in your Retained Mode memory tree. You must track its mathematical state manually:

- `Track Rect`: The absolute coordinates of the scroll bar background (e.g., a 10px wide rectangle anchored to the right edge of `cxClient`).
- `Thumb Rect`: The draggable box.
- `Hover State`: A boolean tracking if the mouse is currently over the track or thumb.
- `Dragging State`: A boolean tracking if the user is currently holding the left mouse button down on the thumb.
### 3. The Mathematics of the Custom Thumb

Instead of passing `nPage` and `nMax` to the OS, you use them to calculate your own pixel geometry.

- **Thumb Height:** `(Viewport_Height / Total_Document_Height) * Track_Height`
- **Thumb Y-Position:** `(Current_Scroll_Y / Total_Scrollable_Distance) * (Track_Height - Thumb_Height)`

### 4. Taking Over the Message Queue

Because the OS no longer manages the scroll bar, you must intercept raw mouse hardware messages and translate them into scrolling logic:

- **`WM_MOUSEMOVE`:** * Check if the mouse coordinates intersect your `Track Rect`. If yes, update your UI tree to trigger a hover color change and call `InvalidateRect`.
    
    - If `Dragging State` is true, calculate the Delta Y of the mouse, mathematically map that to document pixels, update your document's Y offset, and redraw.

- **`WM_LBUTTONDOWN`:** * If the mouse is over the `Thumb Rect`, set `Dragging State = TRUE`.
    
    - **Crucial:** Call `SetCapture(hwnd)`. This forces the OS to keep sending your window mouse messages even if the user accidentally drags their cursor completely off your application screen while holding the thumb

- **`WM_LBUTTONUP`:** * Set `Dragging State = FALSE`.
    
    - Call `ReleaseCapture()`.
## API Reference Architecture

The raw Win32 scrolling API is documented in the following sub-pages depending on the target architecture:

- [[15 - Legacy Scroll Bar (16-bit Architecture)|Scroll Bar Legacy]] Covers 16-bit legacy functions (e.g., `SetScrollRange`, `SetScrollPos`) that lack viewport awareness.    
- [[16 - Modern Scroll Bar (32-Bit Architecture)|ScrollBar Modern]]: Covers the 32-bit/64-bit standard (e.g., `SetScrollInfo`, `SCROLLINFO` struct) required for proportional thumb sizing and modern DWM scaling.
- [[05.1 - Custom Scroll Bar - Child Window Architecture|Child Window Architecture]] - This is a better approach to implement a custom scroll bar. 