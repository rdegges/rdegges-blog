# Randall Degges

## This is an archived post This is an archived post

[Previous][]   [Index][]   [Next][]

### I'm Working on a Startup

April 8 2012, 1:12 PM  by Randall Degges

![][]

For the past several months, I've been working with some really great friends of
mine on a startup, [OpenCNAM][]. As some of you know, I have a lot of experience
doing telephony development: primarily building large backend systems.

Since I started hacking telephony code (almost 4 years ago), one thing that has
always bothered me is how hard it is to find (and use) telephony data. As anyone
in the telephony industry will tell you: the industry is huge, old, and
fragmented.

Even the simplest of all things: getting someone's caller ID name information
given a telephone number is tremendously difficult. The way caller ID works on
the PSTN (public telephone network) is insane. Caller ID information is
fragmented, stored in huge carrier databases, and there is no simple way to
extract the information.

For programmers (like me), this makes building telephony applications a
nightmare. Frankly, we were tired of jumping through hoops to get simple caller
ID data, and decided to do something about it.

OpenCNAM - Caller ID Made Simple

![][1]

[OpenCNAM][2] is an API company designed from the ground-up to be as simple as
possible. The main idea is to make getting caller ID information easy--*really
easy*. No more negotiating deals with telco providers and surfing through
endless pages of Google looking for relevant information.

If you're a programmer looking to use caller ID information in your software,
and you can't get something working within 5 minutes of visiting [our
website][], we've failed our mission.

In the spirit of making things as simple as possible, OpenCNAM also has a
generous free tier. That's right. If you're writing an application that does
less than 60 queries per hour, you don't even need an account with us! All you
have to do is hit our API endpoint, and BAM, you get results back. It's that
easy.

Need to do more than 60 queries per hour? No problem. Once we're out of beta
you'll be able to create an account, add some funds, and pay for exactly what
you use (with automatic volume discounts). 

**NOTE**: If you'd like to give our beta a try, send an email to
[staff@opencnam.com][] and we'll get you an API key.

With 17,717,475 successful queries and counting, OpenCNAM is already taking off.
If you do any telephony development at all, I'd love to hear your feedback.

Over the coming months, I'll be spending more time writing about my startup
journeys, so be sure to check back!

#### Tags

programming, startup

#### 19699 views and 4 responses

-   Apr 8 2012, 8:24 PM
    fred responded:
    Almost all of the queries I made come back with blank info. Does that mean
    the phone number owner has the name info unlisted/blocked?
-   Apr 8 2012, 8:38 PM

    Randall Degges responded:

    @fred hey! If you're hitting the API without an API key, we will only return
    caller ID results if the number is already in our cache. So, if you get a
    blank result, try again in a few seconds and you should see a valid caller
    ID name.

    We do this so that we don't make requests take forever (since it takes a few
    seconds to lookup caller ID information in our backend).

-   Apr 9 2012, 4:41 AM
    James Sun responded:
    Thanks @Randall. I saw the same behavior that fred did and waiting a few
    secs did the trick. Great service... I can't begin to imagine all the hoops
    that you guys jumped through to get this data.
-   Apr 9 2012, 7:30 AM
    tony-brown-357 liked this post.

  [Previous]: ../../../posts/2012/04/why-im-learning-node.html
  [Index]: ../../../index-3.html
  [Next]: ../../../posts/2012/03/do-stuff-have-fun.html
  []: ../../../image/2012/04/39873354-monk.jpg
  [OpenCNAM]: http://www.opencnam.com/ "OpenCNAM - Caller ID Simplified"
  [1]: ../../../image/2012/04/39872506-opencnam-logo.png
  [2]: http://www.opencnam.com/ "OpenCNAM - Caller ID Made Simple"
  [our website]: http://www.opencnam.com/ "OpenCNAM"
  [staff@opencnam.com]: mailto:staff@opencnam.com
