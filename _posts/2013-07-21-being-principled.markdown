---
layout: default
title: Being Principled
date: 2013-07-21 00:00:00 
---

I experienced some frustration the last few weeks with some of our "software craftsman" abandoning craftsmanship principles in the face of "the business" telling them to go faster. The responsibility of the business is to always challenge us to go faster. Our responsibility as principled craftsman is to always strive for quality in our work. Quality in our work usually means that the "cost" of development efforts goes up. The cheapest software (in the long run) is the kind we never have to write. We should be striving to write as little code as possible, always be testing, and refactoring. A principled craftsman doesn't offer up his methods as places to remove scope. Instead, we should offer options that remove gold platting. When refactoring, we should be reviewing if a feature is even used. Replacing YesterdaysCoolJSLib with TodaysCoolJSLib isn't insulating us from having to rewrite with TomorrowsCoolJSLib.

One thing I've learned about people in management positions is that they are usually more than willing to make decisions for you. I suppose that sounds bad, but in classic command and control organizations that is middle management's role. Individual contributors and senior management throw all their problems at middle management and expect results. It's of little surprise that people put in that situation are prone to making decisions when presented with problems. So if you offer core parts of your craft as impediments, many pointy-haired boss types will quickly remove those for you.

* **Offering**: Testing all the time is tedious and not coding. 
* **Response**: Then don't do it.
- - -
* **Offering**: Writing automated tests slows us down now. 
* **Response**: Then don't do it.
- - -
* **Offering**: Building an automated release will make us deliver fewer customer features. 
* **Response**: Then don't do it.
- - -
* **Offering**: Learning new technologies is difficult. 
* **Response**: Then don't do it.
- - -

There's a trend there that you can't blame the pointy-haired boss for. Software development is challenging work on its own and adding in elements of craftsmanship makes it even more challenging. However, most of us started doing this work for the challenge. In most interviews, I hear people say they are most passionate about solving problems. Our goal should always be to deliver quality above all things in those solutions. To be clear, quality doesn't always mean fully-featured, fancy, sexy or sweet. Quality means our work product is reliable and maintainable. We know most of our code is going to need to change. Put the work in now to make the code maintainable in the future. Code that tests, releases and monitors itself will enable the future developer to know if his or her changes broke something quickly. By the way, that future developer is probably you in many cases.

Overall, being principled means you protect your values. You don't sacrifice them to the pointy-haired business god at first rumble of thunder. You work with your team to figure out how to deliver what the business needs with the highest quality. You take pride in your work. You go the extra mile to make sure the work is done well. If you find yourself adding caveats to your work like "no one uses this anyway" or "this is just for internal users" or "the pointy-haired boss said is was ok like this" then you probably should revisit your principles. 