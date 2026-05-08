> If we did have hardware consisting of multiple CPUs, each with its own private memory, how would core-to-core communication be achieved? Am I correct in thinking that new circuitry would be required that doesn't exist in today's shared-memory systems?

I don't think that it's the case, but it might benefit from evolving further in that direction.

We already have multiple CPUs with private memory: L1 caches are private to each core in a multi-core CPU.

The catch, IMO, is that sharing and synchronization is done at too fine a level, needing complicated cache coherency algorithms.

We currently pass data between cores on a variable / memory-cell level, which can interrupts any processor while global memory is updated.

I advocate using a narrower waist - the only way to share values between isolated processes would be to enqueue them in a more constrained manner, i.e. atomic, asynchronous insertion into queues. The mental model might be an Arduino sending a packet of info to an ESP32 over a thin wire. We already know how to do this kind of thing ("networking") but we felt constrained by 20th century hardware to do something more convoluted, more synchronous inside programming languages, i.e. functional calling instead of message passing. 

We've been subjected to false advertising, e.g. that "C" is a "low level language". Due to that misbelief, we became enamoured with functional, stack-based calling conventions and have overlooked other very different ways of solving sub-problems. A glaring example is "forth", which doesn't implicitly use the C calling convention. Another example is Prolog (it's syntax only looks like "functions", but it's not). I'm not sure that Green Arrays forth is "the" answer, but it certainly looks different from what we think of as "programming". GA144 - https://www.youtube.com/watch?v=0PclgBd6_Zs. I feel that we are discouraged from thinking this way and other ways, because of our devotion to synchronous, sequential, functional programming.

---

While it may not be the case, it could benefit from further development in that direction.

We already have multiple CPUs with private memory: L1 caches are private to each core in a multi-core CPU.

The challenge, however, is that sharing and synchronization are performed at an overly fine level, necessitating complex cache coherency algorithms.

Currently, data is passed between cores on a variable/memory-cell level, which can interrupt any processor while global memory is being updated.

I advocate using a narrower waist. The only way to share values between isolated processes would be to enqueue them in a more constrained manner, i.e., atomic, asynchronous insertion into queues. This mental model can be likened to an Arduino sending a packet of information to an ESP32 over a thin wire. We already know how to accomplish this type of task (“networking”), but we were constrained by 20th-century hardware to implement something more convoluted and synchronous within programming languages, i.e., functional calling instead of message passing.

We have been subjected to false advertising, such as the notion that “C” is a “low-level language.” Due to this misconception, we became enamoured with functional, stack-based calling conventions and overlooked other very different ways of solving sub-problems. A glaring example is “forth,” which does not implicitly use the C calling convention. Another example is Prolog (its syntax only resembles “functions,” but it is not). I am not certain that Green Arrays’ forth is “the” answer, but it certainly differs from what we consider “programming.” GA144 - https://www.youtube.com/watch?v=0PclgBd6_Zs. I believe we are discouraged from considering this and other approaches due to our devotion to synchronous, sequential, functional programming.

This can be accomplished easily with the software we already have, without any hardware modifications. This does not require new technology, only a shift in perspective.