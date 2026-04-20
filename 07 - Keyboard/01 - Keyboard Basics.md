Keyboard input is delivered to your program’s window procedures in the form of messages. There are **eight different messages** that Windows uses to indicate various keyboard
events. Although this seem a lot but we can **safely ignore at least half of them**.

Windows itself handles some of the messages, like we have used `Alt + F4` without thinking but that gets handled automatically. Other keys including `Alt` or `Windows Key` combination are also handled by windows and the result is sent to program by the windows.

>[!extra] Handling shortcuts in your program
>To handle shortcuts in your program you create a 2D hash table structure pointing to function pointers, mark alphabets, numbers as reserved and then start with table of following format.
>
| Key Pressed      | A             | B           | C      | D      | E      | F      | G      |
| ---------------- | ------------- | ----------- | ------ | ------ | ------ | ------ | ------ |
| `None`           | `Res.`        | `Res.`      | `Res.` | `Res.` | `Res.` | `Res.` | `Res.` |
| `Ctrl`           | `Res.`        | `MyPointer` | `Res.` | `None` | `Res.` | `Res.` | `Res.` |
| `Ctrl` + `Shift` | `Some Contrl` | ..          | ..     | ..     | ..     | ..     | ..     | 
> 
> Now to handle multiple key press use bitwise manipulation
> - `None` = `000` (Binary) = **Row 0**
> - `Shift` = `001` (Binary) = **Row 1**
> - `Ctrl` = `010` (Binary) = **Row 2**
> - `Ctrl + Shift` = `011` (Binary) = **Row 3**
> - `Alt` = `100` (Binary) = **Row 4**
> - `Alt + Shift` = `101` (Binary) = **Row 5**
> - `Alt + Ctrl` = `110` (Binary) = **Row 6**
> - `Alt + Ctrl + Shift` = `111` (Binary) = **Row 7** 

 