
Windows treats the keyboard from two distinct perspectives: as a collection of physical hardware keys, and as a generator of logical, displayable characters. This dictates how messages are dispatched to a window procedure.

## 1. Keystroke Messages (Physical Action)
A keystroke represents the physical action of pressing or releasing a specific key on the hardware, regardless of what that key means in context. 
* There is only one physical "A" key. Pressing it generates a keystroke message.
* **Non-Character Keys:** Keys like Shift, Ctrl, Function keys (F1-F12), Arrow keys, Insert, and Delete *only* generate keystroke messages. They do not produce displayable characters.

## 2. Character Messages (Logical Translation)
A character is the logical result of a keystroke, modified by the current state of the keyboard (Shift, Caps Lock, Ctrl, or preceding dead keys).
* The single physical "A" key can translate into 'a', 'A', 'á', or a control character (Ctrl-A).
* Windows handles this translation automatically. 

## The Message Dispatch Split
Because of this two-tier system, a single physical action can result in different message streams:
* **For displayable characters:** Windows sends *both* a keystroke message (the key went down) and a character message (the translated symbol).
* **For non-displayable keys:** Windows sends *only* the keystroke message.

## Architecture Application
When building a custom UI framework and native Win32 Markdown editor, this separation dictates the event loop logic. Raw keystroke messages (`WM_KEYDOWN`) must be intercepted to handle cursor navigation, text selection, and editor shortcuts. Meanwhile, character messages (`WM_CHAR`) can be streamed directly into the text rendering buffer. Relying on the OS for this translation avoids the overhead of manual scan-code parsing, helping maintain a highly optimized memory footprint well under the 50MB RAM target.