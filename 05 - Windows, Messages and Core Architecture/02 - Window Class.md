-  A window is always created based on a window class. The window class identifies the window procedure that processes messages to the window.
- More than one window can be created based on a single window class.

For example, all button windows—including push buttons, check boxes, and radio buttons—are created based on the same window class.

When we create a window, we define additional characteristics of the window that are unique to that window.

When we need to use a Window class to create a [[04 - Window and Window Creation|window]] it is necessary to create and register a class first through the function [[#`RegisterClass` Function|RegisterClass]].

---
## `RegisterClass` Function

This function requires a single parameter, which is a pointer to a structure of type [[02 - Window Class|WNDCLASS]] 

This structure includes two fields that are pointers to character strings, so the structure is defined two different ways in the WINUSER.H header file. First, there’s the [[1 - Early Character Encodings & The Rise of ASCII#ASCII|ASCII version]] and other is [[5 - Unicode|Unicode]], WNDCLASSA: 

```c
typedef struct tagWNDCLASSA 
{ 
	UINT style ; 
	WNDPROC lpfnWndProc ; 
	int cbClsExtra ; 
	int cbWndExtra ; 
	HINSTANCE hInstance ; 
	HICON hIcon ; 
	HCURSOR hCursor ; 
	HBRUSH hbrBackground ; 
	LPCSTR lpszMenuName ; 
	LPCSTR lpszClassName ; 
} WNDCLASSA, * PWNDCLASSA, NEAR * NPWNDCLASSA, FAR * LPWNDCLASSA ;
```

```c
// Unicode
typedef struct tagWNDCLASSW { 
	UINT style ; 
	WNDPROC lpfnWndProc ; 
	int cbClsExtra ; 
	int cbWndExtra ; 
	HINSTANCE hInstance ; 
	HICON hIcon ; 
	HCURSOR hCursor ; 
	HBRUSH hbrBackground ; 
	LPCWSTR lpszMenuName ; 
	LPCWSTR lpszClassName ; 
} WNDCLASSW, * PWNDCLASSW, NEAR * NPWNDCLASSW, FAR * LPWNDCLASSW ;
```

The two most important fields in the WNDCLASS structure are the second and the last. The second field (`lpfnWndProc`) is the address of a window procedure used for all windows based on this class. In HELLOWIN.C, this window procedure is `WndProc`. The last field is the text name of the window class.

This can be whatever you want. In pro grams that create only one window, the window class name is commonly set to the name of the program.

## Details on properties of WNDCLASS

1. `style` - Style is defined in [[02.1 - WNDCLASS Styles]]
2. `lpfnWndProc` - This is the pointer to the [[03 - Window Procedure|window procedure]] that you want to use for the program or a set of windows in your program.
3. `cbClsExtra` - Read more here - [[02.2 - cbClsExtra and cbWndExtra]]
4. `cbWndExtra` - Read more here - [[02.2 - cbClsExtra and cbWndExtra]]
5. `hInstance` - Handle (memory address) of program in memory.
6. `hIcon` - This field saves a `Handle to Icon` for window and is loaded with [[3 - LoadIcon|LoadIcon]]. ^dc8f54
7. `hCursor` - Similar to hIcon, The [[4 - LoadCursor]] function loads a predefined mouse cursor known as IDC_ARROW and returns a handle to the cursor.
8. `hbrBackground` - The hbr prefix of the hbrBackground field name stands for “handle to a brush.” A brush is a graphics term that refers to a colored pattern of pixels used to fill an area. The GetStockObject call shown here returns a handle to a white brush: `GetStockObject (WHITE_BRUSH)`.
9. 