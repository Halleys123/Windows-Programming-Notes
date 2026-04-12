`GetTextMetrics` is used to get information about the currently selected font and that information is saved in device context. So this function needs `HDC` as input.

```c
BOOL GetTextMetrics(
  [in]  HDC          hdc,
  [out] LPTEXTMETRIC lptm
);
```

The data about font is copied to `lptm` structure that has about 20 fields, but first five are enough to know about the font.

```c
typedef struct tagTEXTMETRIC { 
	LONG tmHeight ; 
	LONG tmAscent ; 
	LONG tmDescent ; 
	LONG tmInternalLeading ; 
	LONG tmExternalLeading ; 
	LONG tmAveCharWidth ; 
	LONG tmMaxCharWidth ; 
		// [other structure fields] 
} TEXTMETRIC, * PTEXTMETRIC ;
```

The values of these fields are in units that depend on the mapping mode currently selected for the device context. In the default device context, this mapping mode is `MM_TEXT`, so the dimensions are in `units of pixels`.

## Use of `GetTextMetrics`

```c
HDC dc = GetDC (hwnd) ; 
GetTextMetrics (hdc, &tm) ; 
ReleaseDC (hwnd, hdc) ;
```

![[character_structure.png]]

### Breakdown of height of character

#### Important fields in TEXTMETRICS

1. `tmHeight`
	- um of `tmAscent` and `tmDescent`
2. `tmAscent` and `tmDescent`
	 - These two values represent the maximum vertical extents of characters in the font above and below the baseline.
3. `tmInternalLeading`
	- it is often the space in which accent marks appear
	- The term “leading” refers to space that a printer inserts between lines of text.

#### Secondary fields in TEXTMETRICS

1. `tmExternalLeading`
	- which is not included in the `tmHeight` value
	- This is an amount of space that the designer of the font suggests be added between successive rows of displayed text.
	- You can accept or reject the font designer’s suggestion for including external leading when spacing lines of text

### Breakdown of width of character

The `TEXTMETRIC` structure contains two fields that describe character widths: 

1. `tmAveCharWidth` field is a weighted average of lowercase characters
2. and `tmMaxCharWidth` is the width of the widest character in the font.

For a fixed-pitch font, these values are the same

- You can calculate this fairly accurately as 150% of `tmAveCharWidth`.

---

It’s important to realize that the dimensions of a system font are dependent on the
pixel size of the video display on which Windows runs and, in some cases, on the system font size the user has selected.

- Don’t write your Windows programs so that they guess at character dimensions. 
- Don’t hard-code any values. 
- Use the `GetTextMetrics` function to obtain this information.

Because the dimensions of the system font do not change during a Windows session,
you need to call `GetTextMetrics` only once when your program executes