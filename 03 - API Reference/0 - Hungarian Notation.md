---
tags:
---
This system involves prefacing the variable name with a short prefix that indicates the variable’s data type, `szCmdLine` is an example of Hungarian notation. Many Windows programmers use this variable-naming convention called Hungarian notation in honor of the legendary Microsoft programmer Charles Simonyi

Simply speaking 
1. variable name begins with a lowercase letter or letters that denote the data type of the variable
2. When naming structure variables, you can use the structure name (or an abbrevia tion of the structure name) in lowercase either as a prefix to the variable name or as the entire variable name

Hungarian notation helps you avoid errors in your code before they turn into bugs. Because the name of a variable describes both the use of a variable and its data type, you are much less likely to make coding errors involving mismatched data types.

| Prefix Data | Type                                            |
| ----------- | ----------------------------------------------- |
| c           | char or WCHAR or TCHAR                          |
| by          | BYTE (unsigned char)                            |
| n           | short                                           |
| i           | int                                             |
| x, y        | int used as x-coordinate or y-coordinate        |
| cx, cy      | int used as x or y length; c stands for “count” |
| b of f      | BOOL (int); f stands for “flag”                 |
| w           | WORD (unsigned short)                           |
| l           | LONG (long)                                     |
| dw          | DWORD (unsigned long)                           |
| fn          | function                                        |
| s           | string                                          |
| sz          | string terminated by 0 character                |
| h           | handle                                          |
| p           | pointer                                         |

---
Much more used in Chapter 5 refer to:
- [[2 - MessageBox]]
- [[02 - Window Class]]
- [[03 - Window Procedure]]
- [[01 - Core architectural overview]]
- [[04 - Window and Window Creation]]