---
aliases:
---
```c
int DrawText(
  [in]      HDC     hdc,
  [in, out] LPCWSTR lpchText,
  [in]      int     cchText,
  [in, out] LPRECT  lprc,
  [in]      UINT    format
);
```

`[in] hdc`

A handle to the device context.

`[in, out] lpchText`

A pointer to the string that specifies the text to be drawn. If the _nCount_ parameter is -1, the string must be null-terminated.

If _uFormat_ includes DT_MODIFYSTRING, the function could add up to four additional characters to this string. The buffer containing the string should be large enough to accommodate these extra characters.

`[in] cchText`

The length, in characters, of the string. If _nCount_ is -1, then the _lpchText_ parameter is assumed to be a pointer to a null-terminated string and **DrawText** computes the character count automatically.

`[in, out] lprc`

A pointer to a [[6 - Client Area & Window Area]] structure that contains the rectangle (in logical coordinates) in which the text is to be formatted.

`[in] format`

The method of formatting the text. This parameter can be one or more of the following values.