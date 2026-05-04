  
## TL;DR

Every executable program can be described as a formal language. The converse does not hold. Not every capability of physical hardware can be expressed in a mathematical model of computation. This is a one-way street, and the traffic has been going the wrong way for fifty years.

---

## L;R

## The Statement That Feels Obviously True

Everything executable by a computer is a formal language. This is true, and it feels complete — as if it closes the question. If every program is formal, and formal languages are the subject of computer science, then computer science covers everything a computer can do. The logic seems airtight.

It isn’t. The statement is a one-way relationship. It says nothing about the converse. The converse — that every capability of physical hardware can be expressed in a formal, mathematical language — does not follow, and is false. The word “formal” is doing double duty without anyone noticing: assembler is formal in the sense of being precisely specified, but modern programming languages are formal in the sense of being mathematical abstractions independent of physical hardware. Conflating the two meanings makes the one-way street look two-way.

## Interrupts: The Counterexample That Cannot Be Argued Away

An interrupt is a hardware event that stops the CPU at an arbitrary moment — determined by the outside world, not by the program — saves machine state, executes a handler, restores state, and resumes. The program did not call anything. There is no caller, no return value, no scheduled moment. The outside world reached into the machine between two instructions and redirected it.

The immediate objection is: you can write a callback that responds to an interrupt. Yes, you can. But the callback is not the interrupt. A callback is a function, and a function in the mathematical sense is synchronous and side-effect-free by definition. An interrupt handler is neither. It has side effects by definition — it modifies machine state — and it fires asynchronously, at a moment the program did not choose and cannot predict. You can bolt a callback onto the interrupt mechanism, but the asynchronous, uninvited, side-effecting hardware event underneath it remains outside the mathematical model.

Mathematical computation is a closed system. It maps inputs to outputs on its own schedule. An interrupt is the outside world breaking into that schedule uninvited. A closed system has no vocabulary for this. You can simulate it — polling, cooperative yielding, continuation-passing — but simulation changes what is being modeled. Polling is not an interrupt. It is the program asking, at moments of its own choosing, whether something happened. An interrupt does not ask permission.

## Where the Trade Was Made

The assembler-to-C transition is where the physical model was quietly replaced by the mathematical one. Assembler is the machine’s own notation: every instruction directly specifies a physical state change, including interrupt vectors, privileged operations, and cycle-exact timing. C replaced this with a mathematical abstraction: functions, typed values, a memory model, a calling convention. The abstraction is useful — it raised the floor enormously. But it lowered the ceiling. Capabilities that were native and direct in assembler became impossible or required operating system mediation in C.

Every language since — C++, Java, Python, Haskell, Rust — has refined the mathematical model. None has gone back to recover what was traded away at the assembler-to-C boundary. The industry has spent fifty years polishing an abstraction that is categorically mismatched with the physical hardware it runs on, and has celebrated the polish as progress.

The mismatch does not matter for batch computation: sort this list, compute this hash, prove this theorem. The machine waits. But for real-time systems, distributed networks, interrupt-driven hardware, and any system embedded in a world that keeps happening on its own schedule — the mathematical model is not an approximation of the physical reality. It is a different thing entirely, and no amount of additional mathematical machinery brings it closer. The machinery is inside the closed system. The interrupt comes from outside.

**See Also**

_Email_: [ptcomputingsimplicity@gmail.com](mailto:ptcomputingsimplicity@gmail.com)

_Substack_: [paultarvydas.substack.com](http://paultarvydas.substack.com/)

_Videos_: [https://www.youtube.com/@programmingsimplicity2980](https://www.youtube.com/@programmingsimplicity2980)

_Discord_: [https://discord.gg/65YZUh6Jpq](https://discord.gg/65YZUh6Jpq)

_Leanpub_: [WIP] [https://leanpub.com/u/paul-tarvydas](https://leanpub.com/u/paul-tarvydas)

_Twitter_: @paul_tarvydas

_Bluesky:_ @paultarvydas.bsky.social

_Mastodon:_ @paultarvydas

_(earlier)_ _Blog:_ [guitarvydas.github.io](http://guitarvydas.github.io/)

_References:_ [https://guitarvydas.github.io/2024/01/06/References.html](https://guitarvydas.github.io/2024/01/06/References.html)

_Paid subscriptions are a voluntary way to support this work._
