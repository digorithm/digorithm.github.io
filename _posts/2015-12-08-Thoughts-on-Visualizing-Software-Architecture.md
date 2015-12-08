---
layout: post
title: "Thoughts on Visualizing Software Architecture" 
categories: [general, setup, demo]
keywords: "machine learning, artificial intelligence, engineering, tutorial, sentiment analysis, nlp, python, scikit learn, sklearn"
tags: [machine learning, artificial intelligence, engineering, tutorial, text classification, NLP, Sentiment Analysis]
fullview: true
permalink: post/thoughts-visualizing-software-arch
---

The more I think about the advantages and disadvantages of upfront design, the more I find it a really complex subject, I mean, the whole Software architecture diagramming in general. I've been following the development of the [C4 model](http://www.codingthearchitecture.com/2014/08/24/c4_model_poster.html) for a considerable time, I find it interesting, but it still brings me the old struggles about how much detail we should put into the diagrams and whether we should do it while planning the Software, while developing it, or after *(by doing reverse engineering)*.... or even more extreme, doing these 3 options, developing the architecture diagrams and evolving them over time.

I think that, when drawing architecture diagrams, we should focus on getting the right purpose and the right level of abstraction. Two of the most important purposes to me, when designing a diagram, is:

1. tech stack architecture and how each container and component relate to each other

2. deployment architecture

I do think that the diagram should reflect the code in certain level of abstraction and that this could be really useful for a new engineer in a large project. Tackling a huge codebase is not an easy task, and the diagrams should act as a map of the code, saying how things communicate with each other.

What I think is, we need certain kinds of architecture diagram, each one for a specific development phase. What comes to my mind is:


**Project planning phase**, We can't code without any plan, at this level I feel necessary to draw the most 'visible' containers and components, this way we can plan what to do next, how to split up teams to each part of the system. It's when a deployment architecture can be written as well

**Project evolution and maintenance**, as we reach this phase of the project, it's interesting to see the Big Picture, the Not-So-Big Picture and the Small Picture. I can see many cases where this could be helpful, for instance: new engineers coming to the project, debugging something that crosses many components or even containers *(visualizing the flow can be helpful)*, finding bottlenecks, and I'm sure there are many more.

Given this task, I'd prefer taking a top-down approach, going from a more "Big Picture" architecture to a more detailed one. As an example, I'm going to use a side project I was working on, which is a simple webapp to search for simple food recipes *(the difference, though, is that it crawls the recipes from other websites)*.

I took a backend&api + ClientApp approach, which means, in the backend is where the main logic happens, the core business classes and everything, in addition to exposing a HTTP API *(I would say REST API, but I'm scared of a few rest purist coming here and saying that this is not restful because of this and that, so let's say it's a simple api that talks over http)*.

Each of these 2 parts will be a different application, which can be dockerized and run in different plataforms *(two separate amazon EC2, for example)*. So, if we're planning it, we could start by saying:

![](/content/images/images/gdd20overview.png)

So, yea, the webclient will talk to the backend's api over HTTP, the backend will communicate to the webclient and to a database instance. Ok, that's a start. We can easily separate a team for the backend and its api and another team for the webclient.

Still, it doesn't say much about the inner details of each container, and now we need something more detailed. So we detail it a bit more:


![](/content/images/images/gdd20arch.png)

As we can see now, it's the same structur*e, **but with way more details now, which makes it clearer to everybody *(I hope so)* how we can tackle the problem with code. An extra level of detail would be going down one more level of abstraction and describe the classes through a class diagram, explaining how the core backend would be implemented, for example.

Now, this diagram, plus all requirements gathered can be a nice start for the coding phase. We can add now how we would deploy it:


![](/content/images/images/gdd20deploy.png)

So, in the end, I believe those 3 diagrams + the codebase could be helpful for a new engineer or to someone that is debugging the system or trying to improve it.

Though, I still have many questions and doubts about this, I can't stop overthinking it, so I'm extremely open to any discussion about this.
