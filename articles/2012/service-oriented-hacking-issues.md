# Randall Degges

## This is an archived post This is an archived post

[Previous][]   [Index][]   [Next][]

### Service Oriented Hacking - Issues

August 23 2012, 6:03 PM  by Randall Degges

![][]

Over the past several months, I've been spending most of my time writing APIs
for my new [company][]. As a [passionate programmer][], I obviously care about
not only the quality of my code, but its speed, effectiveness, and
maintainability.

If you're a developer, you're likely familiar with the term "[service oriented
architecture][]". In this post I'd like to briefly discuss some service oriented
issues I've experienced myself, and introduce some advice for getting past them.
While there are loads of resources out there are building service oriented
applications in theory, there are almost no practical references. I'm hoping
that this will be the first in a series of many practical service oriented
articles I'll write over the coming months.

Refactoring

 

If you're writing APIs, web portals, or high traffic websites, I'd say it is
almost critical.

 

There's a lot of talk in the developer community about building service oriented
applications as opposed to monolithic applications. Unfortunately, there are
very few talks, examples, and practical advice out there on how to properly
build service oriented applications.

While building service oriented applications are ideal in theory, I've found
that practically applying this knowledge to working codebases is usually fairly
difficult, so I thought I'd take a stab 

In this short article, I'd like to explain what all the fuss is about, and
tackle the largest problem in creating

If you aren't familiar with the idea, I'll quickly break it down for you.

Monolithic Codebases

Let's say y

#### 22 views and 0 responses

  [Previous]: ../../../posts/2012/08/become-a-better-programmer-monitoring.html
  [Index]: ../../../index-2.html
  [Next]: ../../../posts/2012/08/two-months-in.html
  []: ../../../image/2012/08/43913937-demon.jpg
  [company]: https://www.telephonyresearch.com/ "Telephony Research"
  [passionate programmer]: http://www.amazon.com/gp/product/1934356344/ref=as_li_ss_tl?ie=UTF8&camp=1789&creative=390957&creativeASIN=1934356344&linkCode=as2&tag=rdegges-20
    "The Passionate Programmer"
  [service oriented architecture]: http://en.wikipedia.org/wiki/Service-oriented_architecture
    "Service Oriented Architecture"
