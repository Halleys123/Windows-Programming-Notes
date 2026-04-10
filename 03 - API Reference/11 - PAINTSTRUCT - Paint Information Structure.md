
```c
typedef struct tagPAINTSTRUCT {
  HDC  hdc;
  BOOL fErase;
  RECT rcPaint;
  BOOL fRestore;
  BOOL fIncUpdate;
  BYTE rgbReserved[32];
} PAINTSTRUCT, *PPAINTSTRUCT, *NPPAINTSTRUCT, *LPPAINTSTRUCT;
```

This structure saves the size of minimum rectangle that should be erased/repainted when some regions of the window are invalidated 

![[Invalid_Region_multiple_windows.excalidraw|800]]

### `hdc`

A handle to the display DC to be used for painting.

### `fErase`

Indicates whether the background must be erased. This value is nonzero if the application should erase the background. The application is responsible for erasing the background if a window class is created without a background brush. For more information, see the description of the [[02 - Window Class#^efdbd9|hbrBackground]] member of the [[02 - Window Class]] structure.

### `rcPaint`

A [RECT](https://learn.microsoft.com/en-us/windows/desktop/api/windef/ns-windef-rect) structure that specifies the upper left and lower right corners of the rectangle in which the painting is requested, in device units relative to the upper-left corner of the client area.

### fRestore, fIncUpdate, rgbReserved

All are internally reserved for windows usage. may study in future when studying about windows internals.