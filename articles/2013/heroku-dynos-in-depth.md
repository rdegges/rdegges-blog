# Randall Degges

## This is an archived post This is an archived post

[Index][]   [Next][]

### Heroku Dynos (in Depth)

February 10 2013, 7:06 PM  by Randall Degges

![][]

**NOTE**: This article is all about [Heroku][]. If you've never heard of them,
you may want to skip this one \>:)

So, I'd like to talk about dynos with you (not *dinosaurs*, although, I do love
talking about dinosaurs). Ok, maybe I'll talk about dinosaurs a little bit, but
mainly about Heroku dynos: containers that run user-defined processes on
Heroku's cloud platform.

Dynos are really the 'core' of Heroku's platform. Dynos are what run your web
processes, your one-off tasks, etc. If you're building a Rails website, for
instance, a dyno would run your web process as well as any background tasks
you've defined with rake: cleaning up user sessions, doing stuff every hour,
whatever. Dynos also run worker processes (stuff like [resque][], etc.). I'm
sure you get the idea.

So if dynos are just process containers (essentially), why are they cool? Well,
other than the fact that their name reminds me of little dinosaurs (see picture
below), there are a lot of reasons.

![][1]

-   Dynos (like the rest of Heroku's platform) run on top of [Amazon Web
    Services][], one of the largest cloud providers in the world.
-   Dynos come with unlimited bandwidth (this can end up costing you a fortune
    with other providers).
-   Dynos run in their own isolated execution environment. This means no shared
    files, users, etc. Every time a dyno is created it is identical to the rest.
-   Dynos have 512MB of RAM reserved for them, enough to do \*almost\* anything
    you need to do. (Looking to render videos and stuff? Best to do that
    separately.)
-   Dynos can be instantly provisioned or removed, allowing you to easily 'scale
    up' your application's infrastructure.
-   Dynos are billed by the second, meaning you can easily handle 'burst'
    traffic and not pay a fortune to do so.
-   You get **750** hours of dyno time free each month, per Heroku application
    (a 30 day month is about 720 hours, for reference). **PER APPLICATION!**
    This means that if you've got 10 apps running on Heroku, you get 7,500 dyno
    hours free, each month!
-   If a dyno crashes for some reason (maybe the underlying Amazon server
    broke), it will be automatically moved to another server transparently,
    ensuring your application stays running (no maintenance needed!).
-   Web dynos (any dynos powering your website: Rails, Django, Node, whatever)
    have load balancing taken care of out of the box (Heroku's routing mesh
    automatically load balances incoming HTTP requests to however many web dynos
    you have active).

Not bad, right? In terms of cost alone, Heroku can cut your hosting bill
significantly in the form of free bandwidth and free dyno hours.

The real kicker, however, (at least in my opinion) is the automatic load
balancing Heroku provides. Heroku's routing mesh is incredibly awesome:

-   It randomly distributes incoming HTTP requests to your application across
    all available dynos. This means that if you have a single active dyno all
    requests will hit it. If you have 10 dynos, each of those 10 will get hit by
    incoming requests. This means you can easily scale your application without
    worrying about the routing logic.
-   It provides you with both HTTP **and** HTTPS load balancing. Each Heroku
    application automatically supports both HTTP and HTTPS out of the box,
    meaning you can use SSL easily with the domain name your application is
    assigned: *appname.herokuapp.com*.
-   The routing mesh gives your application 30 seconds to respond to incoming
    requests. If it takes longer the request is killed with a 503. This is GOOD
    because it forces you to write decent code that takes user experience into
    account. NOTE: If a request takes longer than 30 seconds to complete, while
    the Heroku routing mesh will return a 503, your application server will
    still continue to process the request--this way, valuable user information
    isn't lost.

And, in case you missed it above, since dynos are automatically maintained by
Heroku's Dyno Manifold, you never have to worry about bad things happening. Even
if your application crashes, an Amazon server crashes, an IT guy trips over a
wire and shuts down 100 of your dynos--you don't need to worry because Heroku
will automatically spin up all your dynos (instantly) on other Amazon cloud
servers.

The Heroku Dyno Manifold's restorative powers, paired with the Heroku Routing
Mesh means you get the best of everything: no maintenance web hosting, automatic
load balancing, and happy users.

As a sidenote--if you're at all interested in this stuff, you may enjoy my book:
[The Heroku Hacker's Guide][]. I'm currently working on the second edition,
which I promise you'll love \>:)

#### Tags

programming, heroku

#### 808 views and 7 responses

-   Feb 10 2013, 7:09 PM
    Matthew Conway liked this post.
-   Feb 10 2013, 8:24 PM
    Ben Roberts responded:
    Downsides? You didn't mention the downside of dynos which really comes down
    to cost. Yes, 1 free dyno is awesome. However if you've built something
    significant, the right way, you likely need worker processes. For a
    heavily-trafficked site with significant data processing involved, we need 2
    web dynos, before long. (I've never been able to figure out how to make
    need-based scaling work--usually by the time we know we're experiencing a
    spike and need to scale momentarily, multiple request have already been
    dropped.) And the first-tier production database is \$50/month. Add in
    rabbitmq, memcachier, loggly, etc. … and the monthly fees go up. We manage a
    cms-like platform where we basically utilize the same platform for many
    clients. For this to work, we ideally need at least a paid worker dyno for
    each client in addition to the free web dyno. \$34.50/mo/dyno = \$414.00 a
    year. For a basic, production database, add \$600 a year. For a client with
    lots of traffic, it could e another \$414.00/yr with the addition of a
    second web dyno. If we were charging multiple thousands per year per client,
    this would be fine, but we often aren't. We looked at programattically
    turning off and on dynos every time we need a worker process, which isn't a
    trivial task for something like periodic asynchronous cache-refreshing, even
    though heroku provides an API to scale dynos. For one, a non-running process
    can't turn itself on, so either a web proccess needs to turn an unrelated
    worker on (weird, on multiple levels), or we need a third task or scheduler
    to turn these on and off. Anyhow, heroku's good enough and saves enough time
    that we've decided at the moment against re-tooling our platform to go back
    to running on a VPS (yuck) where I can run multiple processes for free, but
    the cost is so prohibitive for our type of business that in order to address
    it, we've taken the time to build a truly dynamic multi-tenant platform on
    top of django (which contrib. sites isn't) and are testing that out.
    Hopefully that will make running a worker and extra web processes more cost
    effective. Regardless, as we grow and become better staffed, I think the
    dyno pricing model may push us towards utilizing our own hardware. I realize
    most aren't using heroku for our type of business, and its certainly not a
    great fit for it thus far, pricing-wise, but its such great technology that
    its hard to complain and hopefully we'll figure out how to make it work
    better for us with this new multi-tenant stuff.
-   Feb 10 2013, 8:45 PM

    Randall Degges responded:

    Ben,

    It definitely sounds like what you're trying to do is more about reducing
    cost to serve a lot of clients simultaneously vs. scaling a web application.
    If you need multiple dynos, there's obviously no way to get around paying
    for them, and the same goes for the database.

    If you consider everything you're getting though, it's an tremendous deal:

    - No maintenance.\
    - Use the Heroku scheduler to spin up / spin down dynos when you need to
    (have it run a check every 10 minutes), then actively scale your stuff.
    There's also auto-scale options available in the addon market, and stuff
    like hire-fire app.\
    - No servers to manage, no ops guys needed.\
    - If you're using Heroku Postgres, you also get backups, read slaves,
    duplicate masters, etc.\
    - Easy upgrading of addons, and increasing competition in the addon
    marketplace bringing down prices.

    I totally understand your situation, but for 99.99% of people, using Heroku
    is probably way cheaper than doing things yourself.

-   Feb 11 2013, 5:09 AM

    Ben Roberts responded:

    Rnadall,

    its definitely cheaper than doing some things ourselves. I just wanted to
    point out that you definitely pay a premium with the dyno + addons model for
    some types of apps, and the pricing model might not be a great fit for
    everyone. I'm a huge heroku advocate, but it seemed your original post
    lacked any "cons" discussion so I thought I'd add my two cents :) As I
    mentioned, we're really looking at building our apps in a way that works
    even better with heroku so that we don't get priced out of a great platform.

    Regards,

    Ben

-   Feb 11 2013, 4:07 PM

    Rakesh responded:

    Randall:

    Can't wait for the second edition..

-   Feb 14 2013, 7:08 AM

    Koichi Hirano responded:

    The heroku routing mesh is very disgusting. You said:

    "It randomly distributes incoming HTTP requests to your application across
    all available dynos."

    There is no intelligence. So even if a dyno is heavily busier than other
    dynos, the routing mesh still forward requests to the dyno, and requests
    being forwarded to the dyno would end up with 503.

    It may be OK for apps built upon node.js, though.

-   Feb 14 2013, 1:45 PM

    pg responded:

    Hi Randall,\
    Here is an interesting post from Rap Genius on Heroku and the discussions
    that follow. I am curious of your take on it:
    [http://rapgenius.com/James-somers-herokus-ugly-secret-lyrics][]

    -pg

  [Index]: ../../../index.html
  [Next]: ../../../posts/2013/02/productivity-and-calmness.html
  []: ../../../image/2013/02/47195011-t-rex.jpg
  [Heroku]: http://www.heroku.com/ "Heroku"
  [resque]: https://github.com/defunkt/resque
  [1]: ../../../image/2013/02/47195282-dinosaur.jpg
  [Amazon Web Services]: http://aws.amazon.com/ "Amazon Web Services"
  [The Heroku Hacker's Guide]: http://www.theherokuhackersguide.com/
    "The Heroku Hacker's Guide"
  [http://rapgenius.com/James-somers-herokus-ugly-secret-lyrics]: http://rapgenius.com/James-somers-herokus-ugly-secret-lyrics
