# Randall Degges

## This is an archived post This is an archived post

[Previous][]   [Index][]   [Next][]

### Tracking Call Data with django-tropo

January 2 2011, 11:00 PM  by Randall Degges

This article is my attempt to explain how django-tropo implements call tracking.
If you don't know what django-tropo is, you may want to first read my [previous
post][] on the subject.

## The Problem

One of the biggest problems I've had using tropo with Django is handling call
tracking. Let's quickly take a look at the way tropo works for incoming calls:

1.  You pick up your phone and dial a tropo phone number, eg: 555-555-5555.
2.  Tropo (via voxeo's cloud) sees an incoming call to one of their phone
    numbers, and figures out where to route the call to based on your
    application settings.
3.  Tropo sends some data to your web server in JSON format. This data contains
    all of the caller information (what phone number the person is calling from,
    what phone number they're calling to, what type of call it is (voice, sms,
    aim, etc.), and some other stuff.
4.  Your web server gets a POST request from tropo, You build a JSON response
    which tells tropo what to do (eg: say hello world).
5.  Tropo reads in your JSON response, parses it, and then does whatever you
    tell it to (eg: tropo will play “hello world” to the caller).
6.  Tropo then sends a request back to you with additional JSON data, and asks
    for the next set of instructions.
7.  You analyze the new JSON data from tropo, figure out that it is a continuing
    call, and then tell it what to do next.

The workflow here is quite simple: tropo and you are just passing JSON back and
fourth. Tropo's JSON gives you status updates, and your JSON tells tropo what to
do.

In all but the simplest programs, you will undoubtedly want to track your calls
in a meaningful way (eg: a database). This way you can figure out what buttons
the caller pressed (did you ask them for their credit card number?), if callers
hung up before the end of the call (maybe your app is not as cool as you
thought), or a number of other things. Furthermore, tracking your calls gives
you the maximum amount of data that you can have—for billing, statistics, or
whatever else you may need.

This is why it is imperative that you be able to easily track this call data
from tropo.

## How django-tropo Does It

Assuming we want to track all our call data from tropo, what is the easiest way
to do it? By wrapping all of your tropo views with a decorator that
automatically analyzes your JSON from tropo, and creates the corresponding
tracking objects in your database.

django-tropo has two main database models: `TropoSession` and
`TropoSessionStep`, which serve to represent your calls.

A `TropoSession` object contains information about each unique tropo call
session that is started. This includes stuff like the time the call came in,
what the unique callId and sessionId values are, etc.

A `TropoSessionStep` object contains information about each unique call step. A
call step is just JSON data that tropo sends you AFTER the session has been
created. Complex call menus will frequently need a lot of back-and-fourth
communication with tropo to finish a call, so for each ‘call step', a new
`TropoSessionStep` will be created, and associated with the corresponding
`TropoSession` object.

By analyzing a `TropoSession` object, we are able to see not only the session's
identifying information—but also all of the related `TropoSessionStep` objects,
each of which contains all tropo information for each step of the call as it was
being processed. This lets us look back and get any bit of information we could
possibly need about any call, at any time.

Here's an example of how simple a tropo view can look, using django-tropo's
`tropo_view` decorator:

    from tropo import Tropo
    from djtropo.decorators import tropo_view

    @tropo_view
    def ivrdemo(request):
        """Say 'hello, world!' then hangup."""

        t = Tropo()
        t.say('hello, world!')
        t.hangup()
        return t.RenderJson()

Without any hard work at all, this decorator analyzes your JSON data, and
creates the necessary database models.

NOTE: You may also notice that this view only returns a string (some JSON data).
This is because the `tropo_view` decorator also provides some other useful
features for dealing with tropo views, including the ability to automatically
create an appropriate `HttpResponse` object to render your JSON and set the
mimetype to `'application/json'`.

## Future Improvements

Not everyone wants to log all of their call data, but some people may still want
to use the `tropo_view` decorator for its other helpful features.

For these people, I plan to add a settings flag, `TROPO_TRACK_CALLS`, which when
turned on will log all calls, and when turned off (or not specified) will NOT do
any logging. This should give developers more fine-grained control over their
applications, and hopefully make it useful in many situations and environments.

## Suggestions?

Got a better way to handle call tracking? I'd love to hear it!

#### 8610 views and 0 responses

  [Previous]: ../../../posts/2011/01/startup-mode.html
  [Index]: ../../../index-6.html
  [Next]: ../../../posts/2011/01/my-experiences-with-tropo.html
  [previous post]: http://projectb14ck.org/my-experiences-with-tropo
