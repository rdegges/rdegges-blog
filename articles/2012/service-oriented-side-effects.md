# Randall Degges

## This is an archived post This is an archived post

[Previous][]   [Index][]   [Next][]

### Service Oriented Side Effects

September 28 2012, 2:55 PM  by Randall Degges

![][]

There are two primary ways to write web applications these days:

-   By building a single, monolithic web application (where all your code is in
    a single project and runs on a single domain).
-   By building lots of small independent web services, each one with their own
    codebase and URL endpoint. (This methodology is known as [Service Oriented
    Architecture][].)

There are benefits and drawbacks to each approach.

What this article is about, however, are the side effects of writing your
applications as web services as opposed to monolithic web applications. This
article assumes you have some basic knowledge of writing web applications.

To get started, lets discuss the basics: the monolithic pattern and the service
pattern.

The Monolithic Pattern

If I had to guess, I'd say that close to 99% of all active web applications are
written as monolithic web apps. Why? I think that with so little practical
information around on building service oriented web applications, it is only
natural to build monolithic applications instead.

For starters, it's a lot easier (at least in the beginning) to put all your web
app code into a single project. Over time, as your project grows and more and
more complexity is added, it's a lot easier to simply refactor the old stuff
than it is to rip your application apart into multiple independent services.

With so many monolithic applications out there, certain patterns have emerged
over the years that make building and scaling monolithic web applications
simpler--primarily, [message queues][].

When you're working on a large monolithic web application, and need to:

-   Speed up your response time,
-   Render things asynchronously, or
-   Perform time intensive operations on separate hardware...

The defacto way to do so is by relying on the message queueing pattern. What
this allows you to do is dump tasks into a message queue (something
like [RabbitMQ][], [Amazon SQS][], or [Redis][]), which will then be read
(consumed) by one or more worker servers which read the task, process it, and
dump the resulting values into some place that your web application can retrieve
it.

Using message queues allows you to fire-and-forget slow operations, letting them
happen naturally behind the scenes without delaying your web response time.

The Service Pattern

While service oriented architecture is certainly a hot topic in tech circles, it
has still not gained widespread adoption, primarily due to the fact that it can
be complex to implement, and has a steeper learning curve for new developers.

Service oriented web applications differ from monolithic web applications in
serveral main ways:

-   They are composed of multiple, small, independent services (think: www, api,
    accounts, portal).
-   Each of these small services talks via HTTP to the other services.
-   Each of these small services has less code in it (since they do less
    things), making them generally easier to maintain over long periods of time.
-   Each of these small services has their own resources: web servers,
    databases, etc.

As you can probably see, building service oriented web applications essentially
gives you many small moving parts as opposed to a single large part.

Effect 1: Maintainability

The first side effect you get by writing service oriented web applications is
simple: better maintainability for your web application.

By having multiple small codebases that are each responsible for a very small
amount of logic, you'll typically have an easier time maintaining your code in
the long term--since complexity is always kept to a minimum and the code is easy
to navigate.

Let's say you've written an 'accounts' web service, which is solely responsible
for creating, updating, and removing user accounts. This means that if you need
to update some account logic, you know exactly where it is. There is no
searching through thousands of lines of view code, database wrappers, or
anything else--as this logic exists in only a single small place in your
application.

Compare this with maintaining monolithic applications with lots of complexity in
a single place, it becomes harder to make code changes without side effects,
more difficult for new developers to dive in (where do I get started?), and
tricky to refactor without breaking things.

Effect 2: Scalability

One of my favorite effects of writing service oriented web applications is the
passive effect it has on scalability.

By having multiple small web services, each with their own resources (databases,
message queues, web servers), this gives you the ultimate flexibility in scaling
your web application. Let's say your application consists of the following
services:

-   myapp-api, your public facing web API for developers.
-   myapp-www, your public facing website.
-   myapp-accounts, your internal API for managing user accounts.

Let's say that each time a developer makes a request to your myapp-api project,
you first have to authenticate the user against your myapp-accounts project
(real-world scenario). Let's also say that each time users log into their
account on myapp-www, they ALSO must authenticate against your myapp-accounts
project.

Since both your myapp-api and myapp-www projects rely on myapp-accounts for
authentication data, it is likely that your myapp-accounts project will be
getting a lot more usage than either your myapp-api or myapp-www projects.

Luckily, since you've built your web application as a composition of independent
services, you now have several options to scale your myapp-accounts project:

-   You can add more web servers.
-   You can add a larger database.
-   You can scale your database queries out (through read slaves).
-   You can add or refactor your logic to make better use of message queues for
    time intensive tasks.

And you can do this all without affecting either of your other two services:
myapp-api or myapp-www!

Compare this with scaling monolithic web applications: if you need to increase
capacity for users authenticating against your database, you must scale your
entire application--you can't selectively choose which parts to scale, and which
parts to leave alone (note: this isn't *entirely* true, but I'm saying it
anyway).

Effect 3: Simplicity

Another effect of writing service oriented web applications is that you
generally have a much simpler backend system.

As I mentioned at the start of this article, one of the primary ways to scale
monolithic web applications is by heavily relying on message queues to process
time intensive tasks. When you begin writing services as opposed to monolithic
apps, you can actually rely much less on messages queues than you'd think,
eliminating technical complexity and architectural burden.

Let's say you have a monolithic application that fires off a time intensive task
(via a message queue) which subtracts money from a user's bank account. The
reason you fire this off as a task is because this function must perform a lot
of sanity checks before removing the user's money, and you want to ensure this
happens safely outside of your user's request-response cycle.

What if one of your internal services handled all banking transactions for
users? Instead of:

-   Running a message queue,
-   Running a backup message queue (in case of failure), and
-   Running multiple worker servers...

You could simply fire off an HTTP POST request to your internal
myapp-transactions project and let it handle things in real time. The reason you
could do this is because, knowing your transactions application requires fast
servers, you could:

-   Ensure your myapp-transactions service runs on very fast web servers,
-   Can quickly process and handle banking transactions,
-   Keep your code simple and independent of external dependencies.

Instead of relying on a message queue, you could: rip it out of your
architecture all together, have a small (easily maintainable) codebase for your
transactions, expose a simple HTTP service that your other applications can talk
with, and scale your transactions service independent of all your other services
(this way, if you suddendly start doing a lot of transactions, you only need to
focus on scaling a single small codebase).

Wrap Up

In my experience, building service oriented web applications generally leads to
simpler, faster, and more scalable web applications. Without realizing it, you
can:

-   Make maintaining and expanding your codebase easier.
-   Lower the bar for new developers.
-   Make scaling your application a simple process.
-   Reduce architectural overhead and complexity.

Also: if you're at all interested in building service oriented web applications,
you may want to check out my book: [The Heroku Hacker's Guide][], it teaches you
how to use [Heroku's platform][] to build fast, small, independent web services.

#### Tags

programming, webapplications

#### 5804 views and 11 responses

-   Sep 28 2012, 6:16 PM

    Tad responded:

    You do great a job at selling SOA. I don't agree that by suddenly shoving a
    mq in the mix you make it better for green engineers. Understanding how
    components deliver on SLAs and consistency & availability of message
    transports are easily misunderstood and abused.

    Also, distributed SOA development in a large company can be a total
    nightmare because of lack of api contracts, lack of debugging tools, rolling
    outages, and most notable integration testing. There's a solution the later
    but it requires that you at least triple your production fleet for most use
    cases. Don't forget you increase deployment complexity dramatically when
    teams own and manage their own service.

    My money's on something like a process oriented architecture, see Erlang,
    riak\_core. Truth be told I only have a small site & service deployed
    currently.

-   Sep 28 2012, 8:38 PM
    Henry liked this post.
-   Sep 28 2012, 10:52 PM

    Bob responded:

    I'm curious why you have redis as a mq?

    I thought it was just a key-store?

    Thx\
    Bob

-   Sep 29 2012, 12:50 AM
    NK? responded:
    I'm new to webservice programming but what you describe seems what netkernel
    from 1060 provides [www.1060.org][]
-   Sep 29 2012, 8:46 AM
    Matt responded:
    SOA and queues are not mutually exclusive, and SOA is by no means tied to
    HTTP. You can have small, flexible, independent services connected via
    queues (or whatever). Not covered here is the problem of maintenance and/or
    downtime. HTTP is not terribly forgiving in that respect, which is the main
    benefit queues add (the ability to queue up work during downtime). In your
    bank example what do you do in the case of an HTTP 500? Trust the money was
    taken out, or was not? HTTP is not transactional and building like it is
    want change that.
-   Sep 29 2012, 9:11 AM

    xternal (Twitter) responded:

    @Bob, I'm not the author but think I know the answer.

    Redis has a few mechanisms that make it possible for using as a queue.
    Usually it's the list in conjunction with the brpoplpush command
    ([http://redis.io/commands/brpoplpush)][] or using it's pubsub
    ([http://redis.io/commands\#pubsub][]). Either can provide the basics of a
    queue, though it's up to you how to handle failures, ack, etc.

    I think Redis is popular as queue for a number of reasons, including: it's
    easy and light to setup, the server is reliable, and it's often easier to
    understand than queuing systems like AMQP. A modest programmer could sit
    down and have a simple queue running off Redis in minutes, but taking the
    time to learn AMQP exchanges, routing, keys, etc, not so much. I'm not
    saying it's better, it's just faster to get running on. This isn't always
    the best reason for a choice.

-   Sep 29 2012, 9:13 AM
    xternal (Twitter) responded:
    Also, I should add, a lot of times it's because redis is already available.
    If you've already got it up as a KV, sometimes it's, why install another
    service for your queuing when you've already got one.
-   Sep 29 2012, 12:32 PM
    languagehacker (Twitter) responded:
    This is a great post, that really clearly states a lot of the fundamental
    advantages of handling cross-cutting concerns using SOA. Maybe it's a trait
    of the term "side effects", but I was really hoping to hear more about the
    caveats of working with such an architecture. It seems like lot of people
    are aware of the scalability and simplicity advantages of creating a true
    service-oriented architecture, but maybe not so much about the possible
    trade-offs you may need to make in order to accomplish it. Do you have any
    thoughts on this?
-   Sep 29 2012, 2:05 PM

    bob responded:

    Thanks Xternal for the info. I'm using redis with the very cool iodocs
    application and wasn't aware of the extra features.

    bob

-   Sep 30 2012, 4:27 AM
    kaderka (Twitter) responded:
    hi, nice article, but i would be more interested in reading about
    disadvantages of SOA and how to tackle them.
-   Oct 4 2012, 12:38 AM
    kevin responded:
    Thanks there for such a real nice article.. \
    [http://www.godotmedia.com/][]

  [Previous]: ../../../posts/2012/10/service-oriented-problems.html
  [Index]: ../../../index-2.html
  [Next]: ../../../posts/2012/09/the-simplest-question.html
  []: ../../../image/2012/09/44752613-robot.jpg
  [Service Oriented Architecture]: http://en.wikipedia.org/wiki/Service-oriented_architecture
    "Service Oriented Architecture"
  [message queues]: http://en.wikipedia.org/wiki/Message_queue "Message Queues"
  [RabbitMQ]: http://www.rabbitmq.com/ "RabbitMQ"
  [Amazon SQS]: http://aws.amazon.com/sqs/ "Amazon SQS"
  [Redis]: http://redis.io/ "Redis"
  [The Heroku Hacker's Guide]: http://www.theherokuhackersguide.com/
    "The Heroku Hacker's Guide"
  [Heroku's platform]: http://www.heroku.com/ "Heroku"
  [www.1060.org]: http://www.1060.org
  [http://redis.io/commands/brpoplpush)]: http://redis.io/commands/brpoplpush)
  [http://redis.io/commands\#pubsub]: http://redis.io/commands#pubsub
  [http://www.godotmedia.com/]: http://www.godotmedia.com/
