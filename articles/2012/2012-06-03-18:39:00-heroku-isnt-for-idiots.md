% Heroku Isn't for Idiots
%
%

Heroku Isn't for Idiots
=======================

**WARNING**: This is a *bit* of a rant. I'm going to assume you have a
basic understanding of [Heroku](http://www.heroku.com/ "Heroku"), and
web application architecture.

[![Demon](http://getfile5.posterous.com/getfile/files.posterous.com/temp-2012-06-03/FACcxEFxDxBadFDwqJonIryClzuasvergxlFkeHFwttziwqmqDCnDBinkyai/demon.jpg.scaled696.jpg)](./images/135701004-0-demon.jpg.scaled1000.jpg)

Heroku isn't for idiots. There--I said it. I'm already starting to feel
better!

I'm usually not the type of person to get involved in internet debate
(because I typically avoid politics as much as possible), but yesterday
I read an incredibly fradulent article which really made my blood boil:
[The Cloud is Not For
You](http://justcramer.com/2012/06/02/the-cloud-is-not-for-you/ "The Cloud is Not For You").

Before I start to rip holes in David's article, I'd just like to
say--[David](http://justcramer.com/ "David Cramer") is a good
programmer. In fact--he's a much better programmer than myself. If
you're involved in the python web programming world, you've likely used
his software, and seen his presentations.

While I've never met the guy, he seems like a decent dude. So if you
take anything away from this rant, let it be purely technical \>:)

Also: I should probably mention that I'm in no way affiliated with
Heroku. Like David, I know several Heroku employees, but that's the
extent of my relationship.

Ok then! Let's talk about Heroku!

 

Heroku is Just Unix

At its core, Heroku is just a simple unix platform; specifically, Ubuntu
10.04 LTS.

![Heroku\_is\_ubuntu](http://getfile4.posterous.com/getfile/files.posterous.com/temp-2012-06-03/ofrsItFhDewDcorGBBqBCIblAnDCubrayCpvjfGfyJFoqsypinahnbFahyos/heroku_is_ubuntu.png.scaled696.png)

The entire Heroku platform is really nothing more than small Ubuntu
virtual server instances that can be spun up and down on demand. Each
instance (Heroku calls them
[dynos](https://devcenter.heroku.com/articles/dynos "Heroku Dynos")),
has:

-   512MB of RAM, 1GB of swap. Total = 1.5GB RAM.
-   4 CPU cores (Intel Xeon X5550 @ 2.67GHz).
-   Isolated execution. Anything you store on your dyno will be isolated
    from all other dynos.
-   A [chroot
    jail](http://en.wikipedia.org/wiki/Jail_(computer_security) "Jails")
    environment. This means that you are completely locked down to one
    directory tree, with no write access to system files.
-   Per-second billing. Dynos cost 5 cents per hour. You get one dyno
    for free each month (per app). If you use a dyno for 2 seconds,
    you'll pay 0.16666666666666666 cents. If you use a dyno for an
    entire month solid, you'll pay 36\$ (assuming a month has 30 days).

**NOTE**: If you want to calculate your costs for running your
application on Heroku, they have a really nice [cost
calculator](http://www.heroku.com/pricing#0-0 "Heroku Pricing") you can
use.

So what does this mean? Well, it basically means that Heroku can run
pretty much any Ubuntu compatible software you can think of. It also
means that there's really nothing \*magical\* going on here--it's just
unix.

 

Heroku is Centered Around Apps

Unlike most other cloud providers, Heroku isn't in the VPS business.
Their goal isn't to sell you virtual Ubuntu servers that you configure
yourself. Their real goal is to handle that stuff for you, and let you
focus on building applications.

![Heroku\_apps](http://getfile7.posterous.com/getfile/files.posterous.com/temp-2012-06-03/HDpCfimxiImoCkoogixFozBixjCrDBfifoAEJEjIsEiJxDagApkCieuofDmh/heroku_apps.png.scaled696.png)

The main idea is that:

1.  An application is a piece of software that talks over HTTP
    (websites, APIs, whatever).
2.  An application exists to serve a single purpose.
3.  An application has a single codebase, version controlled with
    [Git](http://git-scm.com/ "Git").
4.  An application should consist only of application code--not
    infrastructure setup, or anything else.
5.  Applications should be easy to scale horizontally. Need to support
    more incoming requests? Just add another application server to your
    pool, and Heroku will handle the load balancing.
6.  Applications should be fault tolerant. Did your application or
    server crash? It will be instantly running on another virtual server
    so that you experience no downtime.
7.  Applications should be easy to rollback. Need to rollback to a
    previous release? You can do it instantly.
8.  Applications should use environment variables for configuration. You
    should never hardcode your configuration.

Does that sound good to you? Of course it does! Heroku encourages good
application design and architecture from the ground up, by making
certain assumptions about your project from the start.

If you're writing an application--why should you have to:

-   Run redundant servers incase something breaks?
-   Spend time configuring every piece of your infrastructure?
-   Worry about hardware or host issues?
-   Build your own deployment systems so that you can instantly deploy
    your code across hundreds of virtual servers at once?
-   Build your own rollback system to instantly restore your application
    to a previous release?

If you want to do that stuff, Heroku isn't for you. Heroku is for
programmers who want to build applications.

 

Heroku Enforces Best Practices

![Symbol](http://getfile5.posterous.com/getfile/files.posterous.com/temp-2012-06-03/vbkatadExvBlxIFeBCewFFAdHjJrdqFFnFFwkDDycyHgmnwvlbjhaxuIGumr/symbol.png.scaled696.png)

Do you write good software? If so, you're probably familiar with the
term "best practices". In the past several years, web developers have
learned quite a lot about the ideal way to build web services.

Since web development first became popular, all patterns and development
rules have culminated into something called [The Twelve-Factor
App](http://www.12factor.net/ "12factor"). The 12 factor app is a set of
guidelines that service developers should follow to build applications
in the most effective way possible.

Heroku's entire platform not only encourages 12 factor design, but
*enforces* it.

The Heroku platform doesn't tolerate poorly architectured applications,
and doesn't cater to ineffective programmers. Heroku isn't built for
idiots.

 

Heroku is Modular

One of Heroku's greatest strengths as a platform is its modularity. The
core Heroku platform gives you a basic way to deploy and manage your
codebase and applications, while the [Heroku
addons](https://addons.heroku.com/ "Heroku Addons") allow you to
pick-and-choose your various infrastructure components.

![Heroku\_addons](http://getfile0.posterous.com/getfile/files.posterous.com/temp-2012-06-03/xxfEBeEmusCbqHqhkblxjryHyDjmiqrbhGewwdefvGgoqgADztqxwDstkEdJ/heroku_addons.png.scaled696.png)

Adding, resizing, and removing infrastructure components is reduced from
hundreds of hours of sysadmin work to a single command.

Instead of building out your own infrastructure services, you can easily
spin up (and down) the services you need, when you need them--paying
only for what you use. Not only do you not need to worry about service
interruption (these are all fully managed infrastructure services), but
you have support channels, multiple options (you can easily swap out
addon providers), and excellent monitoring tools available to you with a
single click.

 

Let's Talk About Replacing the Cloud

If you want to build your own redundant, fully managed infrastructure
services--go right ahead. Nobody is stopping you. There are great tools
like Chef, Puppet, and Salt which make this process a lot less painful
than it used to be.

Unfortunately, regardless of which tools you choose--managing
infrastructure with any of the tools above is an enormous job. Sure, you
could do what David did:

> About three days into it, and I had learned how to use Chef (I don’t
> write Ruby), brought up two full pluggable configurations for a db
> node and a web node, written a deployment script in Fabric, migrated
> to the new hardware and destroyed my Heroku and Linode instances.
> Three days, that’s all it took to replace the cloud.

But all you end up with is a false sense of security in your new design.

![Happy-grin](http://getfile5.posterous.com/getfile/files.posterous.com/temp-2012-06-03/uDFdsrmsypBBpxviiDaqBjvlkdiDfGuHiEhsmsdEIpJExhoCyheovsbgJwBb/happy-grin.png.scaled696.png)

To quickly debrief you, here's what David did:

-   Rented a dedicated server.
-   Wrote his own Chef scripts to create a database.
-   Wrote his own Chef scripts to create a webserver.
-   Wrote a deployment script using Fabric to deploy his application to
    his new servers.

Sure, this sounds great in theory, right? I mean, why not just rent (or
buy) a physical server or two, and run your stuff there?

Let me ask you this: what happens if:

-   His harddrive / memory / NIC / etc fails?
-   He accidentally runs OS updates, and breaks versions of his
    packages?
-   He wants to instantly rollback to a previous release of his
    codebase?
-   He wants to add another server to handle incoming HTTP requests?
-   He needs to spin up a database read slave to handle a high amount of
    read requests?
-   His load balancer fails?
-   His Fabric script stops working because he renamed his project or
    reorganizes his application directory tree?
-   Or one of an infinite amount of other possible problems?

I'll tell you what happens: *his shit stops working*.

Yah--it isn't very hard to spin up some simple infrastructure services
using Chef--pretty much anyone with a couple hours of free time can
figure that out. The real issue is writing and architecturing a platform
robust enough to not only account for hardware / software failure, but
also to provide you (the developer) with enough security that you can
effectively sleep easily knowing that your service will be running when
you wake up in the morning.

If I had to bet money, I'd bet on chaos every single time. Why? Because
software is complex. Hardware is complex. Spending a few hours
bootstrapping a simple one or two server setup is nowhere near good
enough to run services that people actually use.

Sure, David may be able to get away with it--for a while. But unless he
plans on dedicating a significant portion of his time over the next
couple of years to actually building a robust service platform--I
believe it's safe to say that David didn't successfully replace the
cloud with his work.

If anything, he simply localized his problems.

 

Let's Talk About Monitoring

One of the big points that David mentions in his
[article](http://justcramer.com/2012/06/02/the-cloud-is-not-for-you/ "The Cloud is Not For You") is
that he had no way of monitoring his dyno and application performance,
and couldn't figure out why so many weird things were happening:

> Then shit started to hit the fan.
>
> In no specific order, we started finding numerous problems with
> various systems:
>
> -   Redis takes too much memory to reliably queue Sentry jobs.
> -   Dynos are either memory or CPU bound, but we have no idea how or
>     why.
> -   The Postgres server can’t handle any reasonable level of
>     concurrency.
> -   We randomly have to spin up 20 dynos to get anywhere in the queue
>     backlog.

![I\_disapprove](http://getfile5.posterous.com/getfile/files.posterous.com/temp-2012-06-03/bfeunxGescIIitkDrgeFqfJiFsAsigyDpDHxBDxagHqzfpCotkwrfgubnIDE/i_disapprove.png.scaled696.png)

Really David? **REALLY?**

Not only is there no data here to substantiate any of these claims--but
monitoring these things is a piece of cake.

One of Heroku's addons is meant specifically to diagnose this sort of
issue: [New Relic](https://addons.heroku.com/newrelic "New Relic"). If
you've never heard of New Relic, you're missing out. It's the greatest
software monitoring system ever built.

Here's a screenshot of the dyno page of my New Relic panel. What you see
here shows the average memory usage of my dynos, as well as the amount
of dynos I have running, and any backlog (HTTP requests that have been
waiting in a buffer because I don't have enough dynos to concurrently
process them).

[![Newrelic-dynos](http://getfile7.posterous.com/getfile/files.posterous.com/temp-2012-06-03/JdGEBklrDArGlfecEjInCHbqFraxjhDaepEtrxegerECIuGpvDIagenbcAEn/newrelic-dynos.png.scaled696.png)](./images/135701004-7-newrelic-dynos.png.scaled1000.png)

Now let's take a look at my web transactions page:

[![Newrelic-webtransactions](http://getfile8.posterous.com/getfile/files.posterous.com/temp-2012-06-03/pJxxxHtiFuqJmImgvhFlzcgIzkbJbpJojjxfzcuepsfbruCspqvevbzqCgjq/newrelic-webtransactions.png.scaled696.png)](./images/135701004-8-newrelic-webtransactions.png.scaled1000.png)

I can easily drill down into any view in my Django site, and see exactly
what is happening. I can see (among other things):

-   How much time it took to process.
-   How much time was spent connecting to PostgreSQL.
-   Which queries were ran, and how long each of them took.
-   How much time was spent running various Python functions.
-   How much time was spent hitting external services (memcache, redis,
    whatever).
-   And tons of other things.

Hell, I can even view specific stats on my celery workers:

[![Newrelic-backgroundtasks](http://getfile5.posterous.com/getfile/files.posterous.com/temp-2012-06-03/GDuivmEaxDaJAcxrsaksfxruBglavgvEqBGzDxzdFmhtbmidsaEqwudailGA/newrelic-backgroundtasks.png.scaled696.png)](./images/135701004-9-newrelic-backgroundtasks.png.scaled1000.png)

I'm not even going to bother showing you the database tab, because I
think you already have a good idea of where I'm going with
this: **Heroku isn't the problem. You are.**

 

Let's Talk About Bad Ideas

![Badtime](http://getfile0.posterous.com/getfile/files.posterous.com/temp-2012-06-03/GrgGCuflFltbBejBqGfIpyFxtjiBqrcJADxcjfdwrlcDGtAyvJhHGpFtBfwE/badtime.jpg.scaled696.jpg)

First of all, Sentry is an awesome application. I even use it myself for
monitoring multiple services. And in the off-chance you're not famliar
with what Sentry does--it essentially provides application monitoring /
error reporting / error searching for your applications.

However, what does it say (to your customers) when you tell them that:

-   Your monitoring solution is hosted on cheap rented hardware.
-   It takes 24 hours to spin up a new box in the event that one dies.
-   You have no failover plan.
-   You don't spend the time to debug your application issues, and
    instead blame them on vague causes you haven't verified.
-   You openly declare that adding resources on demand is useless.

All in all, it just seems like a bad idea.

 

On the Shoulders of Giants

[![Giant](http://getfile1.posterous.com/getfile/files.posterous.com/temp-2012-06-03/fcizGqoyFrJDGnvBpgeotFirwiFvhCaaBdyblluvHxIuqjIvBinIgtjjJctn/giant.png.scaled696.png)](./images/135701004-10-giant.png.scaled1000.png)

To be totally open and honest (I've written about this a lot in the
past)--I've done the same thing before. I've built my own hardware
deployment / backup / redundant platform.

I've spent time learning Ruby and Puppet. I've spent time provisioning
servers remotely using a combination of Fabric, Jenkins, and Puppet.
I've dealt with the complexities and hardships of making a reliable
auto-deployment system with rollbacks enabled. I've integrated monit,
cacti, nagios, and munin monitoring.

At the time, I thought it was an awesome idea. After I jumped over the
initial learning curve, it all seemed like a piece of cake. "*This is
the way everyone should do it! It's so cheap! It's really awesome!*".
But after a while, things changed.

I found myself spending tons of time every month dealing with:

-   Disaster recovery.
-   Building redundant services.
-   Setting up heartbeat to handle failover.
-   Tweaking infrastructure build scripts to add new options to
    RabbitMQ, PostgreSQL, and other services.
-   ...

After a few months passed, I looked back and said to myself: "*What the
hell?*". I went through so much trouble and time to build out my
infrastructure and add in support for backups, recovery, etc.--that I
ended up blowing enormous amounts of time on tasks that have all already
been solved by people far smarter than myself.

To make things even more hilarious--after testing Heroku out initially,
and migrating one of my large work applications over to it--our hosting
costs dropped dramatically. After analyzing the costs, I found that:

-   I saved hundreds of hours of engineering time every month.
-   I didn't have to run redundant services for everything (no redundant
    load balancers, backup web servers, backup workers, backup
    databases, etc.) since Heroku fully manages these things.
-   I didn't have the overhead of running my own monitoring servers /
    software.
-   I could easily add web servers / workers to the pool when needed.
-   I could easily scale my database by adding read slaves, duplicate
    masters, and any other combination of hardware to support increased
    I/O needs.
-   My deployment system became instant, and rolling back releases was
    equally easy.
-   All systems administration tasks became a one liner.

While most of our cost savings came in the form of not having to run
redundant servers--it was still an enormous amount.

Through this experience, I learned:

-   Heroku helps you build your applications more effectively by forcing
    you do to things the right way.
-   It is often cheaper to run on top of a managed cloud platform (like
    Heroku) as opposed to handling backups / redundancy yourself--as you
    have to manage and deal with a lot more servers and problems.
-   A lot of the common myths about 'cloud problems' are just
    that--myths.
-   Outsource things you can't be the best at. I'm not building a
    deployment platform. I'm building software. I'd rather leave the
    deployment platform to someone else who can do it much better than
    me.

 

Takeaways

I hope you had fun reading this article. I wrote it with humorous
intent--not to be taken seriously or offensively by anyone; I have a lot
of respect for David Cramer.

The main point of this article is to combat the myth that "the cloud is
not for you". The cloud IS for you. Platforms like Heroku allow you to
easily develop and scale your web services in a way that completely
changes development.

Instead of spending your time worrying about administration issues, you
can code to your heart's content without worrying about stability,
scaling issues, or anything else.

The next time you write a web service, give
[Heroku](http://www.heroku.com/ "Heroku") a try; I promise you won't be
disappointed.

 

**UPDATE**: In response to this (and many other related articles I've
written) I wrote a book on Heroku, [The Heroku Hacker's
Guide](http://www.theherokuhackersguide.com/ "The Heroku Hacker's Guide").
If you're at all interested in deploying web applications on Heroku, you
should check it out.
