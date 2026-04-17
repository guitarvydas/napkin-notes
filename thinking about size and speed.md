Random, unfinished thoughts follow. Please feel free to chime in, kibitz, whatever.

- What is lean code?
	- lean footprint on a CPU-based machine?
	- lean notation for expressing problems in certain domains?
	- hardware boost for certain problem domains? Not necessarily CPU-based?

- IoT 
	- uses Linux, hence not lean footprint, 
	- OTOH, if you use a billion transistors then you might still have lean code, but not-so-lean hardware (am I advocating pushing functionality down into hardware?)

- Sector Lisp 
	- intentional minimalism = assembler tricks
	- unintentional minimalism = sticking to one and only one paradigm 
		- pure functional
		- SL doesn’t even have assignment (assignment is anti-functional)
	- To me Sector Lisp’s size was a surprise. I went in thinking that it was all due to assembler tricks. Clearly intentional minimalism. Eventually, though, I concluded that its small size was overwhelmingly due to the way it treated the paradigm. It uses the functional paradigm and treats it in a pure manner - no assignment, no heap, no attempt to deal with the full gamut of the underlying hardware and opcodes. I interpreted this as unintentional minimalism, although I’m not sure that this is a correct interpretation. Now, I’m wondering if what I call bloat is caused by the intentional unioning of multi-paradigmatic features into GPLs (which makes me question if GPLs are the right path)…

- micro python for horse monitoring
	- example of small code in a modern application
	- 13 microcontrollers attached to each race horse in the stable, to monitor health 24/7
	- programmed in micro-python
	- networked together, send info to internet server(s)

- HPC
	- what is HPC (High Performance Computing)?
	- I think that HPC is: compute an answer to hoarier and hoarier equations quickly, ignoring time and sequencing issues
	- maybe HPC doesn’t apply (or, no longer applies to) other kinds of uses of RMs? 
		- is that my point - interesting uses of “computers” are not always best expressed as equations, e.g. control flow (better with state machines, statecharts)? I seem to think that there are other uses and they are orthogonal to HPC
	- RM == Reprogrammable Machine

- if it’s synchronous, then, by definition, it’s not asynchronous
	- regardless of how many “concurrency” band-aids are forced into the notation. 
	- I claim that asynch is an interesting domain that can’t be covered by current sync/seq techniques and equations
- am I fighting GPL mentality? build lean notations for each paradigm(s) in a project, use anything that is available as building material for executing the notations

- I observe a general assumption: GPLs are the answer
	- i.e. we should use the same GPL for HPC 
	- i.e. we should use the same GPL for programming devices in fighter jets
	- i.e. we should use the same GPL for programming robots
	- i.e. we should use the same GPL for programming games
	- etc.

- I felt that there was a huge, discernible difference in footprint in staged compiler development vs. denotational semantics approach 
	- 1Mb : 300Mb
- what is lean for your problem, might not be lean for my problem
- LLMs must be coupled to the Library of Alexandria (today == internet) to be as useful as they are
- type checking 
	- level 0 == hardware supported types (int8, int64, float, etc.)
	- level 1 == user defined types (actually problem-specific types) that don’t necessarily match the hardware (Muratori’s The Big OOPs)
- my question: what happened? I think of Turbo Pascal as being “lean” - it is a convenient and concise way to program CPUs, why do we need more than that? 
	- theory: going beyond TP actually means deep-diving into a single problem domain, but, we’ve cluttered the way forward by adding complicated baubles to the functional domain
- going from subroutines to functions (blame: C) tipped the scales towards timeless “compute-ation”
	- Chuck Moore didn’t go that route, yet Forth still produces usable, interesting things especially in control domains, and creates what I think is small and useful (e.g. Forth Haiku - Forth is the notation, JS/GLSL is the building material)
- differences
	- (a) GPL is the answer to all RM problems, 
	- (b) each different RM problem needs its own notation (to be thought of as “lean”)
- What is a “computer”? What is it good for? What is “programming”? 
	- I think there are many answers, yet, we’re deep diving into only _one_ domain of use and constantly adding baubles to apparently-support other domains (this is causing “bloat” in my view). 
	- We “shouldn’t” be using the current crop of GPLs to program (all aspects of) games (is that Muratori’s implicit point in The Big OOPs?)
- dissecting Turbo Pascal is an attempt to glean implicit techniques that may lead down other paths of design. 
	- I feel that these techniques were ignored in the rush towards building RMs to perform compute-ing.

- I don’t think that the current compute-ing techniques are leading to useful notations for other problem domains, regardless of the number of complications added to the current techniques
	- Ceptre and Nova are interesting examples of building new notations that feign to be compute-ation
	- IMM they fall outside of current functional notation

---

- if you include all of the code required to run a program, you have to include the operating system
	- Perplexity says that only the *kernel* for modern O/Ss are
		- Linux 40M LOC
		- Windows (proprietary) est. 50M-80M LOC
		- MacOS (Darwin base) 80M-86M LOC
	- IMM, any programming language that is based on these O/Ss is not just bloated, but ultra-bloated
	- What PLs do not require these OSs?
		- micro-python
		- Forth
		- Sector Lisp
		- PBP (0D)
	- I consider that any SBC (Single Board Computer) that includes Linux is pre-bloated
- we model "concurrency" on the older concepts of "time-sharing"
	- it's easy to roll-your-own mutual multi-tasking (PBP free repo, 1/2 page of code in Holt's book "Concurrent Euclid, The UNIX system, and TUNIS")
	- time-sharing was developed to allow multiple programs to run on *one* CPU
	- time-sharing involves consideration for memory-sharing
		- commonly known as "thread safety"
			- deep rat hole of little "gotchas" that all evaporate if you disallow memory sharing (accidental complexity, accidental bloat)
		- impractical assumption for modern problem domains, e.g. distributed processing across long distances (internet, robots, IoT)
		- IMO, pushing further down this path, adding baubles like promises and even more "type checking", is a dead end
		- current PLs only *fake out* concurrency, synchronous concurrency at that 
		- we *can* use current PLs to *fake out* asynchronous concurrency - easily (done in PBP)
		- current PLs rely on hardware tricks to achieve async concurrency
			- code that resides only in L1 cache is async
			- as soon as that code needs to touch >L2 caches, the hardware tricks fall back onto synchronous concurrency of the kind that relies on physical closeness and doesn't lend itself to long-distance distribution (due to inherent memory sharing)
		- current PL assumption of memory-shared threading is usable on Linux-based SBCs, but essentially ignores the issue of micro-controllers that do not include Linux
		- multi-core is a dead end
			- implements time-sharing model but relies on physical closeness
			- this model cannot extend, easily, to distributed problems
		- does micro-python allow multi-threading?
			- **Bottom line:** MicroPython has _threading_, but it's intentionally stripped down. For async I/O concurrency, `asyncio` is the right tool. For true CPU parallelism, the RP2040 or ESP32 dual-core ports via `_thread` are your best bet.
			- *is the RP2040 a practical solution to long-distance asynchronous parallelism?*
				- Short answer: **No — it's the wrong tool for that concept.**
				- **The core issue:** The RP2040 tempts you into shared-memory thinking because sharing is _easy_ and message-passing requires discipline. For your PBP/0D model, physical separation (separate MCUs, separate processes) is more honest — the architecture enforces the invariant rather than relying on programmer restraint.
		-  Current threading is a structural dead end for distributed reliability. BEAM fixes the state problem. PBP fixes both the state problem _and_ the topology problem — but the production proof is still ahead of it.
