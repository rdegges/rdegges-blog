Title: My Experiences With Tropo and Django
Date: 2011-01-01 12:26
Author: Randall Degges
Tags: programming, python, django, tropo


For the past several weeks, I've been doing a lot of coding on my free time
using [tropo][]. If you aren't into telephony stuff, tropo is basically a
RESTful HTTP API for controlling phone calls, sms messages, and various forms of
instant messaging. I started playing around with their API casually a while
back, but didn't do anything serious with it until a few weeks ago.

The project I've been working on is a fairly complex IVR for a big company,
which requires precise voice recognition with DTMF fallback, as well as custom
grammars (we need to recognize names, cities, street names, etc.), and a wide
array of other things which are far too complex for my normal tools
([asterisk][], [freeswitch][], [opensips][], etc.).

Typically, in order to build such a complex application, I'd have to spin up at
least 5 or so servers: a few asterisk servers, an opensips load balancer, a
database server, and a web server for displaying client statistics and reports.
And that's a lot of work--building the servers, setting up monitoring software,
managing them, etc.

With tropo, I only need two servers: a web server, and a database server (and
then a load balancer if I need multiple web servers for scale). Managing a web
server and a db server is much easier than worrying about managing asterisk
servers, speech recognition software, SIP load balancers, not to mention the
redundant VoIP providers, etc.

My tools of choice for developing tropo apps are python and Django. tropo has a
really simple [WebAPI][] which uses JSON to pass data around back and fourth,
and their [python WebAPI library][] is pretty decent. Writing a full tropo app
with Django on the other hand, not so easy.

This leads me to my next topic: django-tropo.

Since I had to build a pretty complex app with tropo and Django, I figured I
might as well remove some of the major roadblocks and throw together a reusable
Django app. Some of the main problems I had using tropo with Django was:

-   No simple way to track call progress automatically through the duration of a
    call.
-   No simple way to verify that the JSON being POST'ed to your site is valid.
-   No \*easy\* way to handle question / answer call flow without generating
    numerous similar views and hundreds of URLs.

django-tropo solves all of these issues, and in addition, provides many other
helpful tools to make building tropo applications with Django easier.

I'm going to publish django-tropo to github in a few days, so stay tuned for
more information. It will be a rough beta, but I'll continue to work on it and
make sure it keeps improving as time goes on.

 

  [tropo]: https://www.tropo.com/home.jsp "tropo"
  [asterisk]: http://www.asterisk.org/ "asterisk"
  [freeswitch]: http://www.freeswitch.org/ "freeswitch"
  [opensips]: http://www.opensips.org/ "opensips"
  [WebAPI]: https://www.tropo.com/docs/webapi/new_tropo_web_api_overview.htm
    "WebAPI"
  [python WebAPI library]: https://github.com/tropo/tropo-webapi-python
    "python WebAPI library"
