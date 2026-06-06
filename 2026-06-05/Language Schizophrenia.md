
# Programming Language Schizophrenea

Some programming languages are meant to deal with bit and byte level issues.

Most programminng languages attempt to lay semantic nicetiies over top of the low-level basic operations provided by CPUs (coupled with RAM).

UNIX v1 was written in assembler (not C) and used PDP-7 machine-code concepts. K&R C lifted UNIX V4 from assembler to K&R C - supposedly a “high level language”.

Today’s concept of a “high level language” goes way beyond that of K&R C. These ‘high level languages try to unify semanitcs and. biyt-level twiddling. This is schizoprenia.

K&R C was meant to twiddle bits and bytes. Modern programming languages, including ANSI standard C, are meant to twiddle semantic concepts and user-defined data layouts, not bits and bytes.

In the bit-twiddling domain, a bool is a byte, hence any 8-bit operation on it is valid, including adition and subtraction.

In the semantics-first domain, a bool is semantically a boolean and no arithmetic operation is valid.


I think that the problem is “one language to rule them all”-ism,  We need languages that describe designs in high level, semantic ways, Languages to be used by developers. And we need _other_ languages that deal with low-level composition of opcodes.

What we need - and don’t have - are tools that transmorgrify semantic descriptions to bit/byte level descriptions.


