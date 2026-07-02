# ThreadSafe Virtual File System (VFS) 🖥️

A highly concurrent, user-space Virtual File System written in **C++17**. This project simulates core Operating System mechanics, including custom memory management, virtual memory paging, and deadlock prevention.

## Core Features ⚙️
* **Custom Memory Allocator:** Manages a pre-allocated 50MB memory pool, handing out 4KB "pages" without relying on standard OS `malloc`.
* **Virtual Memory Swapper (LRU):** When RAM is exhausted, a background thread evicts the Least Recently Used (LRU) files to a `.sys` disk file, triggering simulated **Page Faults** when they are requested again.
* **Synchronization:** Utilizes C++17 `std::shared_mutex` to implement strict Reader-Writer locks, allowing simultaneous multithreaded reading while ensuring exclusive writer access.
* **Deadlock Detection Daemon:** A background thread that continuously maps lock requests to a Wait-For Graph, utilizing Depth-First Search (DFS) to detect cyclic dependencies.

## System Proof of Work 🚀
Here is the terminal output of a 15,000 file stress test, proving the successful execution of the Swapper, Page Faults, and Reader/Writer Locks working in tandem:

```text
[SYSTEM] Initialized 50MB Custom Memory Pool.
Starting ThreadSafe-VFS...

--- Starting 15,000 File Stress Test (Will take a moment) ---
Successfully created 15,000 files. RAM was exhausted and files were paged to Disk!

--- Spawning Concurrent Readers and Writers ---
[Thread 134197917828672] Reading: ImportantData.txt
[Thread 134197909435968] Reading: ImportantData.txt
[Thread 134197875865152] Reading: ImportantData.txt
[Thread 134197892650560] Reading: ImportantData.txt
[PAGE FAULT] File 'StressFile_5.txt' not in RAM. Fetching from disk...
[Thread 134197528229440] Reading: StressFile_5.txt
[Thread 134197901043264] *** WRITING *** to: ImportantData.txt
[Thread 134197884257856] *** WRITING *** to: ImportantData.txt

System Shutting Down cleanly.
