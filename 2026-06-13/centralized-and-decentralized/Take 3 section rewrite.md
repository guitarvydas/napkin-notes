Here is a replacement for the "The development environment needs to change too" section:

---

**The development environment needs to change too**

One honest complication: software development happens on single machines. A developer writing a decentralized application is still sitting in front of one CPU with shared memory.

Right now, we handle this backwards. We build systems that fake asynchronous processing as if it were synchronous — we wrap message-passing in synchronous APIs, ==we pretend the network call blocks cleanly, we hide the asynchrony and then spend enormous effort managing the consequences when the illusion breaks.== Then we ship the entire development environment to users. Users must buy computers running operating systems that support faked multi-processing. The bloat, the context-switching overhead, the complicated scheduling algorithms — all of it lands on the user's machine because we never separated the development model from the deployment target.

We need to invert this. Development systems should fake asynchronous message-passing on a single machine, not fake synchrony on top of an asynchronous reality. Each component should be treated as if it owns a private CPU and private memory. Components communicate only by passing messages. No shared memory. No thread safety. The fact that the underlying machine is actually running these components by time-sharing a single CPU is an implementation detail of the development environment — not an architectural assumption that bleeds into the design.

One might expect that faking message-passing on a single development machine would make apps noticeably slower. This is not obviously true. Avoiding shared memory by default does impose a cost on apps that genuinely benefit from bulk data transfers — but it does not tax the vast majority of apps that never needed that optimization in the first place. The current version of PBP builds this fake asynchronous processing from scratch, in a green-thread manner. This implies — though it has not yet been fully tested — that PBP does not actually require an operating system as we know it. If that holds, the target hardware does not need to be burdened with a bloated OS, nor with the costs of time-sharing, context-switching, and the overhead of complicated scheduling algorithms. We should be able to ship simpler, leaner systems to users. Not copies of our development environments.

Our current workflow makes this worse in a specific way. We write single synchronous nodes first, then retrofit reactivity onto them so they can be plumbed together — callbacks, event emitters, promise chains, async/await. This is programming asynchrony in assembler. We are manually encoding, at the application level, coordination that should be handled by the architecture. The result is code that is hard to read, hard to reason about, and hard to compose.

That said, the existing body of synchronous software is not worthless. Code written for a single synchronous node can be wrapped and treated as a leaf component in a larger asynchronous scheme. The wrapper handles the message boundary. The interior of the component stays exactly as it was. This gives us a migration path. We do not need to discard what works. We need to stop using it as the architectural model for the whole system.

---

Insert this in place of the existing "The development environment needs to change too" section. The PBP reference is the only place in the article where you appear by implication — flag it if you'd rather keep that detail out of this particular piece.