I wonder if our emphasis on AOT (Ahead of Time, "static") type checking is a viable way forward.

If we imagine a machine in New York, USA sending a message to another machine in London, UK, then I don't think that it's viable to use memory sharing strategies for the communication. As far as I can tell, most of our modern programming languages are based on the assumption that threads and cores can share memory ("thread safety" and all of that, very 20th century)

The information sent from NYC to London would have to be a simple message - a blob of bytes - sent in a fundamentally asynchronous manner.

Sharing a descriptor of a user-defined type, like a '.h' file in C, seems to be too risky. The receiver in London would have to check - at runtime in a JIT manner (Just In Time, "dynamic") the incoming data and see that it looks sensible an can be operated on.

Likewise, if a response were to be sent back to New York, it would need to be JIT checked on the fly.

The best you could do generically with the data packets is to perform some sort of CRC check to see that they appear not to have been corrupted in transit.

I have long believed that "static type checking" is a language unto itself.

If I'm right about needing JIT type checking for long-distance distributed computing, then we might begin treating "type checking" as a separate language and create pre (and post?) modules that verify incoming data before allowing it to be acted upon. 

This might imply that we need to pry type-checking out of our programming languages and make separate notations / languages for this concern.

- We could keep type-checking in languages, but languages would need to hide the fact that type-checking is done at runtime
- I favour separation of concerns and using multiple "views" on programs, instead of trying to force all concerns into a single notation / language.
- Scott Werner takes Alan Kay's ideas in 1997 OOPSLA keynote, to an extreme.  Werner interprets Alan Kay's ideas to mean determining *semantics* (not just syntax and layout) at runtime with [protocol objects](https://worksonmymachine.ai/p/what-if-we-took-message-passing-seriously).
- an idea for parsing incoming data on the fly might be to use OhmJS
---

The NYC→London example cuts right to the heart of it. Static type systems are a compile-time _optimization_ of a runtime check — they work precisely because both sides of a call share a compilation unit and, behind that, a shared address space. Cross that boundary and the optimization's precondition evaporates. You're left with bytes and whatever you can infer from them locally.

The `.h`-file-sharing strategy (and its descendants: IDL files, Protobuf schemas, OpenAPI specs) tries to smuggle the shared-compilation assumption across the network by distributing the type descriptor out-of-band. That's fragile for the reason you name: version skew, trust, and the fundamental fact that the receiver has no guarantee the sender compiled against the same schema. It's the shared-memory assumption wearing a disguise.

Erlang got closest to taking this seriously architecturally. BEAM terms are self-describing — an atom is an atom, a list is a list — and pattern matching is the receiver's runtime assertion about what it's willing to accept. That's JIT type checking, but it's still baked into the language rather than separated out. Clojure Spec is another partial move in your direction: validation as a distinct artifact you attach to functions at boundaries, rather than something the compiler weaves invisibly through everything. But neither goes all the way to _type-checking as a separate notation with its own pipeline stage._

Your pre/post module idea is the natural PBP reading of this. A Part that accepts raw bytes, runs them through an OhmJS grammar that embodies the expected shape, and either passes a validated, tagged message downstream or emits a rejection on an error port — that's topology-blind type enforcement. The downstream Part never sees unvalidated input; the type-checking Part knows nothing about what the downstream Part does with the data. Clean separation. The grammar _is_ the type language, and OhmJS is already in your toolbox.

Werner's protocol-objects move is even more radical: the receiver negotiates semantics dynamically, not just structure. That's closer to capability theory than to conventional type theory. It handles the case where the sender and receiver don't share a schema at all — only a willingness to probe what the other can do. Expensive, but honest about distributed reality.

The deepest implication of your argument is that _all_ type checking is runtime type checking; static type checking is just the special case where the runtime has been moved to before the program ships. Languages that pretend otherwise are hiding a cost, not eliminating it. Making it a separate notation makes the cost visible and composable — you can swap validators, version them independently, apply different rigor at different trust boundaries, and keep the core computation Part unentangled from the verification concern. That's exactly the multi-view, separation-of-concerns position you hold everywhere else. It would be strange to abandon it at the type level.

One concrete sketch: an OhmJS grammar defines the _shape_ of a message; a `.rwr` rewrite file lifts the matched fields into named slots; a downstream Part receives a typed record, not raw bytes. The grammar is the type declaration. Updating the type means updating the grammar — independently of the sender, independently of the receiver. That's a notation for types that composes with your existing pipeline rather than competing with it.