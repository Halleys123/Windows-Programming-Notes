The virtual key code is stored in the [[5 - WPARAM and LPARAM#^7061e3|wParam]] parameter of the `WM_KEYDOWN`, `WM_KEYUP`, `WM_SYSKEYDOWN`, and `WM_SYSKEYUP` messages. This code identifies the key being
pressed or released.

## Scan Codes vs. Virtual Keys

To understand why Win32 uses "Virtual" key codes, you have to look at how MS-DOS software interacted with hardware in the early 1980s, before Windows forced applications to talk to the OS instead of the metal.

### The DOS Era

The Raw Scan Code In the DOS days, there was no operating system managing input in the background. If you wrote a word processor or a game, your application had to hook directly into the CPU's hardware interrupt line (Interrupt 0x09).

When a user pressed a key, the keyboard's internal microcontroller sent a **Scan Code**. 
* A Scan Code is **not** a letter or a character. It is literally a geographical coordinate on the physical keyboard matrix. 
* On the original 1981 IBM PC keyboard, Scan Code `16` meant "the first key on the second row." On a US keyboard, that happens to have the letter 'Q' painted on it.

### The Architectural Nightmare 

Treating Scan Codes as characters created a massive device-dependency problem: 
1. **Localization:** A French AZERTY keyboard puts the 'A' key where the 'Q' key is. If a programmer hardcoded `if (scancode == 16) { type('Q'); }`, a French user pressing 'A' would get a 'Q' on their screen. The software was physically bound to a US plastic layout. 
2. **Hardware Evolution:** When IBM introduced the 101-key Extended Keyboard, they moved keys around and added new ones. Scan codes changed. DOS programs broke because they expected the old geographical layout.

## The Windows Paradigm Shift

Virtualization When Windows was designed, its primary goal was **Device Independence**. Microsoft told developers: *"You are no longer allowed to touch the hardware interrupts. You talk to Windows, and Windows will talk to the keyboard."*

To solve the Scan Code nightmare, Windows introduced the **Virtual Key Code (VK_*)**.

### What makes it "Virtual"? 

It is "virtual" because it represents the *semantic meaning* or *logical intent* of the key, completely divorced from its physical location on the circuitry.

Here is how the modern Windows input stack handles it: 
1. **Hardware Level:** You press the top-left letter key. The hardware sends Scan Code `16`. 
2. **Driver Level:** The Windows keyboard device driver intercepts `16`. 
3. **OS Translation:** The OS looks at the user's localized language settings. * If the user is in the US, it translates `16` to `VK_Q`. * If the user is in France, it translates `16` to `VK_A`. 
4. **Application Level:** Your Win32 message loop receives a `WM_KEYDOWN` with `wParam = VK_Q`.

---

## Extra information: The USB HID Protocol

Once the microcontroller in keyboard has determined exactly which keys are pressed, it needs a standardized way to talk to Windows. This is where the **HID (Human Interface Device)** protocol comes in.

Before USB, keyboards plugged into a PS/2 port. PS/2 was an **interrupt-driven** architecture. When a key was pressed, the keyboard sent a literal voltage spike down the line that halted the CPU to announce the keystroke.

USB completely changed this to a **polling-driven** architecture. The CPU is no longer interrupted by the keyboard. Instead, the PC's USB Host Controller acts as the master and constantly interrogates the keyboard: _"Do you have new data? Do you have new data?"_ The frequency of this interrogation is your polling rate (typically 1000Hz, or once every millisecond, for high-performance peripherals).

When the keyboard connects, it sends the OS a **HID Descriptor**. This is basically a data contract. It tells Windows: _"I am a keyboard. When you poll me, I will reply with an 8-byte array formatted exactly like this."_

A standard USB keyboard HID Report (the 8-byte array) looks like this:

- **Byte 0:** Modifier keys bitmask (1 bit for Left Ctrl, 1 bit for Left Shift, 1 bit for Alt, etc.).
- **Byte 1:** Reserved (usually 0x00).
- **Bytes 2 through 7:** An array of up to 6 standard keys that are currently held down. (These are USB Usage ID codes, which Windows then translates into the Virtual Key Codes we discussed earlier).