---
layout: post
title: Antiques Roadshow Rule
date: 2013-12-20 00:00:00
---

Often, my wife will put Antiques Roadshow on right before my son is getting read for bed. The two most enjoyable parts of the show are 1.) when people find out something they thought was junk is worth a fortune and 2.) when people think something is worth a fortune and turns out to be worth considerably less. I think 2.) is applicable in the world of corporate software development and I'm coining it the "Antiques Roadshow Rule."

	The Antiques Roadshow Rule - just because you believe something is valuable does not make it true.

There is a common practice in large corporate software factories to label all source code as "intellectual property." The belief is often that because we paid a bunch of professionals to create something it needs to be protected as private to our business. My point in writing this is not to say that all software should be open source. I think there is a happy medium. I think there is software that is central in differentiating a business. I make my living writing software and I think others should have the ability to do the same. What I disagree with is the idea that every piece of software that is written at a company should be closed source.

I suppose my proposed rule is a little heavy-handed in the use of the word valuable. I don't mean to say that most of the software we churn out everyday has no value. What I am saying is that, most of the software we write everyday is not what gives us the competitive business advantage. Just because we paid our developers to write the code does not mean that it is exclusive intellectual property.

I'd love to see more companies use a model like [Netflix](http://netflix.github.io/#repo). Netflix strives to open source any software that is not critical to their core business functionality. At least that is what I heard last year at Code Mash in a presentation about their OSS stack. They have plenty of code out there so I have little reason to doubt them.

###Good Enough For Prod, But Not Open Source
I heard this during the talk I attended about Netflix OSS. The speaker mentioned they have lots of code they would like to open source, but some of it would need to be cleaned up first. His line was "It's good enough for production, but not good enough for open source yet." I knew exactly what he meant, and I thought the statement was very powerful. Sunlight is a great disinfectant. There is a lot more pressure to clean up your code if you know the world can easily see it. Open source is a great way to make sure you are putting out clean code.

###Decouple
There are many ways to decouple code and components. By open sourcing modules, you can easily ensure they are loosely coupled to other systems and infrastructure. We currently maintain a lot of code that makes some big assumptions about systems and infrastructure being available. This code could be greatly improved if those assumptions could be removed. In addition to making the code more modular, it would also make the code more robust. Certainly, systems can be decoupled without open sourcing them but it would hopefully help make it more obvious when we have code that is coupled to something internal.

###Recruiting
There is much internet discussion about the best ways to recruit and interview developer candidates. I personally hate asking candidates to solve a brain teaser or algorithm type problem in an interview. I like working through some code, but I also feel bad when a candidate has very little context to a problem. Having some publicly available code that we could point to ahead of time might help on both sides of the interview table. It would be nice to tell a candidate, "we will do development against this class in your interview, so you might want to look at it ahead of time." This could help the candidate feel more confident at what is coming in the technical interview. It can also help the interviewer by providing more up front context and getting more true interaction with the candidate on the code. It will also give you a good read on how much drive a candidate has in digging into the code. Finally, it could also be used in place of the project based interview where a company asks you to develop something for free so they can then critique your code. 

###Giving Back
Usually it isn't hard to get corporate sponsorship or volunteer time from employees for charity. So why should it be so hard to give some code back to the community? Every developer I know consumes free information and software. Why can't we give back what isn't key to our business?  I think as developers we need to be diligent about encouraging our employers to give back when we can. 

