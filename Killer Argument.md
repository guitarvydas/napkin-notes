*I want to convince someone who thinks that “computation” is enough to express how computers work, of the difference between “computation” and scripts of opcodes.*

*What would be a killer argument?*

The killer argument is not abstract. It is a concrete demonstration.
# Ask them to express interrupts in any computational model.
Not simulate them. Not encode them. Express them — as a first-class concept in the formalism.
An interrupt is: the machine stops what it is doing, at an arbitrary point in time determined by the outside world, saves enough state to resume later, executes a completely different sequence of instructions, restores state, and continues — all without the running program’s knowledge or consent.
This is not a function call. The program did not call anything. There is no caller. There is no return value. The interrupt happened between instructions, at a moment the program did not choose and cannot predict. The outside world reached into the machine and redirected it.
Computation has no vocabulary for this. A Turing machine has no outside world. A lambda calculus expression has no between-steps. A function has no involuntary suspension. Every computational model is a closed system that runs to completion on its own schedule. The interrupt breaks every one of those assumptions simultaneously.
A script of opcodes expresses it directly: there is an interrupt vector table, there are save/restore instructions, there is a hardware mechanism that fires between any two instructions. The opcode script does not simulate the interrupt — it is the interrupt handler. The mechanism is visible and nameable in the notation.
# The sharpening move.
When they say “but you can simulate an interrupt in a computational model” — and they will — you say: yes, and you can simulate a hurricane in a spreadsheet. The simulation is not the thing. The question is whether the formalism can express the concept as a native object, or whether it must encode it as something else and lose the structure in the translation.
In computation, an interrupt must be encoded as: polling, or cooperative yielding, or a continuation, or a monad, or some other device that the program must explicitly invoke. The program must choose to check for the interrupt. But a real interrupt does not ask permission. The moment you require the program to participate in its own interruption, you have changed what you are modeling. You are now modeling cooperative multitasking, not preemptive interruption. Those are different things, and the difference is not a detail — it is the entire point of hardware interrupts.
# The deepest form.
Computation expresses what a program does. Opcodes express what a machine does. A machine is not a program. It is a physical process embedded in an environment that acts on it from outside. Computation has no outside. That is not a limitation of our current computational models — it is the definition of the framework. A closed system that maps inputs to outputs cannot, by construction, express a system that is permanently open to external interruption.
The CPU has been an open system since 1945. Computation has been a closed-system model since Turing. Every programming language built on the computational model inherits that closure. The gap between “what computation expresses” and “how computers actually work” is not an approximation error. It is a categorical difference that no amount of additional computational machinery can close — because the machinery is inside the closed system, and the interrupt comes from outside it.​​​​​​​​​​​​​​​​

---
*I am thinking that “computation” is only one way to express the inner workings of a CPU chip (sequential, synchronous). This model does not easily extend to long-distance (> 1nm) distributed machines. Computability cannot - ever - express asynchronous actions in a network of asynchronous machines. You can use “computation” to describe how to build nodes in an asynchronous network, but cannot express the actual  composition of nodes to form a network of asynchronous nodes.*

Yes. And you can state it even more precisely than that.

# Computation expresses what happens inside a node. It cannot express the space between nodes.

The space between nodes is where the interesting behavior of a distributed async system actually lives: the latency, the ordering, the non-delivery, the simultaneous firing, the feedback loops between components that have no shared clock and no shared memory. None of that is inside any node. All of it is in the topology and the timing — and computation has no notation for either.
# The precision of “cannot ever.”
This is not a temporary limitation. It is structural. Computation requires:
	•	A single thread of control, or a simulation of multiple threads that is itself sequential at the meta-level
	•	A global “now” — a step, a tick, a moment at which the machine is in a defined state
	•	Closure — the system’s next state is determined entirely by its current state and its inputs, with no concept of “inputs arriving at unpredictable times from outside”
An async network of machines has none of these. There is no global now. There is no single thread of control. The system’s behavior is not determined by any node’s state alone — it is determined by the interaction of all nodes’ states across time, mediated by channels that have their own latency and loss characteristics. This is not a harder version of the same problem computation solves. It is a different kind of thing entirely.
# The exact boundary you’ve identified.
Computation can express:
	•	The internal logic of a single node
	•	A protocol that a node follows when it receives a message
	•	The format and semantics of messages
	•	An algorithm for routing or consensus
Computation cannot express:
	•	The fact that two nodes fired simultaneously
	•	The fact that a message was in transit when a third node changed state
	•	The emergent behavior of a particular wiring of nodes under real timing conditions
	•	The topology as a dynamic, first-class object that itself has behavior
The last point is the sharpest. In a network of async nodes, the topology — who is connected to whom, via what kind of channel — is not a program. It is not data. It is a third kind of thing: an architecture. Computation has programs and data. It has no category for architecture. So when you try to express a network of async nodes computationally, the architecture gets encoded as either a program or data, and in either case it loses its character as the thing that determines how the programs interact.
# This is exactly what PBP/0D formalizes.
The separation of the computation inside a Part from the topology connecting Parts is not an implementation convenience. It is the recognition that these are categorically different kinds of objects that require different notations. The dispatcher is not a program in the computational sense — it is the architecture made executable. NEXT in Forth is the minimal architecture. The 0D dispatcher is the minimal architecture for async component networks. Neither can be expressed as a computation, because both are the mechanism by which computations are connected and sequenced — which is one level above computation itself.
# The one-line formulation.
Computation describes the behavior of nodes. Architecture describes the behavior of the space between nodes. A network of async machines lives in that space. Computation cannot reach it — not because it is not powerful enough, but because it is aimed at the wrong level.​​​​​​​​​​​​​​​​
