---
title: "Don't Kill Basic Auth"
date: "2015-03-18"
tags: ["programming"]
slug: "dont-kill-basic-auth"
status: "draft"
description: "A small rant on why you should continue to allow HTTP Basic Authentication for your API services, and why it matters."
---


![Wall-e Sketch][]


One of the disturbing trends I've noticed over the past few years is that more
and more API services are slowly ditching support for HTTP Basic Authentication
(*aka: Basic Auth*) in favor of OAuth.

As someone who's been:

- Using REST API services for years.
- Has built numerous REST API services.
- Has built and ran a REST API company.
- Is currently working at a large developer-focused REST API company.

I can't help but feel like this is a *bad thing*.

It's a *bad thing* because OAuth (*as popular as it is*) is a huge pain in the
ass -- for both the people building the API services as well as the developers
consuming them.

OAuth is complex, misunderstood, widely misused, and lacking universal
implementations.

Basic Auth, on the other hand, is simple, very well understood, and has been
well supported in every language and framework since the 1990's.

So please -- if you work on any sort of REST API, please read the rest of this
article!  It might just help you out.


## HTTP Basic Authentication

To start, let's talk about Basic Auth:

- It's a well and clearly defined [specification][].
- It's been around since ~1996.
- It's super simple.

Here's the short version of how it works.

- You are a developer.
- You have an API key pair: an *API Key ID* and an *API Key Secret*.  Each of
  these is a randomly generated string (*usually a [uuid][]*).
- To authenticate against an API service, all you need to do is put your
  credentials into the [HTTP Authorization header][].

Here's an example showing how Basic Auth works on the command line using
[cURL][]:

```console
$ curl --user 'xxx:yyy' https://api.someapi.com/v1/blah
```

Here's some Python code showing how it works:

```python
>>> from requests import get
>>>
>>> api_key_id = 'xxx'
>>> api_key_secret = 'yyy'
>>>
>>> resp = get('https://api.someapi.com/v1/blah', auth=(api_key_id, api_key_secret))
>>> print resp.json()
```

And if that isn't enough, here's some Node.js code showing it as well:

```javascript
var request = require('request');

var api_key_id = 'xxx';
var api_key_secret = 'yyy';

request({
  url: 'https://api.someapi.com/v1/blah',
  auth: {
    'user': api_key_id,
    'pass': api_key_secret
  }
}, function(err, resp, body) {
  console.log(body);
});
```

Isn't that simple?

All Basic Auth lets you do (*at a high level of abstraction*) is simply specify
your credentials.  That's it.

**NOTE**: *Yes, I know that technically it's a little bit more complicated than
this.  There's base64 encoding, there's the HTTP Authorization header format,
etc., but for argument's sake I'm leaving that out as it's not important in this
context.*

Basic Auth is great for developers because it's simple, intuitive, and easy to
use.

Let's say you're integrating with an API service, all you do is create an API
Key Pair, and start making requests!

What do you do if you accidentally publicize your API Key Pair and it's
compromised?  Well...  You just create another API Key Pair, put that into your
application code, and get rid of the old one!

When used properly, Basic Auth can be a great choice for securing REST APIs.


## The Problem with Basic Auth

Now, for a long time, Basic Auth was *the shit*.  Everyone loved it.  But then
people started to build API services that had to be consumed in new ways.

Take mobile, for example.

Let's say you're building an Android application.  You start by writing a REST
API that does stuff for a mobile app.  So you secure your API with Basic Auth.

You then start writing your Android application code itself, and realize *"Hey,
I need to store my API Keys somewhere so my Android app can access them, right?
How else would I communicate with my API?"*

But then you realize that'd be a bad idea.  It would be a bad idea because if
you store your API keys in your mobile app source code, it'll be very easy for
someone to reverse engineer Android application binary and find your API key
-- thereby exploiting your API service.

This is where Basic Auth starts to *not make sense*: it simply wasn't built to
handle situations where you need to access an API service from an unsecured
device.

The same is true, of course, for client-side web applications.  With the rise of
front-end frameworks like Angular.js and React.js, more and more developers are
using private API services to directly power their website front ends.

In situations like this, the same problem pops up: how can you securely store
your Basic Auth credentials in an unsecured environment like a browser or a
mobile app?

The answer is: *you can't*.

Which brings us to OAuth...


## OAuth

OAuth is a very flexible protocol that is *primarily* designed to handle API
authentication via third party.

What do I mean by this?  Well, it's primary purpose is to grant you, a
developer, *temporary* access to an API service.

Now, OAuth technically has 4 separate *"modes"*, also known as *"grant types"*,
which each are used for different purposes.

The one you're most likely familiar with (*if you use a lot of mobile apps*) is
the *"password grant type"*.

Here's how it works at a glance:

- You (*as a developer*) ask a user for their credentials (*usually an email
  address and password*) in either a mobile app or a web browser.
- You then take these credentials and send them to your API service which
  verifies the credentials, then generates a *temporary access token* for the
  user.
- You then store this *temporary access token* in the unsecured location (*a
  mobile app or a browser*).
- Now that you have that token, whenever you need to make an authenticated API
  request to your API service, you simply put that token into the
  [HTTP Authorization Header][] and that will be used to authenticate you.

Now, this obviously works a bit better than Basic Auth because with OAuth,
there's no need to distribute your API credentials with your mobile app or
browser based code -- instead you generate a *temporary access token* ON THE
FLY.

This essentially makes it impossible for an attacker to *reverse engineer* your
browser code or mobile app binary to find your hidden credentials.

But -- and this is a fairly big *but* -- the security of your *temporary access
token* is directly dependent on the amount of time this token lasts.

Here's what I mean:

Let's say you implement the OAuth Password Grant flow in your mobile
application.  And let's say that when you generate *temporary access tokens* via
your API service, you expire these tokens after 6 months.

This means that if an attacker can read the *temporary access token* from
wherever you store it on your mobile device: they can abuse your API service
until that token expires (*6 months in this case*).

So, if you want to improve the security of your API, you simply have to reduce
the amount of time this *temporary access token* is valid, right?

Well, let's say you make it expire after 1 day.

This means that every single day, your users will have to re-authenticate by
entering their email address and password into your app!  How annoying!

So, as you can see: with OAuth there's a direct tradeoff between security and
convenience.

Now -- I'm not saying this is a bad thing.  Quite the contrary: it's an
incredibly useful thing!  It's a huge step up from the alternatives -- but
here's the thing: it has it's use.


## The Problem with OAuth

The main problem with OAuth is that th

























**NOTE**: I'm going to get a bit raw here, feel free to skip this one if ya
like.

Let's start with the basics.

If you're building an API service, you primarily have four choices when it
comes to *securing* your API:

- You can use [HTTP Basic Auth][].
- You can use [HTTP Digest Auth][].
- You can use [OAuth 1.0a][].
- You can use [OAuth 2.0][].

Now, you could decide to build your own shit all together (*I've seen this plenty
of times*): but this is almost universally a bad idea.  If you go this route, I
can almost *guarantee* you you're going to screw something up, and cause all
sorts of issues for not only yourself, but the poor developers who end up using
your API service as well.

*So, for the sake of conversation, we'll rule that last one out.*

Now, let me ask you some questions: how familiar are you with the above
authentication protocols?  Do you know how they work?  Do you know how to use
them on the server and the client?  Do you know in which cases they're useful,
and in which cases they're harmful?

Unless you're very familiar with API security, chances are that you don't.  And
that's ok!

The truth is that HTTP Basic Auth (*and Digest Auth*) are very old protocols --
they've been around since ~1996.

If you've been writing API services for a while, you're likely familiar with
Basic Auth (*and potentially Digest Auth*) -- but if you're a newer developer,
odds are you've only seen and been exposed to OAuth.

And herein lies my personal problem: I've noticed a disturbing trend amongst
tech companies: they're gradually starting to phase out HTTP Basic Auth support
in favor of OAuth.

In this article, I'm going to attempt to explain why this is a *bad idea*, and
why you (*as a developer*) should push to support HTTP Basic Authentication in
all of your API services when possible.


## How Basic Auth Works

Before we get into the ranting bit, I want to take a moment to explain how HTTP
Basic Auth works.  It's *really basic*.  All it requires is that you specify a
*username* and *password* to authenticate.

*That's it!*

I like to think of HTTP Basic Auth as a simple login page for API services.  As
long as you (*the developer*) keep your username and password safe and sound,
you won't have any problems: you'll be able to authenticate securely, and make
requests.

Are there downsides to something this simple?  Well, of course:

- If you have an easy to guess username and password, it can *and will* be
  brute forced by a bad guy.
- If you accidentally publish your username and password somewhere (*like
  Github*), someone will find it.
- If one of the servers that contains your username and password is compromised,
  then the attacker can simply find where your username and password are on the
  server, and then get access to your account.

But let's not focus on the bad.  Let's dig a little deeper.


### How It Works: Headers

All forms of HTTP authentication (*at least when it comes to API services*)
start in the [HTTP Authorization header][].

HTTP headers are nothing more than metadata about your HTTP requests.  The HTTP
Authorization header is a well-defined header which is meant to hold
authentication information.

Regardless of which authentication protocol you use (*HTTP Basic Auth, OAuth,
etc.*) -- you'll be using this header value no matter what.

Now, we already know that HTTP Basic Auth needs a username and password.  But
how does it require those fields in the HTTP Authorization header?

*Good question!*  Here's how it works: the value of the Authorization header
must be: the word "Basic " followed by "username:password" as a base-64 encoded
string.

Here's an example:

- Our username will be "rdegges".
- Our password will be "el0nmusKistheCOOLEST!"

If I wanted to authenticate to an HTTP Basic Auth service, I'd use the following
Authorization header:

```text
Basic cmRlZ2dlczplbDBubXVzS2lzdGhlQ09PTEVTVCE=
```

`cmRlZ2dlczplbDBubXVzS2lzdGhlQ09PTEVTVCE=` is the base64 encoded version of
"rdegges:el0nmusKistheCOOLEST!".

When I send that value to an API service (*in the Authorization header*), the
service will:

- See that I specified 'Basic' as my protocol.  This is how the service will
  know I'm using HTTP Basic Auth.
- Base64 decode the remaining value.
- Parse the resulting string, splitting on the ':' character.
- The first part of the string will be the *username*.
- The second part of the string will be the *password*.
- If those two values are valid (*eg: if I'm a real user*), then the service
  will let me make my request.  Otherwise, it'll return an HTTP 401 UNAUTHORIZED
  response.


### Using Basic Auth

If you've ever worked with an HTTP Basic Auth API service before, you've likely
messed around with [cURL][] command line tool.  It handles HTTP Basic Auth for
you via the `--user` flag.

Here's an API request made to a fake API service that properly uses Basic Auth
credentials:

```console
$ curl --user 'rdegges:el0nmusKistheCOOLEST!' https://api.someservice.com/v1/logs
```

The `curl` tool will automatically handle the base64 encoding for me, and will
generate a properly formed HTTP Authorization header.










Now, in the real world, a *username* and *password* will be API keys.  The
*username* field will be an API Key ID, and the *password* field will be an API
Key Secret.

But the rules are the same: the *API Key ID* is an API key that can be made
public (*like a username*) -- but the *API Key Secret* is an API key that must
be kept private (*like a password*).  These two values, when used together,
will allow a user to authenticate with HTTP Basic Auth.

The HTTP Basic Auth specification basically says that in order to construct your
HTTP Authorization header properly, you need to set its contents to the
following:

```text
Authorization: Basic base64(username:password)
```

So, just to clarify: the value you put into the HTTP Authorization header **must
contain** the word 'Basic ' followed by a base64 encoded string that contains
the username, a colon character, then the password.











I'm getting pretty *fucking* tired of [OAuth][].


TOC:

- Intro
- The Problem with Basic Auth
- OAuth to the Rescue!
- The Problem with OAuth
- Why You Need Both
- Summary





  [Wall-e Sketch]: /static/images/2015/wall-e-sketch.jpg "Wall-e Sketch"
  [OAuth]: http://oauth.net/ "OAuth"
  [specification]: http://tools.ietf.org/html/rfc2617 "HTTP Basic Auth / Digest Auth Spec"
  [uuid]: http://en.wikipedia.org/wiki/Universally_unique_identifier "UUID on Wikipedia"
  [HTTP Authorization header]: http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html "HTTP Headers Spec"
  [HTTP Basic Auth]: http://tools.ietf.org/html/rfc2617 "HTTP Basic Auth / Digest Auth Spec"
  [HTTP Digest Auth]: http://tools.ietf.org/html/rfc2617 "HTTP Basic Auth / Digest Auth Spec"
  [OAuth 1.0a]: http://tools.ietf.org/html/rfc5849 "OAuth 1.0a Spec"
  [OAuth 2.0]: http://tools.ietf.org/html/rfc6749 "OAuth 2.0 Spec"
  [cURL]: http://curl.haxx.se/ "cURL"
