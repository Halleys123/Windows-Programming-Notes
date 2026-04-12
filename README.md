# Windows Programming Notes - Repository Index

**A deep, progressive knowledge base for understanding classic Windows programming—from fundamentals through architecture, message handling, and rendering mechanics.**

This is more than an API reference. These notes build mental models of _why_ Windows works the way it does, covering the design decisions, architectural patterns, and historical context that shaped modern Windows programming.

> The notes are written to be read in sequence for maximum cohesion, but each topic can also be used as a standalone reference.

---

### What's Inside

- **100+ structured notes** on Windows fundamentals, architecture, and Win32 concepts
- **Progressive learning path** that reduces context-switching for first-time readers
- **Visual explanations** using Excalidraw diagrams (message loops, coordinate systems, rendering regions)
- **Deep Unicode treatment** covering ASCII, ANSI code pages, DBCS, and wide characters
- **Message system breakdown** including queued vs. nonqueued messages and lifecycle
- **Rendering deep dive** on device contexts, invalid regions, and screen physics

## Quick Start

**New to this repository?** Start here: [[Linear Index]]

**Already exploring?** Keep this page open as a high-level map.

**Looking for a specific topic?** See [[Glossary]] or [[Tag List]]

## Repository Structure at a Glance

<table>
<tr>
<td width="50%">

### Foundation Layer

**[[Linear Index|00 - Meta & Reference]]**

- Glossary & terminology
- Chapter index & navigation
- Tags for cross-referencing

**[[Introduction and History|01 - History & Fundamentals]]**

- Windows evolution
- NT vs. 98 architecture
- Alternative APIs

**[[01 - Important Windows Headers and DLL's|02 - Architecture & Mechanics]]**

- Headers & DLL systems
- Dynamic linking details
- C standard integration
- Blocking behavior

</td>
<td width="50%">

### Core Knowledge Layer

**[[0 - History of Character Sets|04 - Unicode]]**

- ASCII origins
- Code pages & DBCS
- Wide character implementation
- `wchar_t`, `WCHAR` deep dive

**[[01 - Core architectural overview|05 - Windows, Messages & Core]]**

- Message loop mechanics
- Window classes & procedures
- Lifecycle messages
- WM_PAINT, WM_DESTROY, WM_QUIT

</td>
</tr>
<tr>
<td colspan="2">

### Applied & Extended Layer

**[[1 - General Program Structure|03 - API Reference]]** — Win32 function reference (Hungarian notation, MessageBox, LoadIcon, DrawText, etc.)

**[[01 - Introduction|06 - Physics of Screen]]** — Rendering model, device contexts, invalid region tracking

**[[Message Loop.excalidraw|Assets]]** — **Excalidraw visual diagrams** for message loops, coordinate systems, rendering regions + supplementary images

**[[Modern 4K monitors and DPI awareness in Windows|More]]** — Advanced topics (DPI-awareness, modern displays, etc.)

</td>
</tr>
</table>

## Learning Path

```
┌─────────────────────────────────────────────────────────────────┐
│  BUILD MENTAL MODELS OF WINDOWS ARCHITECTURE                    │
└─────────────────────────────────────────────────────────────────┘

  START HERE
      ↓
  [01] History & Fundamentals  ←  Context: Why does Windows work this way?
      ↓
  [02] Architecture & Mechanics  ←  Foundation: DLLs, linking, C standards
      ↓
  [04] Unicode ───────────────────  Critical: ASCII → ANSI → Unicode → wchar_t
      ↓
  [05] Windows, Messages & Core  ←  Core: Message loop, window procedures, lifecycle
   Architecture
      ↓
  [03] API Reference ────────────  Applied: Practical use of Win32 functions
      ↓
  [06] Physics of Screen  ───────  Rendering: Device context, invalid regions
      ↓
Use [[Message Loop.excalidraw|Assets]] visuals and [[Modern 4K monitors and DPI awareness in Windows|More]] extensions throughout
```

Or jump to specific topics via the [[Glossary]] and [[Tag List]].

---

## Repository Details

<details>
<summary><b>📋 Format & Conventions</b></summary>

- **Markdown files** optimized for Obsidian vault + GitHub viewing
- **Excalidraw diagrams** embedded for visual learning (message loops, coordinates, rendering regions)
- **Numbered filenames** within sections indicate reading order
- **Linked glossary** for quick terminology lookup
- **Tags and cross-references** for non-linear exploration

</details>

<details>
<summary><b>🎯 Scope</b></summary>

- **Classic Win32 API** — Foundational and intermediate level
- **Mental model building** — Why Windows works the way it does
- **Historical context** — Design decisions and evolution
- **Architecture focus** — Memory, messages, rendering, lifecycle
- **NOT covered** — .NET, UWP, modern DirectX (but foundational concepts apply)

</details>

<details>
<summary><b>👥 Best For</b></summary>

- C/C++ developers learning Windows from scratch
- Systems programmers building OS-level mental models
- Legacy codebase maintainers
- Computer science students studying operating systems
- Anyone curious about _how_ operating systems really work

</details>

---

## Quick Statistics

```
📚 Content:       100+ structured markdown notes
🏗️  Diagrams:      5+ Excalidraw visual explanations
📍 Sections:       6 major learning modules
🔗 Cross-links:    Glossary, tag index, chapter map
⏱️  Read time:     ~15-20 hours for complete path
```
