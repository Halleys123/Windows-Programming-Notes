The processor is a relentlessly turning engine; it must always be executing instructions.
When a program needs to wait for an event—such as a user pressing a key, a file finishing a download, or a specific amount of time passing—the programmer faces an architectural choice: how does the program wait without wasting the engine's power?

There are two methods: the **Busy Wait** (the anti-pattern) and the **Blocking Function** (the correct architectural design).

### 1. The Anatomy of a Busy Wait (Polling)

Imagine a programmer writes a simple `while` loop to wait for a boolean variable to become true:

```c
// The Anti-Pattern: Busy Waiting
bool isDataReady = false;

while (isDataReady == false) {
    // Do nothing, just check again.
}
```

Mechanically, the CPU does not stop. It evaluates the condition, finds it false, loops back, and evaluates it again, executing this sequence millions of times per second. This consumes 100% of the CPU core's processing power merely to do nothing. It generates heat, drains batteries, and starves other programs of processing time.

### 2. The True Blocking Function

A true blocking function is a mechanical treaty signed between your application and the Operating System Kernel.

When a program invokes a blocking function, it explicitly tells the OS: _"I cannot proceed until X happens. Suspend my thread entirely."_ The Windows OS instantly removes your application from the CPU's execution schedule. Your thread goes to sleep. The CPU usage for your application drops to absolute zero. The Kernel will only wake your thread and return it to the CPU when the specific condition is met.

## Blocking Functions in Standard C

Pure, hardware-agnostic C does not have an inherent concept of "threads" or "sleeping" (prior to C11), because the C Standard does not assume an operating system exists.

However, standard C libraries inherently use blocking functions provided by the host OS for Input/Output operations.

- **`getchar()` and `scanf()`:** When you call `scanf()`, the C runtime library asks the OS to read from the console. The OS suspends (blocks) your program until the user presses the Enter key.
- **Network Sockets:** Functions like `recv()` to read data from the internet will block by default until a packet of data physically arrives at the network card.

## Creating a Custom Blocking Function in Win32

To create your own blocking mechanism in Windows, you cannot rely on standard C. You must use Win32 **Synchronization Primitives**—specifically, an "Event" object.

Suppose you have two threads: Thread A is calculating a massive math equation, and Thread B must wait for the answer before drawing it to the screen. You must make Thread B block.

**Step 1: Create the Event Barrier** The OS provides a handle to a synchronization object.

```c
// Create an Event object that starts in the "unsignaled" (blocking) state.
HANDLE hMathEvent = CreateEvent(NULL, TRUE, FALSE, NULL);
```

**Step 2: The Blocking Call (Thread B)** Thread B invokes the ultimate Win32 blocking function: `WaitForSingleObject`.

```c
// Thread B reaches this line and is instantly suspended by the Kernel.
// It uses 0% CPU while waiting. 
// INFINITE means it will wait forever if necessary.
WaitForSingleObject(hMathEvent, INFINITE);

// Thread B will only execute this line AFTER the event is signaled.
DrawMathResultToScreen();
```

**Step 3: The Wake Signal (Thread A)** Once Thread A finishes its calculation, it commands the OS to signal the event.

```c
// Thread A finishes its work.
CalculateMassiveEquation();

// Thread A signals the OS. The Kernel immediately wakes Thread B.
SetEvent(hMathEvent);
```

By utilizing `WaitForSingleObject`, you have created a custom blocking function. You have successfully halted execution without wasting a single cycle of CPU power, deferring the suspension and the awakening entirely to the mechanics of the Windows Kernel.

---

[[01 - Core architectural overview|GetMessage]] is one the windows function that is blocking by nature.