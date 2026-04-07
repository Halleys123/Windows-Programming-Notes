This function is used to load a Handle to a pre loaded icon.
To obtain a handle to a predefined icon, you call LoadIcon with the first argument set to NULL

When you’re loading your own customized icons that are stored in your program’s .EXE file on disk, this argument would be set to hInstance, the instance handle of the program. The second argument identifies the icon. For the predefined icons, this argument is an identifier beginning with the prefix IDI (“ID for an icon”).

## Syntax

```c
HICON LoadIconW(
  [in, optional] HINSTANCE hInstance,
  [in]           LPCWSTR   lpIconName
);
```

`hInstance`:
- To load a predefined system icon, set this parameter to **NULL**.
- A handle to the module of either a DLL or executable (.exe) file that contains the icon to be loaded.

 `lpIconName`:
 - If _hInstance_ is non-**NULL**, _lpIconName_ specifies the icon resource either by name or ordinal. This ordinal must be packaged by using the [MAKEINTRESOURCE](https://learn.microsoft.com/en-us/windows/desktop/api/winuser/nf-winuser-makeintresourcew) macro.
 - If _hInstance_ is **NULL**, _lpIconName_ specifies the [identifier (beginning with the IDI_ prefix)](https://learn.microsoft.com/en-us/windows/win32/menurc/about-icons) of a predefined system icon to load.

## Uses

1. Used to set window Icon - [[02 - Window Class#^dc8f54|Settings hIcon in WNDCLASS]].
2. used to load custom icons