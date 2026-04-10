
When ever a window program is overlapped with some other window, icon or any element it often needs to be repainted that region is called **Invalid Region** and rest of the window is called **Valid Region**

![[Invalid_Region_single.excalidraw|700]]

Repainting is required only when there is such an invalid Region present. When such an invalid region is present then windows place a WM_PAINT message in the queue.

This information is maintained in a structure called `Paint Information Structure`.

## Validating or Invalidating a region

Usually to make a region a valid again, we start with [[01.2 - WM_PAINT|WM_PAINT]] message, when this message is received in the queue, we can call [[01.2 - WM_PAINT#`BeginPaint` Function|BeginPaint function]] or use [[9 - ValidateRect|ValidateRect]] function that can be used to validate rectangle or arbitrary size in the client area of the window.

Similarly to make an area invalid we can use [[10 - InvalidateRect|InvalidateRect function]]. If the message queue already contains a WM_PAINT message, Windows calculates a new invalid rectangle. Otherwise, it places a WM_PAINT message in the message queue.