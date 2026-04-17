MicroPython's threading support depends on the hardware target:

**`_thread` module (low-level threads)** MicroPython includes a `_thread` module on many ports (RP2040, ESP32, Unix). It provides basic thread creation and mutex primitives. However, it's a minimal implementation — no `threading.Thread` class, no `Event`, no `Semaphore` like CPython's full `threading` module.

**GIL situation** Like CPython, MicroPython has a Global Interpreter Lock on most ports, so threads don't run truly in parallel on a single core — they take turns. On the **RP2040 (Pi Pico W)**, the two cores are exposed via `_thread`, giving you genuine dual-core parallelism, which is one of the more useful threading scenarios in MicroPython.

**`asyncio` (preferred approach)** For most MicroPython use cases, `uasyncio` (now just `asyncio` in recent versions) is the idiomatic concurrency tool. It's cooperative multitasking — a single thread, event-loop driven — and works across virtually all ports. This maps well to your 0D/async-by-default thinking: components yield control rather than blocking.

**Port-by-port summary:**

|Port|`_thread`|True parallelism|
|---|---|---|
|RP2040|✅|✅ (2 cores)|
|ESP32|✅|✅ (2 cores, FreeRTOS)|
|ESP8266|❌|❌|
|STM32|limited|❌|
|Unix|✅|GIL-limited|

**Bottom line:** MicroPython has _threading_, but it's intentionally stripped down. For async I/O concurrency, `asyncio` is the right tool. For true CPU parallelism, the RP2040 or ESP32 dual-core ports via `_thread` are your best bet.

---

**GIL = Global Interpreter Lock**

It's a mutex (mutual exclusion lock) inside the interpreter itself that ensures only **one thread executes Python bytecode at a time**, even on multi-core hardware.

**Why does it exist?** The interpreter's internal data structures (object reference counts, memory allocator, etc.) are not thread-safe. Rather than fine-grained locking on every object, CPython (and MicroPython) use a single coarse lock — simpler to implement and faster for the single-threaded common case.

**Practical effect:**

- Two threads can _exist_ simultaneously
- But only one runs Python code at any moment
- The GIL switches between threads periodically (or when a thread does I/O)
- So for **CPU-bound** work, threads don't speed things up — you get concurrency without parallelism
- For **I/O-bound** work (waiting on network, disk, serial), threads _do_ help — the GIL is released while waiting

**The RP2040 exception:** The RP2040 port of MicroPython runs each core as a separate interpreter instance with its own GIL, so the two cores genuinely run in parallel — they just can't share Python objects safely without explicit locking.

**Analogy:** One kitchen, two cooks, one knife. They take turns with the knife even if they could theoretically chop simultaneously.

---

*is the RP2040 a practical solution to long-distance asynchronous parallelism?*

Short answer: **No — it's the wrong tool for that concept.**

The RP2040's dual-core parallelism is **tight-coupled, shared-memory** parallelism. The two cores share the same RAM, the same address space, and communicate via shared variables + spinlocks. That's the _opposite_ of long-distance asynchronous parallelism.

**What the RP2040 gives you:**

- 2 cores, shared memory
- Fast, low-latency inter-core FIFO (32-bit messages, hardware-assisted)
- Good for: one core handles sensor polling, other handles computation — tightly coordinated

**What "long-distance async parallelism" actually needs:**

- No shared state
- Message passing over a channel (your 0D model)
- Components that don't know or care about each other's internals
- Backpressure, queuing, independent lifecycles

**The RP2040's inter-core FIFO** is interesting — it's a hardware 4-deep FIFO between cores, which _is_ message-passing flavored. But it's 32-bit words, not structured messages, and the two cores still share all memory, so the discipline of not sharing state is entirely on the programmer.

**Better fits for your model:**

- **Multiple microcontrollers communicating over UART/I2C/SPI/CAN** — physically enforces no shared memory
- **ESP32 with FreeRTOS queues** — software message queues between tasks
- **UNIX processes with pipes** — your existing model, essentially
- **Erlang/BEAM** — the canonical "long-distance async" hardware, processes with no shared heap

**The core issue:** The RP2040 tempts you into shared-memory thinking because sharing is _easy_ and message-passing requires discipline. For your PBP/0D model, physical separation (separate MCUs, separate processes) is more honest — the architecture enforces the invariant rather than relying on programmer restraint.