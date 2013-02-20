# Randall Degges

## This is an archived post This is an archived post

[Previous][]   [Index][]   [Next][]

### Service Oriented Problems

October 1 2012, 3:54 PM  by Randall Degges

![][]

In my [last post][], I discussed some of the benefits of writing service
oriented web applications. In this post, I'd like to discuss some of the
problems commonly encountered when writing service oriented web applications.

For the rest of this article, I'm going to assume you have some basic web
application programming experience.

Problem 1 - MVP

One of the largest complaints you'll hear about writing service oriented web
applications is that it takes longer to build a working MVP (minimum viable
product). What this means is that if you're writing your web application from
scratch, and have to launch an initial version as quickly as possible, SOA
(service oriented architecture) probably isn't for you.

The reason this is true is that while service oriented web applications are far
simpler to maintain in the long run (among lots of other benefits), it requires
more upfront development time to get things working. Let's look at *why*.

Let's say you're building a blogging engine (something like [tumblr][]). If you
were start building your product as services, you'd start by writing:

-   A **tumblr-accounts** service which provides an API to create, edit, and
    remove user accounts as well as authenticate users.
-   A **tumblr-api** service which provides an API to create, edit, and publish
    a user's blog posts.
-   A **tumblr-www** service which provides a user facing website, and makes use
    of your **tumblr-accounts** and **tumblr-api** services to build its
    functionality.

**NOTE**: For something as large and ambitious as tumblr, you'd likely want to
have more than the three services listed above. If you are getting started with
SOA, however, always writing an **accounts**, **api**, and **www** service is a
good starting point.

Through the process of writing your accounts, api, and www services you'll also
be faced with another obstacle: defining a clean and clear API that you'll be
using to work with your data. When you begin writing services, you can't
half-ass anything--you have to make a working API for your accounts project, and
a working API for your api project--otherwise, nothing else will work.

Compare this with writing a single monolithic project. When writing a single
large application you can typically make time saving trade-offs as you're
writing your MVP. For instance, instead of maintaining separate databases for
your user accounts and your blog posts, you can throw them all on a single
database. This means you can also use your framework's ORM (object relational
mapper) to create, edit, and delete users and blog posts without having to worry
about writing any sort of APIs.

Over time, as your application grows in features and complexity, having separate
independent services that interact with each other via APIs is going to be a lot
simpler to maintain--but in the short term, it is typically much quicker to hack
together a single monolithic project as opposed to a nicely modularized service
oriented architecture.

Problem 2 - Authentication Implementation

If you've never written service oriented web applications before, the first time
you give it a try you'll likely bump into a frustrating problem: handling user
authentication properly for your website.

In my experience writing service oriented web applications, this is without a
doubt the single largest implementation issue people bump into. Let's *analyze*
the problem (and also discuss the solution).

Going back to our tumblr example from the previous section--we have **www**,
**api**, and **accounts** services. The authentication implementation problem
you'll bump into happens when you start writing your **www** service.

Most web frameworks are built around the idea of creating monolithic web
applications. Many web frameworks ship with an ORM, whose deep integration makes
handling certain types of problems very simple. For instance, using many web
frameworks (I'll use [Django][] as an example), you can easily log a user into
their account, allowing them to access certain web pages.

The way this works is that each time a user makes an HTTP request to your site,
Django will perform a database query to ensure the user account exists and is
properly identified. This ensures that a user is legitimate, and allows you (the
developer) to skip a lot of implementation details dealing with authentication.

When you move to a service oriented architecture, however, you no longer have an
ORM available to your **www** service. Instead of performing a database query
every time a user makes an HTTP request to your site, you need to make an HTTP
request to your internal **accounts** service, authenticating the user properly.

Since most web developers never have to worry about this sort of implementation
themselves, this can be a tricky thing to implement.

Solving this problem is typically different for each web framework. If you're
using Django, for instance, the solution is to subclass the default
authentication backend that ships with Django and override the necessary methods
to perform HTTP requests to your **accounts** service instead of performing ORM
queries. If you're using [Flask][] (another python web framework), the solution
is to use the wonderful third party application: [flask-login][].

Regardless of your toolset, this is almost always a problem people bump into.

Problem 3 - Cost

Another issue people frequently bring up when discussing SOA is cost. Depending
on your application requirements, cost can quickly become a prohibiting factor.

To truly get the benefits of a service oriented web application, you need each
service to be ran and managed independently of one another. This means that
you'd run each of your services on separate groups of servers, each with their
own dedicated resources (databases, caching servers, whatever).

If you have a very small application, and don't plan on getting a lot of usage
out of it--writing it in a service oriented fashion may be more work and more
expensive than it's worth.

One way around the cost issue, however, is to deploy your services to a platform
like [Heroku][]. Heroku's platform allows you to deploy your services live with
a generous free tier (a single free web server, free small databases, caching
servers, etc.). This can be a good option if you'd like to build a service
oriented web application without breaking the bank, as you can easily scale up
when your usage gets higher, and begin paying for higher levels of service as
your application grows.

Problem 4 - Documentation

Quite possibly the largest SOA issue you'll face if you decide to write service
oriented web applications is that there is very little practical documentation
around.

Many popular web frameworks, toolsets, books and classes only teach the
monolithic model of web application development. This means that there are very
few guides, and very few helpful resources out there to help you when you get
stuck.

Unfortunately, since writing service oriented web applications is a lot more
difficult to explain (and has varying requirements depending on your specific
use case), many books, guides, and tutorials don't teach it as it adds
unnecessarily complex material and mental models, which may scare off new
developers.

In the future, as SOA continues to become more popular, I'd be surprised if
there weren't more resources available. I'm guessing that in the next few years
there will be a large amount of books, tutorials, and guides written covering
these topics in more depth.

Wrap Up

If you've read all the way down to here, you probably have a good understanding
of the challenges you'll face writing service oriented web applications.

Writing services means you need to have more development discipline, maintain a
stricter set of APIs for your services, and generally invest more time upfront
when writing your application.

In my experience, writing service oriented web applications can be an extremely
effective way to build great products. Not only do you gain tremendous benefits
initially (a stricter set of APIs, redundancy at a service level, simpler
isolated codebases), but as time goes on these benefits become more and more
prevalent (it is easier to maintain your application, you can scale your
application quicker and more precisely, complexity drops substantially).

Regardless of how you write your applications, go hack something! \>:)

 

#### Tags

programming, webapplications

#### 6464 views and 9 responses

-   Oct 1 2012, 5:51 PM

    bobx responded:

    Actually the biggest problem with SOA is that decomposition along these
    lines offers minimal value and costs too much. You seem to think that theres
    SOA and monolithic app development. Have you heard of REST? Its really
    neither. Also I think there are plenty of component driven approaches which
    are definitely not SOA.

    Remember that SOA is really an enterprise IT integration strategy which most
    people could care less about. There are plenty of more natural ways to reuse
    code than SOA. In fact I think SOA in some ways creates more leaky
    abstractions than other approaches.

-   Oct 1 2012, 6:02 PM

    Jason responded:

    No, no and no.

    SOA does not mean "services first" it means "service oriented".

    You build a bunch of business functionality that's orthogonal to any other
    business concerns and expose that functionality as services.

    Starting with the service definition is a sure fire way to fail.

-   Oct 1 2012, 8:24 PM

    languagehacker (Twitter) responded:

    Thanks for writing this, Randall! Interesting perspective.

    What's your opinion of an integrated service layer within an application?
    I'm mostly talking about an architecture where the service-level logic is
    exposed via web services as well as opaque services. The web services here
    basically consist of lightweight controllers responsible for passing
    parameters sent via HTTP to a service class that encapsulate specific,
    atomic domains of functionality.

    I view this as sort of a nice intermediary that solves a lot of the
    authentication pains. You sort of see this in Symfony2, but it takes on many
    different subtle variations. Have you worked with these? Do you consider the
    strict HTTP version of SOA to be more pure, or more effective?

    Thanks!

-   Oct 1 2012, 9:31 PM
    Johnny Gamba responded:
    Hi, great post, i want to learn more about this SOA thing, could you point
    me to a book or something like that to know more of this.\
    cheers.
-   Oct 1 2012, 11:30 PM
    data\_shaman (Twitter) responded:
    Johnny Gamba: Try this: [http://lmgtfy.com/?q=SOA][]
-   Oct 2 2012, 1:34 AM

    Mikael responded:

    As @Jason said, "no, no, no". This is not SOA. This is just splitting your
    application into more servers. The "S" in SOA is not a physical thing, it's
    logical. And you should never, ever, do RPC calls (your http requests)
    between services. That is not loose coupling.

    Johnny Gamba, you could read Eric Evans "Domain driven design". Skip the
    first section and start at "Bounded Context". Or read everything from here
    [http://bill-poole.blogspot.se/2008/02/saas-service-vs-soa-service.html][]
    (all posts, not just that one).

    @data\_shaman, well, if you google for it, you might end up with "SOA" like
    this...

-   Oct 2 2012, 2:20 AM

    Pierre Chapuis responded:

    Maybe "SOA" is not the best word for what Randall describes, because for
    some "enterprise" people SOA means ESBs and SLAs, for others it means
    WS-\*...

    I think here we're talking "SOA" for web applications, not for a whole
    enterprise. What matters here is the decoupling of the separate functions of
    an "application" into separate processes that communicate via a network
    interface. Whether you use some kind of RPC, REST-ish interfaces or message
    queues matters technically but not so much at the conceptual level. To avoid
    confusion I sometimes use the word "blocks" to mean "services", inspired by
    this quote from "The Pragmatic Programmer":

    \> Sometimes, there are perfectly good jargon words for concepts, words\
    \> that we've decided to ignore. Why? Because the existing jargon is\
    \> normally restricted to a particular problem domain, or to a particular\
    \> phase of development. However, one of the basic philosophies of this\
    \> book is that most of the techniques we're recommending are universal:\
    \> modularity applies to code, designs, documentation, and team\
    \> organization, for instance. When we wanted to use the conventional\
    \> jargon word in a broader context, it got confusing-we couldn't seem to\
    \> overcome the baggage the original term brought with it. When this\
    \> happened, we contributed to the decline of the language by inventing \
    \> our own terms.

    I have been advocating this approach at the startup I work for and we have
    been using it for two years now. The alternative is something like "a Ruby
    on Rails / Symphony / Django application". That means logic for multiple
    functions, views and so on in the same code base. As I understand it the
    point of this series of posts is to propose an alternative, not to get into
    the nitty gritty details of implementation.

    Just two more things:

    \* The point about MVPs is true for the first product of a startup. Once you
    have a product, if you want to develop a new companion product or a new part
    of your product, you will need to re-use part of your production data and
    some features you already have (eg. authentication). Having decoupled things
    earlier will help you ship faster in that case.

    \* Another advantage of this approach is that you can be language agnostic.
    If you need a service to be written in C, well, just do it. If you have a
    monolithic application architecture you will have to write bindings, and it
    can quickly become a nightmare (especially to maintain). Moreover the people
    who are good at writing low-level code at your company may not be the same
    as those working on your front-end. They have different cultures, and making
    them work on what is technically the same code base is not necessarily the
    best idea you could have. Lastly, if you ever decide to change your main
    language (think Twitter, who went from Ruby to Java), it will be easier if
    you can do it service by service. Note that I am \*not\* saying that you
    should do this, but an "SOA" architecture makes it possible.

-   Oct 2 2012, 2:37 PM

    Jason Sirota responded:

    Despite the argument about semantics of what this type of thinking is
    called, it's a very real problem for companies that go from small to bigger.
    Figuring out how to iterate quickly at the user experience level while at
    the same time maintaining robust code at a the data level. It becomes more
    complicated when your company acquires other companies or expands into
    different business lines and needs to store and interrelate disparate data.
    Thinking about common data and common functions as a series of services that
    can be "mashed up" at the experience layer is what the web is all about!

    I would add one Problem to this list that we encountered as we moved in this
    direction and that is: troubleshooting. If you take this to the nth degree
    and your user experiences simply become a "mashup" of various data services,
    it gets a bit harder to pinpoint where a problem is actually occurring. It's
    especially challenging if you're teams are organized so that 1 team is
    responsible for the UX and you have teams responsible for each of the
    various business services. If the UX craps out (and that's usually where a
    problem is reported) then there's a tendency for the teams to start throwing
    the ball around before determining who is going to look into the problem and
    how to solve it. Obviously, this can be overcome by teams of awesomeness but
    it's something to think about when developing architecture this way.

-   Oct 3 2012, 1:07 AM
    Pierre Chapuis responded:
    @Jason You're right about troubleshooting. I have found that the best
    solution to mitigate this is to use trace IDs
    ([http://paul.querna.org/slides/logs-as-event-streams/\#13][]). If you can
    trivially identify a request across all your services the problem is almost
    solved - then it's only a matter of tooling.

  [Previous]: ../../../posts/2012/10/programming-and-motivation.html
  [Index]: ../../../index-2.html
  [Next]: ../../../posts/2012/09/service-oriented-side-effects.html
  []: ../../../image/2012/10/44817312-reaper.jpg
  [last post]: http://rdegges.com/service-oriented-side-effects
    "Service Oriented Side Effects"
  [tumblr]: https://www.tumblr.com/ "Tumblr"
  [Django]: https://www.djangoproject.com/ "Django"
  [Flask]: http://flask.pocoo.org/ "Flask"
  [flask-login]: http://packages.python.org/Flask-Login/ "Flask-Login"
  [Heroku]: http://www.heroku.com/ "Heroku"
  [http://lmgtfy.com/?q=SOA]: http://lmgtfy.com/?q=SOA
  [http://bill-poole.blogspot.se/2008/02/saas-service-vs-soa-service.html]: http://bill-poole.blogspot.se/2008/02/saas-service-vs-soa-service.html
  [http://paul.querna.org/slides/logs-as-event-streams/\#13]: http://paul.querna.org/slides/logs-as-event-streams/#13
