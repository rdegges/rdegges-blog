# Randall Degges

## This is an archived post This is an archived post

[Previous][]   [Index][]   [Next][]

### Heroku Isn't for Idiots

June 3 2012, 11:39 AM  by Randall Degges

**WARNING**: This is a *bit* of a rant. I'm going to assume you have a basic
understanding of [Heroku][], and web application architecture.

![][]

Heroku isn't for idiots. There--I said it. I'm already starting to feel better!

I'm usually not the type of person to get involved in internet debate (because I
typically avoid politics as much as possible), but yesterday I read an
incredibly fradulent article which really made my blood boil: [The Cloud is Not
For You][].

Before I start to rip holes in David's article, I'd just like to say--[David][]
is a good programmer. In fact--he's a much better programmer than myself. If
you're involved in the python web programming world, you've likely used his
software, and seen his presentations.

While I've never met the guy, he seems like a decent dude. So if you take
anything away from this rant, let it be purely technical \>:)

Also: I should probably mention that I'm in no way affiliated with Heroku. Like
David, I know several Heroku employees, but that's the extent of my
relationship.

Ok then! Let's talk about Heroku!

 

Heroku is Just Unix

At its core, Heroku is just a simple unix platform; specifically, Ubuntu 10.04
LTS.

![][1]

The entire Heroku platform is really nothing more than small Ubuntu virtual
server instances that can be spun up and down on demand. Each instance (Heroku
calls them [dynos][]), has:

-   512MB of RAM, 1GB of swap. Total = 1.5GB RAM.
-   4 CPU cores (Intel Xeon X5550 @ 2.67GHz).
-   Isolated execution. Anything you store on your dyno will be isolated from
    all other dynos.
-   A [chroot jail][] environment. This means that you are completely locked
    down to one directory tree, with no write access to system files.
-   Per-second billing. Dynos cost 5 cents per hour. You get one dyno for free
    each month (per app). If you use a dyno for 2 seconds, you'll
    pay 0.16666666666666666 cents. If you use a dyno for an entire month solid,
    you'll pay 36\$ (assuming a month has 30 days).

**NOTE**: If you want to calculate your costs for running your application on
Heroku, they have a really nice [cost calculator][] you can use.

So what does this mean? Well, it basically means that Heroku can run pretty much
any Ubuntu compatible software you can think of. It also means that there's
really nothing \*magical\* going on here--it's just unix.

 

Heroku is Centered Around Apps

Unlike most other cloud providers, Heroku isn't in the VPS business. Their goal
isn't to sell you virtual Ubuntu servers that you configure yourself. Their real
goal is to handle that stuff for you, and let you focus on building
applications.

![][2]

The main idea is that:

1.  An application is a piece of software that talks over HTTP (websites, APIs,
    whatever).
2.  An application exists to serve a single purpose.
3.  An application has a single codebase, version controlled with [Git][].
4.  An application should consist only of application code--not infrastructure
    setup, or anything else.
5.  Applications should be easy to scale horizontally. Need to support more
    incoming requests? Just add another application server to your pool, and
    Heroku will handle the load balancing.
6.  Applications should be fault tolerant. Did your application or server crash?
    It will be instantly running on another virtual server so that you
    experience no downtime.
7.  Applications should be easy to rollback. Need to rollback to a previous
    release? You can do it instantly.
8.  Applications should use environment variables for configuration. You should
    never hardcode your configuration.

Does that sound good to you? Of course it does! Heroku encourages good
application design and architecture from the ground up, by making certain
assumptions about your project from the start.

If you're writing an application--why should you have to:

-   Run redundant servers incase something breaks?
-   Spend time configuring every piece of your infrastructure?
-   Worry about hardware or host issues?
-   Build your own deployment systems so that you can instantly deploy your code
    across hundreds of virtual servers at once?
-   Build your own rollback system to instantly restore your application to a
    previous release?

If you want to do that stuff, Heroku isn't for you. Heroku is for programmers
who want to build applications.

 

Heroku Enforces Best Practices

![][3]

Do you write good software? If so, you're probably familiar with the term "best
practices". In the past several years, web developers have learned quite a lot
about the ideal way to build web services.

Since web development first became popular, all patterns and development rules
have culminated into something called [The Twelve-Factor App][]. The 12 factor
app is a set of guidelines that service developers should follow to build
applications in the most effective way possible.

Heroku's entire platform not only encourages 12 factor design, but *enforces*
it.

The Heroku platform doesn't tolerate poorly architectured applications, and
doesn't cater to ineffective programmers. Heroku isn't built for idiots.

 

Heroku is Modular

One of Heroku's greatest strengths as a platform is its modularity. The core
Heroku platform gives you a basic way to deploy and manage your codebase and
applications, while the [Heroku addons][] allow you to pick-and-choose your
various infrastructure components.

![][4]

Adding, resizing, and removing infrastructure components is reduced from
hundreds of hours of sysadmin work to a single command.

Instead of building out your own infrastructure services, you can easily spin up
(and down) the services you need, when you need them--paying only for what you
use. Not only do you not need to worry about service interruption (these are all
fully managed infrastructure services), but you have support channels, multiple
options (you can easily swap out addon providers), and excellent monitoring
tools available to you with a single click.

 

Let's Talk About Replacing the Cloud

If you want to build your own redundant, fully managed infrastructure
services--go right ahead. Nobody is stopping you. There are great tools like
Chef, Puppet, and Salt which make this process a lot less painful than it used
to be.

Unfortunately, regardless of which tools you choose--managing infrastructure
with any of the tools above is an enormous job. Sure, you could do what David
did:

> About three days into it, and I had learned how to use Chef (I don't
> write Ruby), brought up two full pluggable configurations for a db node and a
> web node, written a deployment script in Fabric, migrated to the new hardware
> and destroyed my Heroku and Linode instances. Three days, that's all it took
> to replace the cloud.

But all you end up with is a false sense of security in your new design.

![][5]

To quickly debrief you, here's what David did:

-   Rented a dedicated server.
-   Wrote his own Chef scripts to create a database.
-   Wrote his own Chef scripts to create a webserver.
-   Wrote a deployment script using Fabric to deploy his application to his new
    servers.

Sure, this sounds great in theory, right? I mean, why not just rent (or buy) a
physical server or two, and run your stuff there?

Let me ask you this: what happens if:

-   His harddrive / memory / NIC / etc fails?
-   He accidentally runs OS updates, and breaks versions of his packages?
-   He wants to instantly rollback to a previous release of his codebase?
-   He wants to add another server to handle incoming HTTP requests?
-   He needs to spin up a database read slave to handle a high amount of read
    requests?
-   His load balancer fails?
-   His Fabric script stops working because he renamed his project or
    reorganizes his application directory tree?
-   Or one of an infinite amount of other possible problems?

I'll tell you what happens: *his shit stops working*.

Yah--it isn't very hard to spin up some simple infrastructure services using
Chef--pretty much anyone with a couple hours of free time can figure that out.
The real issue is writing and architecturing a platform robust enough to not
only account for hardware / software failure, but also to provide you (the
developer) with enough security that you can effectively sleep easily knowing
that your service will be running when you wake up in the morning.

If I had to bet money, I'd bet on chaos every single time. Why? Because software
is complex. Hardware is complex. Spending a few hours bootstrapping a simple one
or two server setup is nowhere near good enough to run services that people
actually use.

Sure, David may be able to get away with it--for a while. But unless he plans on
dedicating a significant portion of his time over the next couple of years to
actually building a robust service platform--I believe it's safe to say that
David didn't successfully replace the cloud with his work.

If anything, he simply localized his problems.

 

Let's Talk About Monitoring

One of the big points that David mentions in his [article][The Cloud is Not For
You] is that he had no way of monitoring his dyno and application performance,
and couldn't figure out why so many weird things were happening:

> Then shit started to hit the fan.
>
> In no specific order, we started finding numerous problems with various
> systems:
>
> -   Redis takes too much memory to reliably queue Sentry jobs.
> -   Dynos are either memory or CPU bound, but we have no idea how or why.
> -   The Postgres server can't handle any reasonable level of concurrency.
> -   We randomly have to spin up 20 dynos to get anywhere in the queue backlog.

![][6]

Really David? **REALLY?**

Not only is there no data here to substantiate any of these claims--but
monitoring these things is a piece of cake.

One of Heroku's addons is meant specifically to diagnose this sort of issue:
[New Relic][]. If you've never heard of New Relic, you're missing out. It's the
greatest software monitoring system ever built.

Here's a screenshot of the dyno page of my New Relic panel. What you see here
shows the average memory usage of my dynos, as well as the amount of dynos I
have running, and any backlog (HTTP requests that have been waiting in a buffer
because I don't have enough dynos to concurrently process them).

![][7]

Now let's take a look at my web transactions page:

![][8]

I can easily drill down into any view in my Django site, and see exactly what is
happening. I can see (among other things):

-   How much time it took to process.
-   How much time was spent connecting to PostgreSQL.
-   Which queries were ran, and how long each of them took.
-   How much time was spent running various Python functions.
-   How much time was spent hitting external services (memcache, redis,
    whatever).
-   And tons of other things.

Hell, I can even view specific stats on my celery workers:

![][9]

I'm not even going to bother showing you the database tab, because I think you
already have a good idea of where I'm going with this: **Heroku isn't the
problem. You are.**

 

Let's Talk About Bad Ideas

![][10]

First of all, Sentry is an awesome application. I even use it myself for
monitoring multiple services. And in the off-chance you're not famliar with what
Sentry does--it essentially provides application monitoring / error reporting /
error searching for your applications.

However, what does it say (to your customers) when you tell them that:

-   Your monitoring solution is hosted on cheap rented hardware.
-   It takes 24 hours to spin up a new box in the event that one dies.
-   You have no failover plan.
-   You don't spend the time to debug your application issues, and instead blame
    them on vague causes you haven't verified.
-   You openly declare that adding resources on demand is useless.

All in all, it just seems like a bad idea.

 

On the Shoulders of Giants

![][11]

To be totally open and honest (I've written about this a lot in the past)--I've
done the same thing before. I've built my own hardware deployment / backup /
redundant platform.

I've spent time learning Ruby and Puppet. I've spent time provisioning servers
remotely using a combination of Fabric, Jenkins, and Puppet. I've dealt with the
complexities and hardships of making a reliable auto-deployment system with
rollbacks enabled. I've integrated monit, cacti, nagios, and munin monitoring.

At the time, I thought it was an awesome idea. After I jumped over the initial
learning curve, it all seemed like a piece of cake. "*This is the way everyone
should do it! It's so cheap! It's really awesome!*". But after a while, things
changed.

I found myself spending tons of time every month dealing with:

-   Disaster recovery.
-   Building redundant services.
-   Setting up heartbeat to handle failover.
-   Tweaking infrastructure build scripts to add new options to RabbitMQ,
    PostgreSQL, and other services.
-   ...

After a few months passed, I looked back and said to myself: "*What the hell?*".
I went through so much trouble and time to build out my infrastructure and add
in support for backups, recovery, etc.--that I ended up blowing enormous amounts
of time on tasks that have all already been solved by people far smarter than
myself.

To make things even more hilarious--after testing Heroku out initially, and
migrating one of my large work applications over to it--our hosting costs
dropped dramatically. After analyzing the costs, I found that:

-   I saved hundreds of hours of engineering time every month.
-   I didn't have to run redundant services for everything (no redundant load
    balancers, backup web servers, backup workers, backup databases, etc.) since
    Heroku fully manages these things.
-   I didn't have the overhead of running my own monitoring servers / software.
-   I could easily add web servers / workers to the pool when needed.
-   I could easily scale my database by adding read slaves, duplicate masters,
    and any other combination of hardware to support increased I/O needs.
-   My deployment system became instant, and rolling back releases was equally
    easy.
-   All systems administration tasks became a one liner.

While most of our cost savings came in the form of not having to run redundant
servers--it was still an enormous amount.

Through this experience, I learned:

-   Heroku helps you build your applications more effectively by forcing you do
    to things the right way.
-   It is often cheaper to run on top of a managed cloud platform (like Heroku)
    as opposed to handling backups / redundancy yourself--as you have to manage
    and deal with a lot more servers and problems.
-   A lot of the common myths about 'cloud problems' are just that--myths.
-   Outsource things you can't be the best at. I'm not building a deployment
    platform. I'm building software. I'd rather leave the deployment platform to
    someone else who can do it much better than me.

 

Takeaways

I hope you had fun reading this article. I wrote it with humorous intent--not to
be taken seriously or offensively by anyone; I have a lot of respect for David
Cramer.

The main point of this article is to combat the myth that "the cloud is not for
you". The cloud IS for you. Platforms like Heroku allow you to easily develop
and scale your web services in a way that completely changes development.

Instead of spending your time worrying about administration issues, you can code
to your heart's content without worrying about stability, scaling issues, or
anything else.

The next time you write a web service, give [Heroku][] a try; I promise you
won't be disappointed.

 

**UPDATE**: In response to this (and many other related articles I've written) I
wrote a book on Heroku, [The Heroku Hacker's Guide][]. If you're at all
interested in deploying web applications on Heroku, you should check it out.

#### Tags

programming, heroku, rant

#### 51472 views and 32 responses

-   Jun 3 2012, 5:42 PM
    samantha responded:
    this is so true. it makes me want to put on my wizard hat and give you a
    kiss.
-   Jun 3 2012, 8:41 PM
    Harshad Sharma liked this post.
-   Jun 3 2012, 10:06 PM
    Mick responded:
    Thanks for the best introduction to the Heroku
-   Jun 3 2012, 11:12 PM

    soft responded:

    Good debate, and at the end, if u plan to write software for ppl to use,
    then consider use heroku, if u plan to work on hardware site like heroku
    company, then learning those manage and slove problem skill.

    one last thing is, it still not bad to learn those skill, afterall, it's a
    knowledge

-   Jun 4 2012, 1:51 AM

    Victor Widell responded:

    "Per-second billing. (...) If you use a dyno for 2 seconds, you'll pay
    0.16666666666666666 cents."

    This is wrong, or at least could be misunderstood. You pay for wall clock
    time, not CPU time. unless you manually scale down your numbers of dynos
    when they are not needed, you pay the full price for them, regardless of if
    you actually \*used\* them.

    [https://devcenter.heroku.com/articles/usage-and-billing][]

-   Jun 4 2012, 3:56 AM
    Carlo responded:
    The math is wrong as well\
    [http://www.current.wolframalpha.com/input/?i=5+cents+per+hour+][]\*+two+seconds
-   Jun 4 2012, 5:35 AM
    RealMan responded:
    Nice reading, but it seems to be way too positive though. I've never dealt
    with Heroku and I what to see both sides of the medal.\
    I know that you haven't been paid for this article, but Heroku should
    consider such an option, since whole thing looks too bright with all its
    pros, and there is no even one con mentioned.
-   Jun 4 2012, 8:10 AM
    Anthony responded:
    Linux is not UNIX.
-   Jun 4 2012, 9:27 AM
    aemadrid responded:
    You seem to forget about one point that David is making throughout his
    article: money. You make great points about technical solutions but a lot of
    those solutions come with a price that adds up pretty quickly. And given the
    stage of David's company I can understand the predicament. There might be a
    much better/redundant/safe solution out there but if you can't afford it it
    won't stay up for long.
-   Jun 4 2012, 9:39 AM

    Darkside responded:

    I feel like you are right on everything you mentioned, but there are some
    things that I feel like went unsaid.

    VPS are a great resource, and a good way to leverage other's resources, but
    they are not without pitfalls. Even the giants like AWS have shown that they
    are not invincible, if you decide to host 100% of your infrastructure on the
    same VPS you are going to have an outage. It isn't a matter of if but when.

    If you have infrastructure that is so critical an outage would cause you to
    loose large sums of money; you need to find a way to not put all of your
    eggs in one basket.

-   Jun 4 2012, 11:58 AM

    Bob on Rails responded:

    Heroku is an awesome solution for getting things done with the least
    friction possible.

    There have been a couple of comments about heroku getting expensive when you
    start to add services. This is really a perspective issue. As a dev who
    doesn't want to touch servers heroku is cheap. Using heroku I pay more for
    the service than my own VPS but I save \$50-80k/year not having to hire a
    server admin.

    On top of that what is an hour of your time worth? @ \$50/hr if you spend 3
    hours a month per server and have 10 servers you have lost a week of
    development time and \$1,500 in lost productivity.

-   Jun 4 2012, 4:20 PM
    Aseem Kishore responded:
    Fantastic post. Couldn't agree more. Thanks for the insights into New Relic,
    etc.
-   Jun 4 2012, 4:58 PM

    paulbiggar (Twitter) responded:

    Funnily enough, you get the same objections to hosted Continuous Integration
    (something my company makes: [https://circleci.com][]). You can just set up
    Jenkins, sure, but then you have to fix it when it breaks, it's hard to
    scale it up when you get more users, and someone has to spend the time to
    set it up and maintain it. And it wont be nearly as fast as someone who does
    this for a living.

    Or you can use a hosted service, and save your developers time and
    heartache, same as with Heroku.

-   Jun 5 2012, 3:20 AM
    develop7 liked this post.
-   Jun 5 2012, 10:51 AM
    Paweł Gościcki responded:
    The reality is that in most cases (99% ) you don't need all this automatic
    scaling / redundancy / fault tolerance / load balancing / etc. that could
    brings. You only need backups. Why? Because both David's dedicated servers
    and the cloud will fail at some point. Don't believe me that cloud could
    fail? Read about last year's week-long EC2 outage that brought Heroku (among
    others) to its knees. In the end it all comes down to costs. And in my
    opinion it's much more cost effective to rent two dedicated servers, set
    them up once and be done with maintenance for a long time (it will really
    take a long time to fully saturate a modern box, in opposite to saturating a
    single dyno; and updates? Run LTS Ubuntu and don't upgrade distros). When
    those two boxes will become insufficient, believe me, Heroku at this point
    will be even more insufficient for you.
-   Jun 7 2012, 9:25 AM
    Rafal responded:
    Good point, unfortunately as I was reading this, heroku was down for about
    30 minutes :). I still love to use the service as it makes development so
    much easier. Usually the one dyno is enough to get started, unless you need
    background jobs, then your cost jump up. There are services out there which
    allow you to monitor and scale your dynos/workers dynamically which can
    reduce your cost according to your usage.
-   Jun 8 2012, 12:22 PM

    DMcCunney responded:

    \<technical\>

    "it's just unix."

    No, it's not. I started using real Unix (AT&T System V R2) back in the 80's,
    before Linux was a gleam in Linus Torvalds' eye. Linux was designed to look
    and act like Unix, because Linus wanted a version of Unix to play with, but
    couldn't afford the (then) \$70K AT&T wanted for a source license. So he
    stated with Minix, an open source OS intended for teaching OS design, and
    hacked it to be more like Unix.The result is taking over the world. (I run
    the 12.04 LTS release of Ubuntu here.)

    Technically, Linux is the Linux kernel - vmlinuz - and anything using that
    is a Linux system. Android devices like smartphones and eBook readers
    qualify. So does my wireless router.

    Technically again, Unix is whatever passes Spec 1170, a set of 1170 things
    an OS must do to be allowed to call itself Unix. The unix trademark is owned
    by the Open Group, which published Spec 1170, and any OS that conforms with
    it may call itself Unix, regardless of origin. (I was deeply amused to note
    that IBM had added a POSIX compliant sub-system to it's mainframe zOS OS
    that passes 1170.)

    I believe there was a Linux distro years back that \*did\* pass 1170, but
    the distro never achieved traction and the development effort to make it
    pass was ultimately wasted.

    Linux looks enough like Unix that old skool unix users like me didn't have
    to jump through hoops to adjust, but it's not unix.

    On the other hand, Linux has larger supplanted Unix, so it \*s\* a technical
    nitpick.

    \</technical\>

    Other than that, nice article, and Heroku is noted for future reference. I
    have no current need to put apps on the cloud, but it looks like a good
    solution if/when I do.

    Regards,\
    \_\_\_\_\_\_\
    Dennis

-   Jun 12 2012, 11:51 PM

    Mike responded:

    I think you missed Davids point which essentially is that Amazon (and
    therefor it's subs like Heroku and Appharbor) aren't providing the value
    they promise.

    While many are willing to pay a premium for the value that the "cloud"
    offers, when it's 5 times the price, people are going to squeal. Part of the
    problem is that Amazon has not increased the performance of their EC2 units
    significantly since, what 2006? And, they really need to fix their I/O
    problem.

    Luckily, prices have come down some...Hopefully they'll continue.

-   Jun 22 2012, 10:44 AM

    Steve responded:

    ""Per-second billing. (...) If you use a dyno for 2 seconds, you'll pay
    0.16666666666666666 cents."\
    This is wrong, or at least could be misunderstood. You pay for wall clock
    time, not CPU time. unless you manually scale down your numbers of dynos
    when they are not needed, you pay the full price for them, regardless of if
    you actually \*used\* them."

    I think the post was to demonstrate that you only had to pay for what is
    used, there are plenty of options out there for auto scaling dynos, check
    out a gem called hirefire

-   Jun 30 2012, 4:29 PM

    Matthew J. Sahagian responded:

    @DMcCunney

    What a useless post. If you're going to make your debate about a technical
    nitpick, at least get details right. Linus did not "hack MINIX"... Linus had
    tried MINIX, but it wasn't suitable for him. Linux was created with a very
    different design from MINIX and independent from it. The famous argument
    between Linus and Tennenbaum make this clear.

    Stop nitpicking with useless posts unless, at the very least, you're going
    to get your nitpicking right.

-   Jul 13 2012, 9:58 AM
    Aram liked this post.
-   Aug 24 2012, 4:54 AM
    ole\_morten (Twitter) liked this post.
-   Aug 30 2012, 4:37 PM
    Lee Wei Yeong liked this post.
-   Aug 30 2012, 5:02 PM
    Mourad DACHRAOUI liked this post.
-   Aug 30 2012, 5:34 PM
    anatomyofashane (Twitter) liked this post.
-   Aug 30 2012, 10:23 PM
    Saeed Jababr responded:
    New relic is a pretty dam great piece of software.
-   Aug 31 2012, 4:32 AM
    Pedro responded:
    Perhaps, if it's just unix in its core, it should not feature the ridiculous
    word 'cloud' in its website in huge letters?
-   Aug 31 2012, 10:15 AM
    Marty responded:
    Hi, I'm with Atlantic.net ([www.atlantic.net/cloud][] Free Trial). I just
    commented about this on another blog today, the real value in Heroku or
    other service is that you can focus on what's really important. I used the
    example that people use pest control or lawn service vs doing yourself. You
    could do it all yourself, but is it really the most effective use of your
    time/effort what you're trying to accomplish. [And thats when everything
    goes right].
-   Sep 4 2012, 2:11 AM
    palominoz responded:
    Perfect article. Will keep it into my favorites! Great!
-   Sep 19 2012, 9:24 AM
    J0elog (Twitter) responded:
    really well written and thought out article!
-   Sep 28 2012, 5:52 AM
    fortrabbit (Twitter) liked this post.
-   Oct 8 2012, 6:41 AM

    Nic Johnson responded:

    I have been madly impressed by Heroku. Spinning up a new live app takes
    minutes. There's nothing to manage, nothing to provision, nothing to
    install, I just request the components I'm going to need as I need them.

    And it's FREE, until you scale, when it starts to get pricey, but that's OK
    if you have a business model because 90% of the crappy apps that get spun up
    never need to scale.

    Get slashdotted? Dial up extra Dynos! It takes moments to bring your app
    back to life. Try doing that with a dedicated server.

    It makes it wildly easy to try things out. Is this a good idea? I don't
    know, get it on Heroku, see if anyone likes it. Yes it is, there you go.

  [Previous]: ../../../posts/2012/06/silence.html
  [Index]: ../../../index-2.html
  [Next]: ../../../posts/2012/06/how-to-have-fun-programming.html
  [Heroku]: http://www.heroku.com/ "Heroku"
  []: ../../../image/2012/06/41749837-demon.jpg
  [The Cloud is Not For You]: http://justcramer.com/2012/06/02/the-cloud-is-not-for-you/
    "The Cloud is Not For You"
  [David]: http://justcramer.com/ "David Cramer"
  [1]: ../../../image/2012/06/41750244-heroku_is_ubuntu.png
  [dynos]: https://devcenter.heroku.com/articles/dynos "Heroku Dynos"
  [chroot jail]: http://en.wikipedia.org/wiki/Jail_(computer_security) "Jails"
  [cost calculator]: http://www.heroku.com/pricing#0-0 "Heroku Pricing"
  [2]: ../../../image/2012/06/41751754-heroku_apps.png
  [Git]: http://git-scm.com/ "Git"
  [3]: ../../../image/2012/06/41752787-symbol.png
  [The Twelve-Factor App]: http://www.12factor.net/ "12factor"
  [Heroku addons]: https://addons.heroku.com/ "Heroku Addons"
  [4]: ../../../image/2012/06/41755712-heroku_addons.png
  [5]: ../../../image/2012/06/41757677-happy-grin.png
  [6]: ../../../image/2012/06/41757807-i_disapprove.png
  [New Relic]: https://addons.heroku.com/newrelic "New Relic"
  [7]: ../../../image/2012/06/41757961-newrelic-dynos.png
  [8]: ../../../image/2012/06/41758071-newrelic-webtransactions.png
  [9]: ../../../image/2012/06/41758115-newrelic-backgroundtasks.png
  [10]: ../../../image/2012/06/41758459-badtime.jpg
  [11]: ../../../image/2012/06/41758197-giant.png
  [The Heroku Hacker's Guide]: http://www.theherokuhackersguide.com/
    "The Heroku Hacker's Guide"
  [https://devcenter.heroku.com/articles/usage-and-billing]: https://devcenter.heroku.com/articles/usage-and-billing
  [http://www.current.wolframalpha.com/input/?i=5+cents+per+hour+]: http://www.current.wolframalpha.com/input/?i=5+cents+per+hour+
  [https://circleci.com]: https://circleci.com
  [www.atlantic.net/cloud]: http://www.atlantic.net/cloud
