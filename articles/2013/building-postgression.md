# Randall Degges

## This is an archived post This is an archived post

[Previous][]   [Index][]   [Next][]

### Building postgression (an API Development Story)

January 29 2013, 8:39 PM  by Randall Degges

[][]

![][]

[Last week][] my friend [Alven][] and I launched a new service for developers
using PostgreSQL: [postgression][]. postgression is a simple web service that
allows you (a programmer) to instantly provision a free PostgreSQL database that
automatically disappears after 30 minutes.

Why would you use this? Primarily for testing code: running unit tests,
integration tests, etc. It's handy because using postgression means you don't
need to configure (or even run) PostgreSQL server locally just so you can run
some tests.

Since launch, postgression has gotten some decent usage. To date there have
been:

-   1,167 total databases created.
-   229 unique users.

While these aren't huge numbers, we're seeing consistent usage, which means some
developers (whoever you are, thanks!) are using the service to run their unit
tests daily.

I'm not going to talk about why we created postgression in this article (if
you're interested, you can read my [last article][]), but instead, I'd like to
talk about how we built postgression.

Since this was a fun project to build (it's been something I've wanted to make
for a while now), I figured I'd share how we did it, as some of you may
appreciate the technical aspects. From here on out things are going to get
technical :)

Tools

To build postgression Alven and I used [Flask][], a popular Python web
framework. We ended up using the following python libraries:

-   [Flask-Cache][]
-   [Flask-Script][]
-   [Flask-SQLAlchemy][]
-   [gevent][]
-   [gunicorn][]
-   [psycopg2][]
-   [python-dateutil][]
-   [redis][]
-   [requests][]

To host our project website, we used [Github pages][] (it's just a simple static
site).

To host our actual API service, as well as power the entire PostgreSQL database
backend functionality, we used [Heroku][] (the most awesome hosting platform
ever built).

To keep track of our usage statistics and metrics, we used [ducksboard][], a
really awesome company that gives you a dashboard you can customize to your
liking (it displays stuff like Google Analytics, custom metrics, etc.). Also:
ducksboard was nice enough to give us a free account, as postgression is a free
service. If you're interested in a sexy dashboard for your company (or personal
projects), you should check them out!

The Backbone of It All: Heroku

As you can probably imagine (after seeing the tools listed above), Heroku is
really the core of postgression, and is what makes the service possible (and
affordable!).

Heroku is a web application hosting platform, that allows you to easily deploy
your applications (Python, Ruby, Java, Javascript, etc.). We're using Heroku
several ways to make postgression work.

-   We're using Heroku to run our postgression API
    ([http://api.postgression.com][]). Heroku runs our Flask application that
    powers the entire thing. When users make an HTTP request to postgression's
    API, it hits Heroku's servers and stuff happens.
-   We're using [Heroku PostgreSQL][] (the largest hosted PostgreSQL service in
    the world, by the way) to instantly provision PostgreSQL databases for our
    users.
-   We're using [Heroku's API][] to automatically provision and deprovision the
    PostgreSQL databases on Heroku.
-   We're using the [Heroku Scheduler][] to run periodic tasks (like cron).
    Right now this includes: updating our metrics on our ducksboard dashboard,
    deprovisioning databases older than 30 minutes, etc.
-   We're using [Redis To Go][] as our Redis host. We use Redis for preventing
    abuse of the system by throttling requests based on public IP address. Each
    public IP is allowed to provision no more than 100 databases per hour.

After we wrote the initial code for postgression, deploying the entire thing to
Heroku (including setting up cron, Redis, and PostgreSQL) took only a few
minutes. Big kudos to the Heroku team for rocking so hard.

And since I know many of you will ask, here's how much it is currently costing
us to run postgression, along with a price breakdown:

-   1 domain name through [DNSimple][] (the best registrar ever) **\$14** /
    year.
-   1 web dyno to power the postgression API: **\$0** / month.
-   1 Heroku basic database for powering our postgression PostgreSQL database
    (this is what we use to track our databases states): **\$9** / month.
-   1 Redis To Go database: **\$0** / month.
-   Heroku Scheduler usage (you pay by the minute for extra computing resources
    if you go beyond the free tier): **\$0** / month.
-   PostgreSQL backups on AWS powered by Heroku's [pgbackups][] addon: **\$0** /
    month.

Total cost for running this service (per month)? **\~11\$**

Flask and APIs

As I mentioned earlier, we wrote the code for postgression in python and Flask.
Why did we choose these technologies? A few reasons:

-   I'm extremely familiar with python and Flask.
-   Alven had almost no experience with python, so it would be a fun learning
    experience for him.
-   Flask makes writing small web services extremely simple (there's very little
    you have to know to build a functioning service).

For instance, here's the 'core' of our Flask app. Note how simple this is,
compared to the typically heavy amount of base framework code you'll find other
places.

![][1]

Our view code is equally straightforward, and our models aren't too bad either
:)

Here's how the API logic works:

1.  A user makes a request to the API (api.postgression.com).
2.  We get the user's public IP, and increment their usage count in redis using
    Flask-Cache. If the user has gone above the 100 allowed requests per hour
    throttle, we return a HTTP 403 FORBIDDEN code.
3.  We check (using Heroku's API) to see if we have any Heroku apps currently
    provisioned that are not using 100% of their allotted database slots (each
    Heroku app allows you to provision a maximum of 30 PostgreSQL databases).
4.  If there is a Heroku app available, we use Heroku's Addon API
    ([https://api-docs.heroku.com/addons][]) to provision a new Heroku
    PostgreSQL database.
5.  If there is no Heroku app available, we create a new Heroku app using the
    Heroku App API ([https://api-docs.heroku.com/apps][]), then provision a new
    Heroku PostgreSQL database inside the newly created app.
6.  We then check the user's request to see if they want their database
    credentials back as a PostgreSQL connection string, or as a JSON dictionary,
    and return the credentials appropriately.

In the background, we have a cron job running every 10 minutes which
deprovisions any Heroku PostgreSQL databases that were created 30 minutes ago
(or more) using Heroku's API.

Simple, right? When it all comes together, we get the following behavior
(screenshot below).

![][2]

Database Layout

As I mentioned above, postgression uses PostgreSQL itself to keep track of all
the Heroku resources it consumes. This makes it easy for us to keep track of
things like:

-   Usage statistics.
-   How many active Heroku databases we have.
-   Which databases need to be deprovisioned (any database more than 30 minutes
    old).
-   How many Heroku applications we currently have (and if they're running at
    capacity or not).

Stuff like that.

To make all that work, we used Flask-SQLAlchemy to define two simple database
models:

-   HerokuApp, which keeps track of all our Heroku applications.
-   HerokuDB, which keeps track of all our Heroku databases, who created them,
    which Herou app they belong to, when they were created, etc.

Here's how they ended up looking:

![][3]

Management Commands and Monitoring

In order to automatically send statistics to our shiny new ducksboard dashboard
(pictured below), we used Flask-Script to write some simple management commands
that are ran automatically by the Heroku Scheduler every 10 minutes.

![][4]

Writing the cron tasks using Flask-Script is really simple, and makes running
automated tasks a breeeze. Below are a couple of the tasks (screenshot), which
we can run with the **python manage.py blah** command.

![][5]

Pretty easy, right?

Building our Website with Github Pages

The last thing we did was throw together a simple website using Github Pages.
While Alven and I can both throw together a simple website, we figured we'd roll
with the simplest option available.

Essentially what we did was put all our documentation (in Markdown format) into
our project's **README.md**. Then, using the 'Automatic Page Generator'
(available under your Github repository settings), we imported our README file,
picked a theme, and published the site.

After it was published, I made some small tweaks, but nothing too big.

The end result? We got the public website going
([http://www.postgression.com/][postgression]) in about 10 minutes.

Takeaways

Building postgression has been really fun. While it's not a complex project by
any means, it's been on my TODO list for a while, and throwing it online has
been an interesting experience.

Both Alven and I are constantly amazed by:

-   How cheap it is to build a fully functional web service on Heroku.
-   How simple it is to deploy code to Heroku, and make updates.
-   How easy it is to build a REST API using Flask.
-   How fast you can throw together a decent looking web page on Github.

I'd also like to give one last shout out to all the Heroku guys (and gals).
They've built an amazing service, and allowed me to build an entire
Database-Testing-as-a-Service API on top of their platform for almost no cost.

If you're interested in postgression at all, be sure to give it a go and check
out our website: [http://www.postgression.com/][postgression]

If you'd like to know more about postgression, feel free to leave a comment or
shoot me an email ([rdegges@gmail.com][]).

#### Tags

programming, heroku, postgresql

#### 1672 views and 0 responses

  [Previous]: ../../../posts/2013/02/your-actions-define-you.html
  [Index]: ../../../index.html
  [Next]: ../../../posts/2013/01/174560299.html
  []: http://rdegges.com/postgression-a-postgresql-database-for-every
  []: ../../../image/2013/01/47037308-dragon-sketch.jpg
  [Alven]: https://twitter.com/zaidos
  [postgression]: http://www.postgression.com/
  [last article]: http://rdegges.com/postgression-a-postgresql-database-for-every
    "postgression"
  [Flask]: http://flask.pocoo.org/ "Flask"
  [Flask-Cache]: http://packages.python.org/Flask-Cache/
  [Flask-Script]: http://flask-script.readthedocs.org/en/latest/
  [Flask-SQLAlchemy]: http://packages.python.org/Flask-SQLAlchemy/
  [gevent]: http://www.gevent.org/
  [gunicorn]: http://gunicorn.org/
  [psycopg2]: http://initd.org/psycopg/
  [python-dateutil]: http://labix.org/python-dateutil
  [redis]: http://redis.io/
  [requests]: http://docs.python-requests.org/en/latest/
  [Github pages]: http://pages.github.com/
  [Heroku]: http://www.heroku.com/
  [ducksboard]: http://ducksboard.com/
  [http://api.postgression.com]: http://api.postgression.com
  [Heroku PostgreSQL]: https://postgres.heroku.com/
  [Heroku's API]: https://api-docs.heroku.com/
  [Heroku Scheduler]: https://addons.heroku.com/scheduler
  [Redis To Go]: https://addons.heroku.com/redistogo
  [DNSimple]: https://dnsimple.com/r/d9a8f0b92dfb78
  [pgbackups]: https://devcenter.heroku.com/articles/pgbackups
  [1]: ../../../image/2013/01/47036347-postgression-core.png
  [https://api-docs.heroku.com/addons]: https://api-docs.heroku.com/addons
  [https://api-docs.heroku.com/apps]: https://api-docs.heroku.com/apps
  [2]: ../../../image/2013/01/47036497-postgression-api.png
  [3]: ../../../image/2013/01/47036949-postgression-models.png
  [4]: ../../../image/2013/01/47037163-postgression-ducksboard.png
  [5]: ../../../image/2013/01/47037187-postgression-script.png
  [rdegges@gmail.com]: mailto:rdegges@gmail.com
