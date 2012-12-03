Title: I'm Working on a Startup
Date: 2012-04-08 20:12
Author: Randall Degges
Tags: programming, startup


![Monk][]

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


## OpenCNAM - Caller ID Made Simple

![Opencnam-logo][]

[OpenCNAM][1] is an API company designed from the ground-up to be as simple as
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


  [Monk]: ./images/116559130-1-monk.jpg.scaled696.jpg
  [OpenCNAM]: http://www.opencnam.com/ "OpenCNAM - Caller ID Simplified"
  [Opencnam-logo]: http://getfile6.posterous.com/getfile/files.posterous.com/temp-2012-04-08/yhJaDEuIiuhHreesIDJAJHCCzwJhdBfohxCszykGwtpnmFGJFCxfzHuetlxm/opencnam-logo.png.scaled696.png
  [1]: http://www.opencnam.com/ "OpenCNAM - Caller ID Made Simple"
  [our website]: http://www.opencnam.com/ "OpenCNAM"
  [staff@opencnam.com]: mailto:staff@opencnam.com
