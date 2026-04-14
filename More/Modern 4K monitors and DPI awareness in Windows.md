---
tags:
  - ai-generated
---

In the web world, the W3C solved this by separating the concept of a **"CSS Pixel"** from a **"Physical Device Pixel."** When a web developer writes `width: 100px`, the browser engine checks the monitor's `window.devicePixelRatio`. If it is a 4K phone screen, the browser silently multiplies that `100px` by 3 or 4, drawing it with 300 or 400 physical hardware pixels. The developer never has to do the math.

Windows could not do this for legacy Win32 applications because the Win32 API does not have an abstraction layer like a browser engine. When a C programmer in 1995 called `Rectangle(hdc, 0, 0, 100, 100)`, they were commanding the hardware to alter exactly 100 physical points of light.

Because Microsoft refuses to break backward compatibility, they handle High-DPI monitors by dividing Windows applications into three distinct architectural categories, known as **DPI Awareness Levels**.

### 1. DPI Unaware (The Blurry Lie)

If you compile a standard Win32 C program today and do not explicitly tell the OS otherwise, Windows assumes your program is from 1998.

**The Mechanism:**

- When your app launches on a 4K monitor (e.g., running at 200% scale), the Desktop Window Manager (DWM) actively **lies** to your application.
    
- If you call [[12 - GetSystemMetrics|GetSystemMetrics]] to ask how big the screen is, Windows returns fake, smaller coordinates (the 100% scale equivalent).
    
- Your application happily draws its 100x100 pixel button, thinking everything is fine.
    
- Finally, the DWM compositor steps in, takes your finished, low-resolution window texture, and forcefully stretches it to 200% using the GPU.
    
- **The Result:** The application works perfectly, the layout doesn't break, but the text and UI elements look blurry, pixelated, and completely unreadable, exactly like zooming in on a low-resolution JPEG.
    

### 2. System DPI Aware (The Single Monitor Trap)

This was Microsoft's first attempt to fix the problem in the Windows 7 era. You add a manifest file to your `.exe` declaring, _"I am DPI Aware. Do not lie to me."_

**The Mechanism:**

- When the app launches, Windows tells it the true DPI of the primary monitor.
    
- Your C code must look at that DPI, calculate the scale factor (e.g., `CurrentDPI / 96.0`), and manually multiply every single coordinate, font size, and button width by that scale factor before drawing.
    
- **The Result:** The app looks razor-sharp and perfect on the main monitor.
    
- **The Failure:** If the user has a dual-monitor setup (e.g., a 4K laptop screen at 200% next to a 1080p monitor at 100%), and they drag your window from the 4K screen to the 1080p screen, your app stays permanently scaled at 200%. It becomes massively oversized and unusable on the second monitor because the app only checked the DPI _once_ at startup.
    

### 3. Per-Monitor DPI Aware V2 (The Modern Holy Grail)

This is the modern standard introduced in Windows 10, and it is exactly what you must implement for your File Explorer.

**The Mechanism:** You tell the OS that you are fully capable of handling real-time monitor transitions. Windows stops stretching your app and stops lying to you. Instead, it introduces a new message to your `WndProc`: **`WM_DPICHANGED`**.

When a user drags your window across the physical boundary between a 4K monitor and a 1080p monitor, the OS fires this message. The message contains two critical pieces of data:

1. The integer of the _new_ monitor's DPI.
2. A pointer to a new `RECT` containing the exact mathematical coordinates your window _should_ be resized to, so it doesn't suddenly appear huge or tiny to the user's eye.

**Your Responsibility:** Inside `WM_DPICHANGED`, your code must catch the new DPI, destroy all your current fonts, create new fonts scaled to the new DPI, mathematically recalculate the layout of every single child window (buttons, list views), and call `SetWindowPos` to snap the window to the OS-suggested `RECT`.


### Why Direct2D is the Ultimate Weapon Here

If you attempt to handle `WM_DPICHANGED` using legacy GDI, the math is a nightmare. You have to manually multiply every coordinate for every rectangle.

This is where Direct2D proves it is not bloat. Direct2D natively operates on **Device Independent Pixels (DIPs)**—the exact same concept as a "CSS Pixel" on the web. When you initialize Direct2D, you pass it the monitor's DPI. You then draw everything using abstract coordinates. If you say `DrawRectangle(100, 100)`, Direct2D automatically does the math on the GPU to scale it to the physical pixels. When `WM_DPICHANGED` fires, you simply tell Direct2D the new DPI value, and the GPU instantly recalculates and repaints the entire UI razor-sharp, with zero manual math required from your C++ code.