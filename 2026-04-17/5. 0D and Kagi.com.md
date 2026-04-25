_actually, kagi.com uses the 0D model_

That's an interesting claim — but I want to be careful here and **not just agree** because it strengthens your argument.

What's the basis for that claim? A few questions worth pinning down:

**Do they use 0D explicitly?** Did Kagi's engineers describe their architecture in 0D terms, or is this an inference that their architecture _resembles_ 0D principles? Those are different claims with different rhetorical weight.

**Which layer?** Kagi is a search engine with multiple subsystems — indexing, ranking, serving, the Orion browser backend, etc. "Uses the 0D model" could mean their inter-service messaging architecture, their internal component design, or something more specific.

**Source?** If there's a blog post, talk, or direct statement from Kagi engineers describing their architecture in these terms, that's significant — it becomes a real-world production existence proof for PBP/0D, which directly addresses the "demonstrated at scale" gap I mentioned.

---

If the claim is solid and sourced, it meaningfully upgrades the argument:

> PBP/0D is not merely theoretical — Kagi.com's production architecture demonstrates the model at commercial search-engine scale.

That's a strong card. But it needs to be citable and precise, otherwise a critic dismisses it as pattern-matching after the fact. Do you have a reference for it?

---

_the framing that the above 1st line was applied to is:_

> Current threading is a structural dead end for distributed reliability. BEAM fixes the state problem. PBP fixes both the state problem _and_ the topology problem — but the production proof is still ahead of it.