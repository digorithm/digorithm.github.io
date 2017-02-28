---
title: Why, how and where to learn Design Patterns
layout: default
categories: [software-architecture, software-engineering]
tags: [design patterns, tips, thoughts, software engineering, programming]
keywords: "design patterns, tips, thoughts, software engineering, programming"
fullview: true
permalink: post/learn-design-patterns
---

The first thought I had when I started to study [Design Patterns](http://en.wikipedia.org/wiki/Software_design_pattern) was:

>_- “damn, is all this really necessary?”_	

I mean, so many patterns, so many details to do things that look simple to do without any further detailed thoughts. And I started to question myself if this was really a productive thing. The first answer I gave was:

>_-”no, *wonderful not-bad-word* this, I’m losing too much time trying to wrap my head around this”_

<!--more-->

So I forgot it and kept developing my software using something like [Extreme Go Horse](https://gist.github.com/banaslee/4147370#file-xgh-en-txt). (i was younger at that time). Then, I saw a pattern being formed:

###Every time I came back to my old codes I couldn’t extend nor debbug it in a easy way. It was always extremely hard to work on it.

And then I realized why: **lack of good architectural decisions.**

So I decided to give another chance to learning Design Patterns and finally I understood its beauty.

## Few things to keep in mind:

1\. It’s not about trying to fit every problem in every Design Pattern. That’s just a waste of time

2\. It may be not very productive at start, the process of applying a DP in a determined problem takes time.

3\. On the other hand, it will be extremely productive when you try to debug your code or try to extend it. Everything starts to make more sense and it’s way easier to change things when you’ve followed some patterns

4\. When studying a Design Pattern, try hard to implement that solution in a problem. This will make total difference

## Now, to the materials!

Start with the classics:

#### [Design Patterns](http://www.amazon.com/Design-Patterns-Object-Oriented-Professional-Computing/dp/0201634988) (a.k.a Gang of Four)

This is the all times classic. These four gentlemen were the firsts to formalized and compile all patterns found on Object Oriented Systems. Here my advice is to go one by one, pick a pattern, read it, understand it, implement it and ,finally, really understand it.

&nbsp;

#### [Head First Design Patterns](http://www.amazon.com/Head-First-Design-Patterns-Freeman/dp/0596007124)

This one I’ve used when I didn’t understand a concept from the Gang of Four, it is easier to grasp the concepts, the approach is way less hardcore than the original GoF, but it still is a great book.

### Code Samples

That said, it’s always good to have examples of each Design Pattern by your side, so you can study the DP and check other examples, so real world examples are perfect for it:

[List with Examples of Patterns implemented in the Java SE and EE API](http://stackoverflow.com/questions/1673841/examples-of-gof-design-patterns/2707195#2707195)

[List with Examples of patterns demonstrated with C#](http://www.dofactory.com/net/design-patterns)

[Excellent examples and explanations of each Design Pattern](http://sourcemaking.com/design_patterns)

[Another great material on Design Patterns, with lots of great thoughts on it](http://c2.com/cgi/wiki?PeopleProjectsAndPatterns)

I believe this is a good start to learn Design Pattern and I hope this helps someone, mainly if they’re struggling with the same doubts I had when trying to learn it.

Oh, one more thing: **Do not ever go Extreme Go Horse, really.**
