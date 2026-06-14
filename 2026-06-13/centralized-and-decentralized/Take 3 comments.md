Most of the article is OK, but I want to further elucidate the section "**The development environment needs to change too**".

Currently we fake asynchronous processing as if it were synchronous.

Then, we ship the whole development environment to users. Users have to buy computers with operating systems that support faked multi-processing.

[the paragraph "We need to invert this. ..." is OK]

When we switch to using development systems that fake asynchronous message-passing on a single machine, one would think that apps on these development systems will operate more slowly. But, it's not clear that this is the case. We avoid memory-sharing by default and this will slow down apps that actually benefit from bulk data transfers, but we will not tax apps that don't need this optimization. The current version of PBP builds fake asynchronous processing from scratch, in a green-thread manner. This is implies that PBP doesn't actually need operating systems as we know them (this hasn't been tested), which means that the target hardware doesn't need to be burdened with bloated operating systems nor the costs of time-sharing and context switching and the overhead of complicated algorithms inside the operating system.

We should be able to ship simpler, leaner systems to users, not copies of our development systems.

- current development workflow creates single sync nodes, then allows programmers to make them reactive so that we can plumb them together in manual ways. This is essentially programming async in assembler instead of some higher level form.

given that our current workflow is geared towards making clockwork code for single nodes, we can continue to use current software by wrapping it appropriately as single nodes in a grander asynchronous scheme