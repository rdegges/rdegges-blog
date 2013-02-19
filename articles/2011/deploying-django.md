# Randall Degges

## This is an archived post This is an archived post

[Previous][]   [Index][]   [Next][]

### Deploying Django

October 30 2011, 12:00 AM  by Randall Degges

![][]

Over the past two years, I've learned a *hell* of a lot about deploying Django
apps into production. And yes, I really do mean a **HELL** of a lot. While
Django is a great web framework, one of its primary weaknesses (in my opinion)
is a lack of strong deployment documentation and community standards. There has
been no easy way (until now, *kinda*) to deploy your apps into production in a
reliable, simple fashion. Period.

Sure, there are excellent *best practices* that a few **awesome** programmers
follow, but for the majority of users, the best practices are just too complex
and time consuming to setup and administer for typical site.

Real World Deployment

Let's pretend, for a moment, that you're an intermediate Django programmer. You
spend 8 hours a day working on a single project that is built purely in python /
Django. Your [primary main objective][] is to keep your users happy (fix bugs,
release new features, etc.). You're fairly familiar with the Django ecosystem:
you know which apps to look at for solving various problems, and you know where
to find information you don't already know.

Let's further assume that you're also an all-around-geek, and manage your own
[Rackspace][] or [Amazon][] servers which you use to host your Django app. You
boot your fresh new [Ubuntu][] cloud server, then install [Apache][] and
[mod\_wsgi][] to run your Django site. You then spin up a second server and
install [PostgreSQL][], which your site uses as its primary data store.

You think you're doing a good job--you wrote your app and got it online, spent
time learning how to use your cloud provider's APIs to boot up your new server,
and you managed to install, configure, and administrate your app. Furthermore,
you know where to look for problems: apache error logs, Django 500 and 404
emails, etc. In your mind, you're [awesome][].

When you think about the amount of effort you put into your work, you feel
highly accomplished. Not only did you do a good job getting everything running,
but you also spent ***years*** learning all the technologies that power your
site: HTML, CSS, Javascript, Python, Django, linux, etc.

... BUT ...

After three months of building your site, and 1 month of getting it running in
production, you finally have real users. As a matter of fact, you're getting
about 20 hits per second. While this may be awesome for business, your web
server's load is [14][], and your users' HTTP requests start timing out.

Since your Postgres server still has reasonable load, you decide to scale your
site horizontally by spinning up a second web server, and splitting HTTP
requests between the two. This way, you can serve twice as many users and keep
your site running smoothly.

You spin up a new server, install Apache / mod\_wsgi again, then copy your code
over. After a few hours researching HTTP load balancing, you decide to
use [HAproxy][] to proxy your requests. A week later, you have HAproxy running
on a new server, and can see HTTP requests hitting each web server.

Finally, your users can view your site again, whew. ***WRONG***.

As your site has grown, you've noticed that several pages have started eating
lots of CPU cycles. Upon further analysis, you see the problem: rendering the
dashboard page requires about 30 time intensive SELECT queries on your
PostgreSQL server. ***Damn***. To make matters worse, you can tell that if you
don't get this fixed right away, the database server will be destroyed by the
week's end.

To avoid repeating the large SQL queries, you decide to cache the page data for
an hour the first time a user visits the page. That way, repeat requests for
that page will not generate repeat SQL queries, thereby taking load off the
database. You provision a new server, and install [memcached][]. Two days later,
you've got a caching server working, and you've rewritten most of your Django
views to utilized memcached.

Now you can finally get back to coding! ***WRONG***.

Your pages are still loading slowly. You analyze your codebase, and realize the
new problem: once your cached page expires (at the end each hour), multiple
users hit the page simultaneously, generating numerous time intensive SQL
queries, effectively hammering your database server and causing HTTP timeouts.
Back to the drawing board.

**At this point, you're frustrated with progress.**

After a few days of research, you discover how to solve your caching problems:
[cache invalidation][] + [distributed message queues][]. Essentially, each time
a user adds new data to your site, you'll:

-   Dump a small message onto the queue that says "do some processing, then
    update the cache".
-   Your queue will get the message, and hold onto it.
-   Your worker processes will grab the oldest message from the queue, one at a
    time, and do what needs to be done (do processing, update the cache, etc.).

This way, your page cache is never stale, and you never run into the same
traffic stampede issues that you previously had.

The only problem now is that setting up a distributed message queue is a lot of
work. You need to setup a [RabbitMQ][], [celerybeat][], and one (or
more) [celeryd][] servers.

Two weeks and a lot of [energy drinks][] later, you've got it all set up and
working. You've rewritten most of your initial Django code to make good use of
celery and memcached.

But... then you realize the Postgres server is dying. ***FUCK!***

Even with all the fancy new scaling tools implemented, the database just can't
handle the constant load. Two weeks later you finish deploying multiple
PostgreSQL read slaves, and configure your Django database routing rules to use
the new PostgreSQL cluster effectively.

At this point, you're dying to get back to programming. You've spent almost two
months keeping your site running, but that's it. You haven't had time to work on
bug fixes, or feature requests for your site. To top it all off, you've had to
rapidly learn numerous new technologies just to keep things going, and you're
afraid that if anything fails, you'll be completely screwed.

In order to maintain your sanity, you spend a few more days researching server
management tools, and discover [puppet][]. Then...

![][1]

**Oh my fucking GOD!**

Yes, grasshopper! You now see it: you have only begun to discover the amount of
work that lays ahead. You've barely scratched the surface as to the tools,
methods, and skills necessary to manage and operate even the *simplest* of
production sites. Your work is cut out for you.

The Truth

If I've learned anything in the past two years working with production Django
deployments, it's that **deployment is fucking *hard***. Every real project I've
put into production over the past few years has involved hundreds (if not
thousands) of hours worth of infrastructure management. Even though I'd like to
think that my overall syadmin skills are slightly above that of your typical
programmer, I still spend an inordinate amount of time working on
infrastructure.

After speaking to loads of other programmers in the Django community, I
realize it isn't only me who has this problem, it's ***everyone***. While I
don't have any statistics to back this up, I'd estimate that typical Django
programmers spends at ***least*** half their time working on infrastructure
related tasks.

**The Django community is at an interesting place right now.**

We're sitting on verge of an enormous technology breakthrough. There are
currently sufficient tools, methods, and practices that allow Django developers
to build simple, robust, and scalable applications. However, the community as a
whole lacks direction and confidence in deployment.

What Django Needs

In order to fix the deployment problems in the Django community, several things
need to happen. If community members realize the importance of this problem, it
is more likely to get resolved quickly. By solving the deployment problem,
Django can set an unprecedented standard for web development.

First and foremost, all of the Django hosting companies around ([Heroku][],
[Gondor][], [epio][], [djangozoom][], etc.) need to really step up their game.
These companies are in the best position to fix the delployment problem. Their
livelihood depends on making developer's lives easier, and since they have
developers giving them money, they're in the best position to gather real world
usage data from numerous Django developers, and build elegant deployment
abstractions.

I'd personally like to see Heroku succeed as the dominant Django hosting
provider. In my opinion, they are tackling the deployment problem the best way.
Their model is simple: provide solid support for hosting your apps, and a [large
collection of add-ons][] that any developer can easily add and remove from their
site in an instant. Their addons allow you (as a developer) to completely ignore
90% of typical infrastructure work, focusing purely on application code.
Furthermore, since Heroku is running on Amazon's cloud, it's likely that their
prices will drop over time as their user base continues to grow and Amazon's
infrastructure becomes a commodity.

Second, we need strong deployment leadership from Django's core team. In my
opinion, [Jacob Kaplan-Moss][] is best posed to take on the role of
deployment-[BDFL][]. Jacob (one of the Django creators and core developers) is
an outspoken [devops][] advocate, frequently giving presentations on Django
deployment and infrastructure best practices.

With a strong leader willing to tackle the deployment problem, the official
[deployment documentation][] needs to be heavily updated. Currently, the
official documentation is woefully inadequate--covering only the most basic
deployment patterns. In order for the Django community to continue to grow and
succeed, the official deployment documentation needs to have a strong voice and
opinion--it needs to say:

-   Here's how to do caching right.
-   Here's how to properly setup asynchronous tasks.
-   Here's how to scale your database for certain usage patterns.
-   etc.

While this will surely upset a lot of people who believe in Python's
flexibility, Django **needs** to take an opinionated stance in regards to tools,
patterns, and practices. Without a stance, the community will stay as it is:
stuck in a confused state regarding deployment. If we (as a community) can agree
to take a stance on deployment: decide what to do, and what not to do--it will
revolutionize Django.

Third, the open source community needs to develop high-level abstractions for
common patterns. There are currently excellent tools available to Django
developers wanting to build production apps (celery, rabbitmq, memcached, redis,
etc.), but due to the complexity associated with setting them up and managing
them, they are not used nearly as often as they should be.

Developers need tools to fully automate and manage various pieces of Django
infrastructure. If it was as easy to deploy an auto-scaling distributed cache as
it was to start a Django app on the command line, Django developers everywhere
would be much happier.

I Have a Dream

In my dream, all Django developers (from the newest to the most experienced) can
rapidly build their web apps, knowing that production deployment is at most 10
minutes away from completion.

Imagine a world where (as a developer) you can build an app, and have it running
live in a best-practices, scalable, and sustainable way in no more than 10
minutes. The possible benefits are tremendous:

-   Production Django sites will typically be more reliable than their
    counterparts.
-   Django developers worldwide will have confidence in not only writing, but
    also deploying their applications to the world.
-   New developers will be greatly attracted to Django as a framework, due to
    its immensely simplified and powerful deployment options.
-   Django developers will be able to spend more time, each day, writing python
    and Django code, instead of focusing their efforts on infrastructure
    management.

**We're at an awesome time in Django history.**

Each of us has the ability to greatly effect the future of the framework we all
love so much. We have the ability to influence the newest generation of sites
that power the internet. We need to collectively decide to show our fellow
developers the light, and remove the confusion that exists around deployment.

**EDIT**: Based on the feeback I received from this article, I decided to write
a series on deploying Django. If you're interested, you can read the first part
[here][].

**UPDATE**: If you liked this post, you may want to check out my new book on
Heroku, [The Heroku Hacker's Guide][]. It covers ideal ways to deploy your web
applications on Heroku to reduce the pain.

#### Tags

programming, python, django

#### 14155 views and 13 responses

-   Oct 31 2011, 5:20 PM

    NoName responded:

    You should try experimenting with NGINX. I am running it with php via
    fastcgi and it produces a third of the load at idle as Apache+mod\_php(not
    to mention all the fancy tricks you can do with NGINX like pre-serving
    static files).

    Also, Try using round robin dns to load balance. Move the capacity problem
    to the dns server(hopefully not yours though).

-   Nov 1 2011, 10:09 AM

    Lindsay responded:

    Hi Randall! I'm the Community Manager at DZone.com and I'd like to discuss
    our Most Valuable Bloggers program with you. It is a free program that
    promotes quality developer blogs such as this one. E-mail me at lgordon {at}
    dzone{dot}com if you are interested! I hope to hear from you.

    I've really enjoyed the posts I have read on your blog. I am not a developer
    but I found that a lot of your writing has a lot of personality and a great
    sense of humor. I especially related to the caffeine post, I've been off it
    a few months and it is still a big adjustment!

    We'd love to feature you on our site!

-   Nov 1 2011, 10:46 AM

    Randall Degges responded:

    @NoName word. I don't actually use the setup described above, it was more of
    a fictional narrative, to hopefully explain how many new developers might
    struggle through the process of running a typical Django app.

    I actually use gunicorn + nginx + haproxy in production for most of my
    non-heroku sites.

-   Nov 1 2011, 2:03 PM

    esoesotracosa (Twitter) responded:

    Yep, deployment with django is hard. I am just a newcomer in this lands and
    I'm only starting to grasp the astonishing depth of it:

    pip for packages and dependencies management\
    virtualenv for environment isolation\
    mercurial and bitbucket for smart storing source code\
    fabric for automatic deployments\
    puppet as system requirements manager\
    ngingx as http server and proxy\
    gunicorn as application server for django\
    postgresql for persistent data\
    memcached for stupidly fast memory caching\
    supervisord for process control\
    nagios for monitoring everything\
    ...\
    [more tools here with the associated problems they solve]\
    ...

    And only after learning all that you are able to do a simple one-server
    deployment. Then you proceed to engineer the shit out of django (HUGE field)
    and the front-end aspect of it (html5boilerplate, jquery, jquerymobile,
    sass...) in order to solve the problem you or a client had and originated
    all this mess.

    Deployment is fun, I enjoy learning this stuff, but it's not a task for
    developers. We (the community) could upgrade the django documentation to
    include the basic aspects of a real world django deployment. There are
    articles here and there but they tend to be short, too much specific or
    simply outdated.

-   Nov 5 2011, 2:26 PM
    Rich Jones responded:
    I love you Randall.
-   Nov 5 2011, 7:30 PM
    issackelly responded:
    Randall, Why don't you do it? This is open source, it's a small community.
    Volunteering Jacob as the answer is a cop-out. You can do this. you can
    write the guide on how to be 10k requests/sec on heroku, and make the
    documentation, if it's great, it will become canonical for guys like us. Do
    it. Writing this sort of post doesn't \_help\_. Everybody knows this is a
    pain. starting a business around it helps, documenting it helps.
-   Nov 5 2011, 11:09 PM

    Randall Degges responded:

    @isaackelly I haven't seen much written about the topic, which is why I
    wanted to write about it here. In all honesty though, I never really
    considered myself a good enough programmer (or writer) to contribute
    anything.

    Maybe it's time for me to dive in! Thanks for the comment.

-   Nov 5 2011, 11:15 PM
    issackelly responded:
    Your code is useful (I use it on ServeLocally.org/Weserveseo.org) and your
    writing is tolerable (maybe less grumping/swearing in 'documentation' type
    writing) You're fine! I agree that the first step is to identify the
    problem, and you've done that. There's a lot of space for something like the
    High Scalability blog specific to Django, and the way you become better/an
    expert in an area is to explore it, to write about it, and to test its
    limits. Good Luck!
-   Nov 9 2011, 1:47 PM

    Lindsay responded:

    Hi Randall! I'm the Community Manager at DZone.com and I'd like to discuss
    our Most Valuable Bloggers program with you. It is a free program that
    promotes quality developer blogs such as this one. E-mail me at lgordon {at}
    dzone{dot}com if you are interested!

    I've really enjoyed the posts I have read on your blog. I am not a developer
    but I found that a lot of your writing has a lot of personality and a great
    sense of humor. I especially related to the caffeine post, I've been off it
    a few months and it is still a big adjustment!

    I promise this is not spam! We'd love to feature you on our site, and I hope
    to hear from you.

-   Nov 10 2011, 9:01 PM

    Roger Barnes responded:

    This is a really good description of the deployment problem. I'm suspect the
    same applies to any web framework, but the solutions do need to be
    Django/Python specific. I do see "Django" (whomever that is deemed to be)
    playing a bit more of a role, but the Django \_community\_ (all of us, no
    matter what our involvement) need to be the major contributors with more
    blog posts, discussions and hopefully some occasional distillation of
    current recommended practices.

    My efforts in that regard are a long way off as I'm still near the start of
    the deployment journey (single server, apache + mod\_wsgi), but I humbly
    submit a recent talk I gave as a contribution to the pool of knowledge...\
    [http://www.slideshare.net/mindsocket/intro-to-pinax-kickstarting-your-django-...][]\
    Notes about development and deployment processes start slide 29

-   Nov 20 2011, 8:16 PM

    natea (Twitter) responded:

    Great post Randall! While there are a lot of emerging Fabric-powered tools
    and such, the Django deployment story is still a mess. With DjangoZoom,
    we're trying to make it a little less painful by providing an easy way to
    deploy production Django apps without needing to think about all this stuff.

    I'm very interested to hear your thoughts on what kinds of add-ons you'd
    like to see offered. The obvious ones are memcache, Celery/RabbitMQ, email
    transport (Postmark, Sendgrid), and logging services (Loggly, Hoptoad).
    Maybe Solr too. At least those are the ones people have been asking for.

    What would your dream deployment service look like?

-   Jan 8 2012, 9:59 AM
    Suvash responded:
    nice post Randall ! totally agree with your dream about deployment being
    only 10 mins away.\
    As I've just begun with django community, i wouldn't say i know a lot, but
    the documentation on a lot of things aren't quite good.\
    Now I'm off to your other posts !
-   Feb 18 2012, 6:52 PM

    claymcclure (Twitter) responded:

    It sounds to me as if you're conflating deployment and scaling. I
    wholeheartedly agree that deploying Django is harder than it ought to be,
    and often harder than deploying apps written in other languages, but the
    scaling nightmares you describe are not unique to Django or Python.

    Entire conferences (see O'Reilly's Velocity Conference) are devoted to
    scaling web applications, and while there are some common tactics, there are
    no universal solutions. Techniques that emerge as best practices for some
    apps won't work at all for other apps.

    So while I agree that Django deployment can—and should—be much easier, I am
    not convinced that scaling will ever be trivial. Nor am I sure that it
    should be, since sometimes the ability to scale an app is a competitive
    advantage. We have Facebook today and not Friendster or SixDegrees largely
    because the former scaled while the latter did not.

    But I suppose I'm biased, having worked in operations at Ning and Twitter.
    Scaling web apps (whether PHP, Java, Ruby, or Python) is half the fun :)

  [Previous]: ../../../posts/2011/12/devops-django-part-2-the-pain-of-deployment.html
  [Index]: ../../../index-4.html
  [Next]: ../../../posts/2011/09/my-use-and-abuse-of-caffine.html
  []: ../../../image/2011/10/33628453-cookie-monster-wtf-is-this.jpg
  [primary main objective]: http://img88.imageshack.us/img88/2310/200311chinpokomongi2.jpg
    "primary main objective"
  [Rackspace]: http://www.rackspace.com/ "rackspace"
  [Amazon]: http://aws.amazon.com/ "amazon"
  [Ubuntu]: http://www.ubuntu.com/ "ubuntu"
  [Apache]: http://www.apache.org/ "apache"
  [mod\_wsgi]: http://code.google.com/p/modwsgi/ "mod_wsgi"
  [PostgreSQL]: http://www.postgresql.org/ "postgres"
  [awesome]: http://www.awesomeness.net/colbert_arms_folded.jpg
  [14]: https://si0.twimg.com/profile_images/135126819/404-fffuuu_bigger.png
    "FFFFFUUUUUU"
  [HAproxy]: http://haproxy.1wt.eu/ "haproxy"
  [memcached]: http://memcached.org/ "memcached"
  [cache invalidation]: http://en.wikipedia.org/wiki/Cache_invalidation
    "cache invalidation"
  [distributed message queues]: http://activemq.apache.org/how-do-distributed-queues-work.html
    "how distributed queues work"
  [RabbitMQ]: http://www.rabbitmq.com/ "rabbitmq"
  [celerybeat]: http://ask.github.com/celery/userguide/periodic-tasks.html
    "celerybeat"
  [celeryd]: http://celeryproject.org/ "celery"
  [energy drinks]: http://www.amazon.com/gp/product/B000NGNEKY/ref=as_li_ss_tl?ie=UTF8&tag=projectb14ck-20&linkCode=as2&camp=217145&creative=399369&creativeASIN=B000NGNEKY
    "rockstar no carb energy drinks"
  [puppet]: http://puppetlabs.com/ "puppet"
  [1]: ../../../image/2011/10/33670296-Oh-My-God!!!.jpeg
  [Heroku]: http://www.heroku.com/ "Heroku"
  [Gondor]: https://gondor.io/ "Gondor"
  [epio]: https://www.ep.io/ "epio"
  [djangozoom]: http://djangozoom.com/ "djangozoom"
  [large collection of add-ons]: http://addons.heroku.com/ "heroku addons"
  [Jacob Kaplan-Moss]: http://jacobian.org/ "Jacob Kaplan-Moss"
  [BDFL]: http://en.wikipedia.org/wiki/Benevolent_Dictator_For_Life "BDFL"
  [devops]: http://en.wikipedia.org/wiki/DevOps "devops"
  [deployment documentation]: https://docs.djangoproject.com/en/1.3/howto/deployment/
    "Django deployment documentation"
  [here]: http://rdegges.com/devops-django-part-1-goals
    "DevOps Django - Part 1 - Goals"
  [The Heroku Hacker's Guide]: http://www.theherokuhackersguide.com/
    "The Heroku Hacker's Guide"
  [http://www.slideshare.net/mindsocket/intro-to-pinax-kickstarting-your-django-...]:
    http://www.slideshare.net/mindsocket/intro-to-pinax-kickstarting-your-django-apps
