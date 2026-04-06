# Windows Programming Tag List

This is the master tag taxonomy for this vault.

## Tag Format

Use tags in this shape:

- prefix/category/value

Examples:

- topic/api/win32
- subject/messagebox
- type/reference

## Why This System Exists

- Better graph clustering by theme (history, API, architecture, internals)
- Better search filtering by note purpose (concept, reference, function)
- Better long-term scalability as Windows Internals notes are added

## Prefixes and Their Purpose

- topic: What major domain the note belongs to
- subject: What exact thing the note talks about
- layer: Where the concept lives in the OS stack
- type: What kind of note this is
- source: Where information came from
- status: Maturity stage of the note
- track: Reading path grouping across books and topics

## Master Tag List With Uses

### Topic Tags

- topic/history/windows: Historical evolution of Windows OS, versions, and design shifts
- topic/history/text-encoding: Historical evolution of character encoding systems
- topic/api/win32: General Win32 API programming and concepts
- topic/api/user32: UI and window manager APIs from USER32
- topic/api/kernel32: Core process, memory, and file APIs from KERNEL32
- topic/api/gdi32: Graphics and drawing APIs from GDI32
- topic/architecture/linking: Dynamic and static linking, DLL loading, import/export behavior
- topic/architecture/memory: Memory layout, stack, heap, allocation, and storage models
- topic/architecture/process-thread: Process model, threading, scheduling, and execution flow
- topic/architecture/gui: Windowing model, message loops, and interaction model
- topic/text/encoding: Character encoding logic, conversion rules, and byte representation
- topic/text/unicode: Unicode architecture, code points, and UTF families
- topic/text/legacy-codepages: ANSI, code pages, and legacy compatibility concerns
- topic/internals/kernel: Windows kernel architecture and execution model
- topic/internals/object-manager: Object namespace, handles, and object lifetime
- topic/internals/process-manager: Process and thread internals, creation, and teardown
- topic/internals/memory-manager: Virtual memory, paging, working sets, and mappings
- topic/internals/io-manager: I/O request flow, IRPs, and device interaction path
- topic/internals/security: Tokens, ACLs, privileges, and security boundaries
- topic/internals/registry: Registry architecture, hives, and configuration mechanics
- topic/internals/syscalls: Transition path from user mode to kernel services
- topic/internals/drivers: Driver model, loading, and driver-kernel interactions

### Subject Tags

- subject/ascii: Notes primarily about ASCII design and behavior
- subject/ansi: Notes focused on ANSI usage in Windows context
- subject/code-pages: Notes about code page mapping and compatibility
- subject/dbcs: Notes on double-byte character sets and behavior
- subject/unicode: Notes about Unicode concepts and implementation
- subject/utf-8: Notes specifically about UTF-8 format or usage
- subject/utf-16: Notes specifically about UTF-16 format or usage
- subject/wchar-t: Notes about wchar_t and wide character storage
- subject/tchar: Notes about TCHAR macros and dual ANSI/Unicode patterns
- subject/winmain: Notes about WinMain or wWinMain entry points
- subject/messagebox: Notes about MessageBox API and related behavior
- subject/winapi: Notes about WINAPI, calling conventions, and macro usage
- subject/callback: Notes about callback functions and callback conventions
- subject/hungarian-notation: Notes about naming convention patterns in Win32 style
- subject/dll: Notes about dynamic link libraries in general
- subject/kernel32-dll: Notes tied to KERNEL32.DLL specifics
- subject/user32-dll: Notes tied to USER32.DLL specifics
- subject/gdi32-dll: Notes tied to GDI32.DLL specifics
- subject/windef-h: Notes tied to WINDEF.H definitions
- subject/winnt-h: Notes tied to WINNT.H definitions
- subject/winbase-h: Notes tied to WINBASE.H definitions
- subject/winuser-h: Notes tied to WINUSER.H definitions
- subject/wingdi-h: Notes tied to WINGDI.H definitions

### Layer Tags

- layer/foundations: Foundational concepts needed before advanced topics
- layer/user-mode: Concepts that run in user mode
- layer/win32-api: Concepts exposed through Win32 API layer
- layer/nt-native-api: Concepts from NT native API level
- layer/kernel-mode: Concepts that execute inside kernel mode
- layer/hardware-interface: Concepts close to CPU, memory hardware, and architecture boundaries

### Type Tags

- type/concept: Explanatory theory-focused note
- type/reference: Quick lookup note for definitions and signatures
- type/function: API function-level breakdown
- type/header: Header-file-level breakdown
- type/mechanism: Step-by-step internal process explanation
- type/timeline: Chronological sequence note
- type/glossary: Terminology definitions note
- type/index: Navigation or chapter map note
- type/comparison: Compare-and-contrast note
- type/future-read: Placeholder topic queued for deeper study
- type/todo: Pending items, unresolved questions, and action list notes

### Source Tags

- source/book/petzold: Content learned from Charles Petzold material
- source/book/windows-internals: Content learned from Windows Internals books
- source/ms-docs: Content derived from Microsoft official documentation
- source/article: Content derived from technical articles or blogs
- source/video: Content derived from lectures or video material
- source/personal-notes: Original synthesis or reasoning written directly in this vault

### Status Tags

- status/inbox: Captured but not yet organized or validated
- status/draft: Initial write-up complete but still rough
- status/learning: Actively being studied and expanded
- status/reviewed: Reviewed for correctness at least once
- status/evergreen: Stable, high-confidence note expected to stay valid
- status/future-read: Deferred until later reading pass
- status/revise: Marked for rewrite or correction

### Track Tags

- track/petzold: Part of the Petzold learning sequence
- track/windows-internals: Part of the Windows Internals learning sequence
- track/cross-reference: Bridge note connecting multiple tracks or domains

## Tagging Rules

- Do not include # inside YAML tags list values
- Use lowercase letters and hyphenated words
- Keep each note between 4 and 7 tags
- Use exactly one topic tag per note
- Use exactly one type tag per note
- Use exactly one status tag per note
- Add subject tags for precision search
- Add source tags only when origin is known
- Add one track tag only when the note belongs to a reading track

## Suggested Minimum Template For New Notes

- topic/...
- subject/...
- type/...
- status/...
- source/...
