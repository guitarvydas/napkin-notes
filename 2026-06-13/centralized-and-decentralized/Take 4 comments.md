==we pretend the network call blocks cleanly, we hide the asynchrony and then spend enormous effort managing the consequences when the illusion breaks.==

I like the second part of this sentence, but I'm unclear about what "we pretend the network call blocks cleanly" means

---

It means: we write code that calls a remote service as if it were a simple function call that pauses and waits for a result — as if the network were just a slow piece of memory. The "blocks cleanly" part implies the call will always return, always in order, always without dropping the connection. None of that is true on a real network.

But the phrase is jargon-dense and the image isn't vivid. A few alternatives:

**Option A** — make the lie explicit:

> we write remote calls as if distance does not exist

**Option B** — focus on the false guarantee:

> we write remote calls as if they are guaranteed to return

**Option C** — cut it entirely and let the second half carry the sentence:

> We build systems that fake asynchronous processing as if it were synchronous — we hide the asynchrony and then spend enormous effort managing the consequences when the illusion breaks.

Option C is probably the cleanest. The dropped phrase was doing work that the rest of the sentence already does. What do you think?

--- 
I like the full explanation given after "it means:". Please reword it to fit with the rest of the article.

---

Replace that clause with:

> we write code that calls remote services as if they were simple function calls that pause, wait for a result, and always return — as if the network were just a slow piece of memory

So the full sentence becomes:

> We build systems that fake asynchronous processing as if it were synchronous — we write code that calls remote services as if they were simple function calls that pause, wait for a result, and always return — as if the network were just a slow piece of memory — then we hide the asynchrony and spend enormous effort managing the consequences when the illusion breaks.

The triple dash rhythm is intentional — each clause tightens the indictment one step further. But if you find that too staccato, the middle clause can fold in more quietly:

> We build systems that fake asynchronous processing as if it were synchronous — we write remote calls as if the network were just a slow piece of memory that always returns a result — then we hide the asynchrony and spend enormous effort managing the consequences when the illusion breaks.

Which rhythm fits better?

---
---
I'm going to use the full sentence with the triple dash. I will paste this in manually, there is nothing more to do other than to note that I will make the change.

---
Noted. Good choice on the triple dash — it earns its punctuation there.

---
---
What should the title for this article be? I'm thinking about Something along the lines of "Moving to a Decentralized Model for Software"

---

Your instinct is good. A few variations to consider:

**Close to your suggestion:**

- "Moving to a Decentralized Model for Software"
- "The Case for a Decentralized Software Model"

**Sharper / more assertive (fits your article voice better):**

- "Software Should Be Decentralized"
- "Decentralize the Software, Not Just the Hardware"
- "The Centralized Model Was an Accident"

**Leans into the economic argument that opens the article:**

- "CPUs Got Cheap. Our Architecture Didn't."
- "The Hardware Changed. The Software Didn't."

The last two are the most in your voice — short, declarative, no hedging. "The Hardware Changed. The Software Didn't." also sets up the article's argument in a single sentence, which is what a good Substack title does.

---
I like "The Hardware Changed. The Software Didn't.", but want to add something about decentralizing. Maybe a subtitle?

---

How about:

**The Hardware Changed. The Software Didn't.** _A case for decentralized architecture_

Or sharper:

**The Hardware Changed. The Software Didn't.** _It's time to decentralize_

Or if you want it to echo the economic argument from the opening:

**The Hardware Changed. The Software Didn't.** _CPUs are cheap. Stop pretending they aren't._

The third one has the most bite and connects directly to the article's opening move. But the second is cleaner if you want the subtitle to stay out of the way of the title.

---
I'm going to combine them into:

**The Hardware Changed. The Software Didn't.** _CPUs are cheap. Stop pretending they aren't. A case for decentralized architecture._

Again, just note this change.

---
---

I want to rewrite a sentence in the article to:

This is programming asynchrony in assembler. We are manually encoding, at the application level, coordination that should be handled in a higher level, asynchronous architectural manner. 

---

Noted.

---
---

