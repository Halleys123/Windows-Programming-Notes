
This is a relic of the past when the microprocessors used 16 bit bus lane in era or MS-DOS and Windows 3.1 and are preserved just for historical purposes. In modern times there is no need to be bothered by the **Near** and **Far** pointers unless you are making a program that should run on these **historical relic systems**.

In those microprocessors like 8086 that were by design 16 bits that means all the registers, bus lane, memory can at most be 16 bit wide. It means it can only count up-to $65,535$ and ram can only be max $64KB$ but the IBM motherboard was able to support **20 bit address bus** that means it could virtually support $1 MB$ of RAM. 

So to bridge this gap intel engineers came up with idea of `Segmented Memory`.

## Segmented Memory

Instead of treating memory as one massive, continuous line, the processor divided the 1 Megabyte of RAM into overlapping 64 KB chunks, known as "Segments." To locate a specific byte in memory, the CPU required two separate 16-bit numbers:

## Near Pointers vs. Long (Far) Pointers

Because calculating a two-part address required additional clock cycles, programmers were given a choice in how they managed memory. This choice birthed the `N` and `L` prefixes.

- **Near Pointers (`NPSTR`):** A "near" pointer was only 16 bits wide. It contained only the Offset. It operated on the assumption that the Segment Address was already set and would not change. Because it was only half the size, it was faster and consumed less memory. However, it was trapped. A near pointer could only move within its current 64 KB segment. If a string was larger than 64 KB, or located in a different segment, a near pointer could not reach it.
- **Long or Far Pointers (`LPSTR`):** A "long" (or "far") pointer was 32 bits wide. It contained both the 16-bit Segment and the 16-bit Offset. Because it carried the complete map, a long pointer could reach any byte within the entire 1 Megabyte of RAM. It was mathematically liberated, but it was slower to calculate and consumed twice as much memory.
- 
Suddenly, a single pointer could address 4 Gigabytes of RAM. The complicated Segmented Memory system was rendered obsolete overnight. Operating systems shifted to a **Flat Memory Model**, where memory was treated as one continuous, unbroken line. Every pointer became exactly 32 bits wide.

Therefore, when Microsoft designed the Win32 API, the distinction between a "near" pointer and a "long" pointer vanished. The hardware no longer required segments. Today, an `LPSTR` (Long Pointer to a String) and an `NPSTR` (Near Pointer to a String) are mathematically identical. They are both simply 32-bit addresses. The prefixes remain in `WINNT.H` purely as a historical treaty, ensuring that a program written in 1991 will still compile seamlessly in the 21st century.

---

Final review is that Long and Far pointer are totally Obsolete and we can totally ignore them .