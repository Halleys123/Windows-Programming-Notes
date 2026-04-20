This page documents the original scroll bar API introduced in the 16-bit era of Windows (Windows 3.1).

>[!warning] **Architectural Warning:** 
>These functions are strictly obsolete for modern document viewing. They are completely unaware of the concept of a "Viewport" or a "Page Size," meaning they cannot mathematically calculate the proportional height of a scroll thumb. If you use these functions, the operating system will always draw the scroll thumb as a tiny, fixed-size square block.

### The Core Functions

#### 1. `SetScrollRange`

Establishes the absolute mathematical boundaries of the document.

```C
BOOL SetScrollRange(
  HWND hWnd,
  int  nBar,      // SB_VERT or SB_HORZ
  int  nMinPos,   // Usually 0
  int  nMaxPos,   // The total length of the document
  BOOL bRedraw    // TRUE to instantly paint, FALSE to batch updates
);
```

#### 2. `SetScrollPos`

Moves the scroll thumb to a specific mathematical integer within the previously defined range.

```C
int SetScrollPos(
  HWND hWnd,
  int  nBar,
  int  nPos,      // The new position of the thumb
  BOOL bRedraw
);
```

#### 3. `GetScrollRange` & `GetScrollPos`

Query functions used to retrieve the current state of the scroll bar from the OS memory.

### Why It Failed

In this architecture, if `nMaxPos` is 1000, and the user's window is tall enough to display 900 lines, the user should never be allowed to drag the thumb past position 100. However, because `SetScrollRange` does not know the window height, it will allow the user to drag the thumb all the way to 1000, resulting in the user staring at 900 lines of empty black space. You have to manually calculate and clamp the position inside `WM_VSCROLL`.


## Scroll Bar Messages

Windows sends the window procedure WM_VSCROLL (vertical scroll) and WM_HSCROLL
(horizontal scroll) messages when the scroll bar is clicked with the mouse or the thumb is
dragged.

The **wParam** **message** parameter is divided into a low word and a high word. The
low word of **wParam** is a number that indicates what the mouse is doing to the scroll bar.

This number is referred to as a “notification code.” Notification codes have values defined
by identifiers that begin with SB, which stands for “scroll bar.” Here’s how the notification
codes are defined in WINUSER.H:

```C
#define SB_LINEUP 0
#define SB_LINELEFT 0
#define SB_LINEDOWN 1
#define SB_LINERIGHT 1
#define SB_PAGEUP 2
#define SB_PAGELEFT 2
#define SB_PAGEDOWN 3
#define SB_PAGERIGHT 3
#define SB_THUMBPOSITION 4
#define SB_THUMBTRACK 5
#define SB_TOP 6
#define SB_LEFT 6
#define SB_BOTTOM 7
#define SB_RIGHT 7
#define SB_ENDSCROLL 8
```

![[ScrollBar - Notification messages.png]]

>[!info]
>lParam is not used in normal Scroll bar that is created by adding [[05 - Scroll Bars (Core Concepts & Custom Implementation)#^a18948|WS_HSCROLL or WS_HVCROLL]] in [[01 - Core architectural overview#^7b45ae|CreateWindow]]. This is only used when a special scroll bar is used that is created using:
>
>```C
>CreateWindowEx(0, "SCROLLBAR", ... WS_CHILD ...)
>```
>
>In this case lParam tell which scroll bar is this message referring to as you can create multiple such scroll bars unlike normal scroll which can be created only one (Horizontal and Vertical) on single window. 

