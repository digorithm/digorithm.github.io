---
layout: default
title: On machine learning enhanced software systems
keywords: "architecture, software, machine learning, systems, machine learning for systems"
tags: [architecture, software, machine-learning, thoughts, systems]
fullview: true
permalink: post/machine-learning-enhanced-software
categories: [software-architecture, systems, machine-learning]
---
I have been brewing up the idea of, somehow, using machine learning to improve software systems since 2016. It was pretty vague and broad, without an actionable plan. I just had the intuition; software configuration and tuning, especially after the adoption of microservices, was getting too complex.

This was not a recent concern, dating back to 2003, when Ganek and Corbi discussed the need for autonomic computing to handle the complexity of managing software systems. They noted that managing complex systems became too costly, labor-intensive, and prone to error due to the pressure engineers felt while maintaining them. This increased the potential of system outages with a concurrent impact on business [1].


Even nowadays, most of the configurations and tuning of the systems are performed manually, often in run-time, which is known to be a very time consuming and risky practice. [1, 2, 3]

The need for autonomic computing isn't a new thing.

If most of these decisions to configure and tune the system are made based on the context -- many different variables such as workload, number of instances of some services, resources usage, and more -- why not delegate these tasks to something that excels at exactly that?

Machine learning sounds like a feasible tool for the job.

Since I started my Masters at the University of British Columbia I kept working on this idea, which seemed interesting although quite weird and, sometimes, unpractical and impossible to implement.

To my surprise, I realized I wasn't alone and some very interesting people were working on these ideas, meaning that it might not be that weird, unpractical, and impossible.

A few day ago, Jeff Dean -- a man that I admire a lot -- [gave a talk at NIPS 2017 talking about machine learning for systems](https://news.ycombinator.com/item?id=15892956), where he stated:

> Learning should be used throughout our computing systems. Traditional low-level systems code (operating systems, compilers, storage systems) does not make extensive use of machine learning today. This should change!

> Computer systems are filled with heuristics: Compilers, networking code, operating systems. Heuristics have to work well “in general case”. [They] generally don’t adapt to actual pattern of usage and  don’t take into account available context.

> Meta-learn everything: learning placement decisions, learning fast kernel implementations, learning optimization update rules, learning input, preprocessing pipeline steps, learning activation functions.

> Keys for success in these settings: (1) Having a numeric metric to measure and optimize; (2) Having a clean interface to easily integrate learning into all of these kinds of systems.

> Learning in the core of all of our computer systems will make them better/more adaptive.

I was in complete awe when I read this. One of the engineers I admire the most was talking about the very same ideas I've been thinking and working on.

This led me to think that it's not only interesting but natural to think about enhancing software systems with machine learning. Throughout the whole software stack we have many heuristics that, although they work well, could be improved by machine learning.

Is it challenging and potentially risky? Yes, most definitely. Especially given that interpretability, apparently, has become a secondary goal in the machine learning community. How can we interpret and explain the decisions made by neural nets?

However, with that said, these obstacles shouldn't hinder scientific and technological progress. [Yes, we should question old paradigms](https://arxiv.org/pdf/1712.01208.pdf) and try to improve things.

As Jeff Dean pointed out: we need to find _practical_ ways to make systems data-aware, as in systems that collect metrics and metadata about themselves, to achieve this we could learn a thing a two from the ideas in systems observability and instrumentation: We have been instrumenting systems for decades, the data is already there.

We also need to find _practical_ and _clean_ ways to _integrate_ machine learning components into software systems, making learning a first-class citizen in the system. Leading to _systems that learn how to improve themselves;_ beating heuristics and manually-performed operations. Think about this for a second, it does sound cool _and_ feasible; I'm not talking about AGI or anything dystopian.

I would also add that we need _practical_ and _clean_  ways to propagate the decisions made by the learned models to the rest of the system, allowing the system to have self-adaptive capabilities. Here, we could learn something from the control theory community.

**The general idea is fairly simple**: make a system learn about its behaviour by training a model on its context -- its workload, overall system usage, and organization/architecture -- then allow it to change its structures and configurations in order to optimize for a certain scenario. Now implement this idea in such a way that it could be possible to integrate it into many kinds of systems.

The most interesting questions I have in mind are:

1. Can self-adaptation by learned models lead to more stable, faster, safer software systems?
2. Can self-adaptation by learned models reduce the need for manually configuring and tuning systems, allowing engineers to focus on more important tasks?
3. Can this be easily integrated into software systems, requiring only small changes to the codebase?
4. Can this work with low overhead?

It is worth noting that this would *not* replace good engineers, but rather free the engineers' cognitive abilities to focus on what matters.

Can you imagine living in a world, where you don't have to be dragged onsite at 3 AM every time an outage happens due to misconfiguration?


---

[1] [The Dawning of the Autonomic Computing Era](https://pdfs.semanticscholar.org/9e58/7266bfb13e39a9f722ae240a7a78ae7380ea.pdf)

[2] [Software Engineering for Self-Adaptive Systems II](https://link.springer.com/book/10.1007%2F978-3-642-35813-5)

[3] [Using Probabilistic Reasoning to Automate Software Tuning](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.90.8651&rep=rep1&type=pdf)
