Insert these paragraphs after **"The mismatch that nobody names"** and before **"The decentralized model follows the hardware"**:

---

**Thread safety has poisoned language design**

Thread safety is not a property of programs. It is a property of the shared-memory model. Remove shared memory and thread safety ceases to exist as a concept. It does not need to be solved. It needs to be made irrelevant.

This distinction has been lost. A generation of language designers treated thread safety as a fundamental problem of computation and built it into the core of their languages. Mutexes, semaphores, atomic operations, memory barriers, volatile qualifiers, synchronized blocks — these are not features. They are patches on an architecture that should not have required them.

The damage runs deeper tha2 syntax. Languages now ship with concurrency primitives as first-class citizens. Programmers learn these primitives early and internalize them as part of what programming is. The question is no longer "why does this need a lock?" The question is "which kind of lock should I use here?" The accidental complexity has been promoted to essential complexity. It appears in textbooks. It appears in job interviews. It appears in the checklist of skills a senior engineer is expected to possess.

Most working programmers today have never written production software that does not grapple with thread safety in some form. To them it is simply part of the job — as natural and unavoidable as parsing or memory allocation. The idea that the entire concern could be eliminated by a different architecture is not on the table. It is not even a thought that occurs to ask.

This is how accidental complexity becomes permanent. Not by conspiracy, but by familiarity. Each generation of programmers learns the workarounds, masters them, teaches them to the next generation, and the underlying architectural mistake recedes further from view.

---

The new section sits between the diagnosis (shared memory doesn't extend to distributed systems) and the cure (the decentralized model). It explains why the mistake has been so durable — not just technically, but culturally and pedagogically.