Window procedure is a user defined function that is exposed to the Operating System (`Windows`), which is used by OS to send [[01 - Core architectural overview#^aa187c|messages]] to the program that user is writing.

These messages are then later used by the user to modify the program maybe like resize, click, keyboard event and so on.
A window procedure processes [[01 - Core architectural overview#Window Messages|messages]] to the window. Very often these messages inform a window of user input from the keyboard or the mouse.

Every window that is created for a program need to have a Window Procedure

```c
LRESULT WndProc(HWND hWnd, UINT msg, WPARAM wParam, LPARAM lParam) {
	switch (msg) {
		case WM_CREATE: {
			// User logic to handle what happens on creation like
			// loading data or such things
			break;
		}
		case WM_CLOSE: {
			// Initialization of closing the program when X is pressed.
			PostQuitMessage(0);
			return 0;
		}
		case WM_DESTROY: {
			// Destruction of the Window object
			PostQuitMessage(0);
			return 0;
		}
	}
	return DefWindowProc(hWnd, msg, wParam, lParam);
}
```

The WndProc function returns a value of type LRESULT. That’s simply defined as a LONG.

---

>[!info] More about WPARAM and LPARAM
>Visit [[5 - WPARAM and LPARAM]] to know what W and L in PARAM means and why are they named in such a manner.
>Unlike [[8 - Near and Long pointers|Near and Long abbreviation]] the [[5 - WPARAM and LPARAM|WPARAM and LPARAM abbreviation]] is still useful and used in todays programming.

