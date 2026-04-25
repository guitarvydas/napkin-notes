**BEAM is a virtual machine**, not physical hardware — I used "hardware" loosely to mean "the canonical platform purpose-built for this model." Apologies for the imprecision.

**Erlang's process model:**

Each Erlang process has:

- Its own **private heap** — no other process can touch it
- Its own **stack and mailbox**
- No shared mutable state, period

When process A sends a message to process B, the data is **deep-copied** from A's heap into B's mailbox. After the copy, A and B have no aliasing relationship. Neither can corrupt the other.

**Why this matters:**

- A process crash cannot corrupt another process's memory
- The GC runs _per process_, on small heaps — no stop-the-world pause
- Processes can be on the same machine, different cores, or **different physical machines** — the send primitive looks identical in all cases
- This is where "long-distance" comes from — the model scales from intra-process to distributed clusters without changing the programming model

**Your PBP/0D critique of Erlang:** You've noted that Erlang still has **topology-blindness** — processes know each other's PIDs (or registered names), meaning the wiring is embedded in the code rather than being an external, inspectable, reconfigurable structure. A component reaching out to grab a PID is still a form of coupling, just asynchronous coupling. PBP externalizes the wiring entirely; Erlang doesn't.

**So the distinction is:**

- Erlang solves _shared state_ correctly
- Erlang does _not_ solve _topology_ — who is connected to whom is still implicit in code
- PBP/0D addresses both: no shared state _and_ explicit external wiring

BEAM is the closest mainstream platform to your model, but it stops one step short.

[_note to reader: "my model" refers to the PBP/0D model that I've been writing about using Claude. Claude seems to remember the issues that I've discussed._]