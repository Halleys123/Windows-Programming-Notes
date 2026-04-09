---
cssclasses:
  - no-callout-icon
---
When programming for Windows, you are engaging in a specialized form of object-oriented programming, where the primary object is the [[04 - Window and Window Creation|Window]]. 

The most common type of window is an application window, such as a browser or File Explorer. However, this represents only a fraction of the architecture.

Furthermore, nearly every element contained within that primary window is also a window itself. These are specifically classified as **child windows**, and they include:
1. Menus
2. Radio Button.
3. Button
4. Dropdown
5. Scroll Bar
6. Text Fields
7. etc.

From a user's perspective, these elements receive input. From a programmer's perspective, this input is delivered by the operating system strictly in the form of **[[#Window Messages|messages]]**.

## Window Messages

^aa187c

We know that a user can easily close or resize a window, or provide keyboard and mouse input. However, logically, a computer does not handle these actions automatically. This tracking is handled by the Windows OS itself, not the programmer. 

Whenever an event occurs, it is the responsibility of the OS to notify your program that something has happened to your **window**. The programmer then writes the logic to react to that notification.

>[!question|noicon]-  Think again about above paragraph, how does OS even send a message to your program?

To allow an external entity to interact with our program, we must expose a function to it. In Windows, this exposed function is called the [[03 - Window Procedure]]. It is a specialized call-back function that the OS utilizes to dispatch messages to our application logic.

### Message Queue and Loop

When a Windows program begins execution, the OS creates a "message queue" dedicated to that program. This queue temporarily stores messages intended for any window the program might create.

To process these, a Windows application utilizes a continuous loop of code called the "message loop." This loop retrieves messages from the queue and dispatches them to the appropriate window procedure.

![[Message Loop.excalidraw|600]]

Before dispatching, the loop typically calls `TranslateMessage`. This function intercepts virtual-key messages (raw keyboard hardware inputs) and translates them into readable character messages.

Finally, `DispatchMessage` instructs the OS to send the retrieved message to the specific [[03 - Window Procedure]] associated with the target window. The destination is already present in the [[#Structure of a message|message struct]] itself that is sent by the OS to our program, our program will just relay it to the correct window.

```C
#include <Windows.h>

LRESULT wndProc(HWND hWnd, UINT msg, WPARAM wParam, LPARAM lParam) {
	switch (msg) {
	case WM_CLOSE:ś
		PostQuitMessage(0);
		return 0;
	case WM_DESTROY:
		DestroyWindow(hWnd);
		return 0;
	}

	return DefWindowProc(hWnd, msg, wParam, lParam);
}

int WINAPI WinMain(_In_ HINSTANCE hInstance, _In_opt_ HINSTANCE hPrevInstance, _In_ LPSTR lpCmdLine, _In_ int nCmdShow) {
	WNDCLASS wc = { 0 };
	wc.hInstance = hInstance;
	wc.lpszClassName = TEXT("TestWindow");
	wc.lpfnWndProc = wndProc;
	wc.style = CS_OWNDC;

	ATOM result;
	if ((result = RegisterClass(&wc)) == FALSE) {
		MessageBox(NULL, TEXT("Invalid"), TEXT("Invalid"), MB_OK | MB_ICONERROR);
		return 1;
	}

	HWND mainWindowhWnd = CreateWindow(wc.lpszClassName, wc.lpszClassName,                                                WS_VISIBLE | WS_OVERLAPPEDWINDOW, 0, 0,                                            CW_USEDEFAULT, CW_USEDEFAULT, NULL, NULL,                                          hInstance, NULL);

	if (!mainWindowhWnd) {
		MessageBox(NULL, TEXT("No Window"), TEXT("Invalid"), MB_OK | MB_ICONERROR);
		return 1;
	}
	MessageBox(mainWindowhWnd, TEXT("Window will be created successfully without                 owner slave relation"), TEXT("SUCCESS"), MB_OK | MB_ICONINFORMATION);

	MSG msg = { 0 };
	while (GetMessage(&msg, NULL, 0, 0)) {
		TranslateMessage(&msg);
		DispatchMessage(&msg);
	}

	return 0;
}
```

The message loop in our program is following code

```c
	MSG msg = { 0 };
	while (GetMessage(&msg, NULL, 0, 0)) {
		TranslateMessage(&msg);
		DispatchMessage(&msg);
	}
```

^55dd28

### Structure of a message

When your program executes `GetMessage(&msg, NULL, 0, 0)`, the operating system reaches into the thread's queue and fills your `msg` variable with the following precise data points:

1. **`hwnd`**: The exact memory handle of the specific window to which the message is directed.
2. **`message`**: The numerical identifier of the event (e.g., `WM_LBUTTONDOWN` for a mouse click).
3. **`wParam`**: Additional data regarding the event (e.g., which mouse button was clicked).
4. **`lParam`**: Further additional data (e.g., the exact X and Y coordinates of the mouse cursor).
5. **`time`**: The exact system time the event occurred.
6. **`pt`**: The position of the cursor on the screen when the message was posted.

```c
typedef struct tagMSG {
  HWND   hwnd;
  UINT   message;
  WPARAM wParam;
  LPARAM lParam;
  DWORD  time;
  POINT  pt;
  DWORD  lPrivate;
} MSG, *PMSG, *NPMSG, *LPMSG;
```

---
## Static Variable Trap

The orderly, sequential execution of messages is guaranteed _unless the programmer explicitly invites the operating system back in._

If your `WndProc` is processing a message and you call a specific Windows API function—such as `UpdateWindow()` or `SendMessage()`—you are handing control back to the Windows Kernel. The Kernel may decide that this API call requires a new, immediate non-queued message (such as `WM_PAINT`).

The Kernel will immediately execute your `WndProc` a second time, pushing it onto the CPU stack directly on top of the first, currently paused execution. This is **indirect recursion**. The operating system is the invisible middleman causing your function to call itself.

This completely shatters the safety of static variables. Here is the precise mechanical timeline of the failure:

1. **Entry 1:** The `WndProc` receives a mouse click message.
2. **State Set:** The `WndProc` sets a variable: `static int memory = 5;`.
3. **The Trap:** The `WndProc` calls `UpdateWindow()`, pausing its own execution.
4. **Entry 2:** The OS intercepts `UpdateWindow()` and instantly forces a second instance of `WndProc` to handle a `WM_PAINT` message.
5. **State Mutated:** The second instance of `WndProc` alters the variable: `memory = 99;`. It then returns.
6. **Resumption:** The original `UpdateWindow()` call finishes. The first instance of `WndProc` resumes execution.
7. **The Crash:** The first instance expects `memory` to still be `5`. It reads the physical memory address and finds `99`. The logical foundation of the code collapses.

Because a static variable occupies a single, fixed location in physical RAM, both recursive instances of the function are writing to the exact same physical wire.

This is why you may want to avoid global and static variables especially when writing a windows program.

---
Continue Reading on Messages and Handling messages in [[01.1 - Some Message and Processing Messages]]