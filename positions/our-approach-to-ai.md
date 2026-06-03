---
status: review
last-reviewed: 2026-06-01
owners: [adam]
version: 0.1
---

# Our Approach to AI

*A public statement of Civic.Social's philosophy, commitments, and intentions over time*

Civic.Social is an open, federated platform for civic participation. We build with artificial intelligence, and we do so deliberately — with a clear view of both what it makes possible and what it puts at risk. This document explains how we think about AI, the principles that constrain us, the tensions we are carrying, and what we commit to as the technology and our own platform evolve. We publish it so that partners, technologists, and the communities we serve can understand where we stand and hold us to it.

---

## Why we are saying this out loud

Generative AI has been built, for the most part, on a particular set of choices: vast non-consensual collection of people's data and creative work, opaque provenance, heavy resource consumption, and tools that increasingly speak *for* people rather than *to* them. Those are choices, not inevitabilities. A civic platform — one whose entire purpose is authentic human voice in a democracy — cannot adopt such tools uncritically. So we are stating our position plainly, including the parts that are uncomfortable, rather than leaving them to be inferred.

We are optimistic about what AI can do for civic life. We are also unwilling to let the prevailing way of building it become the only way. This document is how we reconcile the two.

---

## The tension we are carrying

We want to be honest about a real tension at the center of this work, because the alternative is to let it be discovered rather than disclosed.

Civic.Social is a consent-based, user-controlled, open-standards project. Yet we build it using AI coding tools whose underlying models were trained on large-scale web data collected without consent — the very practice we organize our own platform against. We do not think this contradiction dissolves on inspection. But it is worth understanding precisely, because precision changes what it obligates us to do.

It helps to separate two very different uses of AI:

**Building the software.** We use AI assistants to help write Civic.Social's code. The leading concern with such models is that their training data was gathered without consent, raising both privacy harms and the unconsented use of writers' and artists' work. Our use does not cause or extend that collection — the training already happened, and our work adds nothing to the scraped corpus. In the framework of the UN Guiding Principles on Business and Human Rights, this is the weakest form of involvement: a *linkage* through the tools we rely on, not a *causing* or *contributing*. The tie is real, but attenuated.

**Operating the platform.** This is entirely different, and it is where our binding commitments live. AI that summarizes positions, drafts citizen input, ranks a feed, or moderates content can manipulate, mistranslate, or misrepresent the very people it is meant to serve. Here we are not a distant link in someone else's supply chain — we are the ones making the choices, and we hold ourselves to a far higher standard accordingly.

We will not let discomfort about the build phase distract from the deployment phase, where the real power over citizens actually sits.

---

## Why there is no clean escape — and what that means

A fair question follows: if the training data is the problem, why not simply use a "cleaner" model? We looked hard at this, and the honest answer shapes our whole approach.

There is, at present, no frontier-capable model with clean training-data provenance. In particular, **open-weight models are not the ethical upgrade they are sometimes assumed to be.** Models whose weights are openly published were, by and large, trained on the same web-scraped data as the proprietary ones — and in some documented cases on worse, including pirated books and platforms' own users' posts. "Open weights" describes how a model is *released*, not how its data was *gathered*. Choosing one does not resolve the consent problem; it often relocates it.

So the real choice is not between a tainted tool and a pure one. It is between building open, consent-based civic infrastructure with the imperfect tools that exist — or not building it at all. Refusing the tools undoes none of the past collection; it only ensures the public-interest alternative never gets made. We have concluded that building Civic.Social, with eyes open and safeguards in place, serves the values in this document better than abstaining would.

That conclusion is not permanent. It is a judgment about *this moment*, and the moment is changing. Where genuinely lower-impact options exist today — small, domain-specific models trained on local or licensed data — we already prefer them. Where cleaner-trained large models mature, we intend to move toward them.

---

## What we believe

**Consent is the foundation, not a setting.** People should control information about themselves — what is collected, how it is used, and whether it leaves their hands. We build on open, user-controlled identity standards (W3C Decentralized Identifiers and Verifiable Credentials) precisely because they invert the extractive default. Data about a person belongs to that person. This is the opposite of the scrape-first logic that has shaped most of the current AI landscape, and the contrast is intentional.

**Transparency is owed, not optional.** People have a right to know when they are interacting with AI, what it is doing, and on whose behalf — and a right to an honest account of the tools we ourselves rely on, including their contested origins. We do not pretend the foundations are clean. We name what we use, why, and what we are doing to improve it over time.

**Humans hold the authority. AI never does.** Inside Civic.Social, AI is assistive scaffolding that a person directs and can always override. It may help someone draft, summarize, or find — but it does not decide, rank away, or speak as if it were a citizen. Automation bias, the human tendency to defer to a machine's suggestion over one's own judgment, is especially corrosive in a tool meant to surface genuine public voice. We design against it on purpose: AI output is editable, attributable to the machine, and easy to reject.

**Restraint is a design value.** Bigger is not automatically better. We favor the smallest, most efficient model that does a given job well — both because smaller, domain-specific models are often more accurate on narrow tasks, and because the environmental cost of large-scale AI is real and unevenly borne, falling hardest on communities least responsible for it. Lower resource use is not a compromise we tolerate; it is a standard we prefer.

---

## What we commit to

These are commitments we intend to be measured against.

- **In the platform we operate, we will not build on non-consensual collection of our users' data.** Civic.Social is consent-based by architecture, and we will keep it that way. We hold this line over the parts of the system we control, even as we acknowledge the contested provenance of the development tools that help us build it.
- **We will be honest about the tools we depend on.** We will disclose the AI we use, in terms a non-expert can follow, name the provenance concerns that come with it, and keep that account current as our tools change.
- **We will keep humans in control** of any AI feature that affects what citizens see, say, or how their contributions are treated — with no autonomous suppression of content.
- **We will right-size our models**, defaulting to low-resource, domain-specific options for narrow tasks and reserving large models for work that genuinely requires them.
- **We will treat fairness across languages and communities as a release requirement**, not an afterthought, for any feature that moderates, translates, or ranks.
- **We will stay portable.** We architect our use of AI so we are not permanently bound to any single model or vendor — which is both sound engineering and the practical means of moving toward cleaner-trained and lower-impact options as they mature.

---

## How this will change over time

We expect to revise this document. The AI field is moving quickly, and some of today's unavoidable compromises — particularly around training-data provenance — may not be unavoidable for long. Models trained on licensed or opt-in data, and smaller models suited to civic tasks, are an active frontier we intend to follow and adopt where we can.

Our direction of travel is fixed even as the specifics shift: toward more consent, more transparency, lower resource use, and firmer human authority over the machine. If we ever drift from that direction, we want this document to be the thing you point to.

---

*This is a living document. We welcome scrutiny, correction, and contribution from the communities and technologists who care about getting this right.*
