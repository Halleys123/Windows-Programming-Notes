---
tags:
  - "#history/windows/os_kernels"
---

Both Windows 98 and Windows NT are 32-bit pre-emptive multitasking and multithreading graphical operating systems. GUI also called a "visual interface" or "graphical windowing environment"

Windows use Rectangular grids known as windows to show the information to the user and user is allowed to resize, modify close, move the window on screen thus allowing multiple windows to be open concurrently, and supports multi tasking.

**Earlier versions** of Windows used a system of multitasking called “**non-preemptive**.” that means it was programs responsibility to return the resources to the Windows and if program was malicious or having some bug there was no way of getting the resources other than restarting the PC. **Windows NT and 98** supported **pre-emptive** mode of multitasking. ^911722

## Concurrency and Parallelism in Windows

In those times there was only a single core to run all the application and so pre-emptive multi-threading means that you will be able to do following things.

1. Run multiple programs, application concurrently on single CPU
2. Run same application in multiple parts so that even if one part of the application get stuck you are still not getting stuck in program as a whole. For example One thread scrolls the webpage smoothly (UI), while another thread downloads a video in the background (Network). If the download thread gets stuck, you can still scroll the page because both are separate threads and are getting time in the CPU without interference with each other.
3. Similar concept is used in parallelism you still create multiple threads for same program but instead of running those threads in same Core using concurrency using context switch you run all those in multiple cores with help of parallelism.

So the base is create multiple threads of same programs, then either run those threads in multiple Core (Parallelism) or in same core (concurrency) using context switching. Multi-threading is just a way of writing programs. Then to achieve control over how parallelism occurs in the CPU we need to assign **thread affinity mask** to the thread created which tells OS to fix the thread to some core but even if thread affinity mask is not set by user then OS automatically decide and switch the thread to some core which is not busy at that time. ^4ef346

## GUI

Concept of GUI starts from 1970 based on work done at Xerox PARC for ma chines such as the Alto and the Star and for environments such as SmallTalk. This was later brought into and popularized by Apple and Microsoft.

GUI provide better input and output handling as compared to Character mode OS. Have some features like [[Glossary#^787461|WYSIWYG]]
