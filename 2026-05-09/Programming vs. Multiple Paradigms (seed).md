We seem to think that that way we "program" is the _only_ way to "program". Languages like, Python, Javascript, Haskell, Rust are all basically the same (!). They all encourage programmers to use _only_ the synchronous/sequential paradigm (inspired by machine code).

I don't believe that this is a correct assumption.

I see our function-based programming model to be an artificial construct built on top of hardware devices like CPUs.

But, Prolog-based programming is a different model, also, built on top of hardware devices like CPUs.

But, Forth-based programming is a different model, also, built on top of hardware devices like CPUs.

But, State-based and Harel Statechart-based programming is a different model, also, built on top of hardware devices like CPUs.

In my view, CPUs are just "building material" for building artificial constructs - "paradigms" - on top of hardware. 

The way to program CPUs is to use machine code. You can build up scaffolding for a number of different paradigms.

In my view, "programming" is the idea of building scaffolding for various paradigms.

The idea to use _only one_ paradigm to solve a problem brings accidental complexity into the picture. It is easier to write equations in the functional paradigm, but it is, likewise, easier to write control flow in the state-based paradigm. We _can_ write control flow constructs in the functional paradigm, but the expression of control flow is clunky when expressed in the functional paradigm. When our notation makes it clunky to express certain concepts, it wastes our time and brain power and makes the concept seem to be overly-complicated. An example is our current treatment of parallelism. We think in terms of thread libraries, shared memory "thread safety" and cache coherence, all of which seem to make "parallelism" very complicated. Parallelism is much simpler - 5 year olds "get it" (piano lessons) - but trying to express parallelism in a functional manner requires more work, more work-arounds, more wasted brain power.

The advantage of the synchronous, sequential paradigm (what we call "functional programming") is that it elides the concept of "ordering" and "the arrow of time". One should note that the word "synchronous" is the antonym of the word "asynchronous". Once you express a concept in the synchronous paradigm, it becomes impossible (as in "not possible") to put asynchronicity back into the expression. Hence, we get surprises like "callback hell". If you want to express asynchronicity, you need to begin by not-using the synchronous paradigm (i.e. "functional programming").

We seem to strive to choose one and only one paradigm to solve all parts of a problem. We should, instead, be striving to express _problems_ using multiple paradigms, each suited to particular aspects of programming, e.g. a bit of synchronous expression, a bit of state-based expression, etc., instead of choosing a paradigm at the outset. For example, the Turing award concept behind "blockchain" is a combination of two concepts:
1. synchronous, mathematical constructs to express encryption.
2. state-based concepts to express the ebb-and-flow and time and order dependence of mining node participation, node drop-in and node drop-out.
The concept complicates the issues by trying to map both concepts into only one paradigm (the functional paradigm). The ground-breaking paper (re. BFT [what is the reference?]) resorts to using diagrams to show a facile state machine. The state-based behaviour would have been trivially easy to express in a state-machine, or statechart, diagram. The cryptographic portion would not have benefited from state-based expression, mathematical notation was more suitable. A simple expression of the concepts needed a mix of _both_ paradigms. Analysis requires a convenient notation - functional - but, practicality and implementability requires a _mix_ of notations. If you simply want to build something, you need to use a mix of notations. If you want to analyze something, you need something else (like, the functional paradigm). If you want to build something, don't use the same notation used to analyze that same something.