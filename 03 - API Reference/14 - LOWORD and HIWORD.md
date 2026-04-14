LOWORD and HIWORD are macros that are defined in the Windows header file WINDEF.H. If you’re curious, the definitions of these macros look like this.

These are made so that you don't have to manually decode what [[5 - WPARAM and LPARAM|WPARAM and LPARAM]] values have in them, although you still must know what they hold in case of each `WM_MESSAGE`.

```c
#define LOWORD(l) ((WORD)(l)) 
#define HIWORD(l) ((WORD)(((DWORD)(l) >> 16) & 0xFFFF))
```

The two macros return WORD values—that is, 16-bit unsigned short integers that range from 0 through 0xFFFF.