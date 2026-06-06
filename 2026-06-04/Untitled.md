
Here’s a Substack article in your voice:

Programming Language Schizophrenia

TL;DR Programming languages try to do two incompatible things at once. That’s the problem.

L;R

Two Domains

There are two distinct domains in programming.

The first: bit and byte manipulation. Raw CPU operations. K&R C lived here. UNIX v1 was written in PDP-7 assembler. K&R C lifted UNIX v4 out of assembler into something barely above machine code. That was the point. K&R C was a thin skin over the hardware.

The second: semantic description. User-defined data layouts. Concepts. Abstractions the CPU knows nothing about.

Modern languages try to occupy both domains simultaneously. That is schizophrenia.

The Bool Problem

In the bit-twiddling domain, a bool is a byte. Any 8-bit operation on it is valid. Addition. Subtraction. Bit masking. The hardware doesn’t know what a “boolean” is. It knows ==octets==.

In the semantics-first domain, a bool is a boolean. No arithmetic operation is valid. The meaning is the point. The bits are an implementation detail.

ANSI C tried to bridge both. Modern languages — C++, Rust, even Python — attempt the same bridge. They twiddle semantic concepts and bit-level representations in the same syntax, the same type system, the same language.

That bridge is the problem.

One Language To Rule Them All

We keep reaching for a single language that handles everything. High-level design. Low-level opcodes. User-facing semantics. Hardware-facing bits. One syntax. One compiler. One set of rules.

This is the wrong goal.

What we need are languages for semantic description — tools developers use to express designs. What a system does. How components relate. The concepts, not the encoding.

And we need separate languages for low-level composition — tools that deal in opcodes, memory layouts, byte boundaries.

What we don’t have are transmogrifiers: tools that take semantic descriptions and produce bit/byte-level descriptions. Compilers gestures at this. They don’t deliver it. A compiler that takes C++ and emits x86 is not a transmogrifier — it’s a translator between two languages that are both confused about which domain they inhabit.

The Historical Accident

K&R C was honest about what it was. A portable assembler. Ritchie didn’t pretend otherwise.

The confusion crept in later. Languages accumulated semantic features while keeping the bit-twiddling substrate. The result is neither a clean semantic layer nor a clean systems layer. It’s both, badly.

Hardware engineers didn’t make this mistake. VHDL describes hardware behavior at a semantic level. The synthesis tools transmogrify that into gate-level netlists. Two languages. Two domains. A defined boundary between them.

Software never built that boundary.

What’s Missing

The missing piece is explicit separation. Not one language that does everything. Two languages — or two clearly separated notations — with a defined, automatable boundary between them.

The semantic layer describes what. The bit layer describes how the hardware encodes it. A transmogrifier connects them.

Until we build that, we keep writing languages that are schizophrenic by design.

Tagged: programming languages, PBP, notation, one-language-to-rule-them-all