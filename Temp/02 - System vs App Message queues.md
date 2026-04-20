# Keyboard Input: Queues and Synchronization

In Win32, keyboard input is not sent immediately to an application's message queue. The OS uses a deliberate, two-step routing process to manage hardware interrupts and ensure window synchronization.

## The Two-Step Routing Process

1. **Hardware to System Queue:** As keys are pressed and released, the keyboard device driver translates hardware scan codes into formatted messages. Windows intercepts these and places them into the **System Message Queue**. 
   * *Note:* This is a single, OS-level queue dedicated to the preliminary storage of all mouse and keyboard input.

2. **System Queue to Application Queue:**
   Windows pulls the next message from the System Message Queue and dispatches it to the specific **Application Message Queue**. 
   * *Critical Condition:* Windows only moves a new message into the application's queue *after* the application has finished processing the previous user input message.

## The Architectural "Why": Synchronization and Focus

The primary reason for this two-tier architecture is **synchronization of the input focus**. 

Because users frequently type faster than an application can process the keystrokes (e.g., executing a complex UI redraw or file operation), a bottleneck occurs. 

If Windows bypassed the system queue and dumped all keystrokes directly into the application queues based on the window currently in focus, it would create a race condition:
* A user types a rapid sequence of keys.
* The first keystroke is a command (like `Alt+Tab` or an accelerator) that switches the input focus to a *different* window.
* If the subsequent keystrokes had already been packaged and dropped into the first application's queue, they would be processed by the wrong window.

By holding messages in the central System Message Queue, Windows evaluates the destination of each keystroke sequentially, ensuring that rapid focus changes are respected and keys are delivered to the correct `HWND`.