# Randall Degges

## This is an archived post This is an archived post

[Previous][]   [Index][]   [Next][]

### Why I'm Learning Node

April 7 2012, 8:13 PM  by Randall Degges

![][]

As many (if not all) of you know, I'm a python guy. I program every single day,
and most days, that involves using python. I write Django code for fun and
profit, and also do a fair amount of "raw" python coding (no frameworks).

Unfortunately, while I do a lot of kickass backend coding, I don't often get a
chance to practice my front-end kung fu. Since I'm fairly knowledgeable about
the backend stuff I work on (telephony services, web services, etc.), it doesn't
often make sense for me to also build the front end code (I'm looking at you:
HTML, CSS, Javascript). As a result of this lack of front-end coding practice, I
suck at it.

I suck at HTML, CSS, and most of all, Javascript.

Over the past few years, my lack of front end knowledge has really began to
bother me. I enjoy doing full-stack development. I like taking projects from
conception to completion. Something about the creation process resonates really
well with me. What I don't like, however, is feeling crippled when I have an
idea, write the backend for it, and then have to spend months fiddling around
with the front-end to make it look even half decent.

Since I first started writing websites, Javascript has really changed a lot.
While Javascript used to be a toy language for moving navigation elements on a
page, it's now usable everywhere: server side (nodejs), client side (backbone),
and even in the database (mongo).

So, after many years of not properly learning Javascript--I decided to take it
seriously and dive in. My goal is to become a proficient Javascript programmer,
so that I can:

-   Use some of the new Javascript frameworks and tools that are out there
    (node, backbone, knockout, etc.).
-   Contribute to Javascript projects that I like.
-   Write my own Javascript code (server side and client side) without feeling
    like an idiot.
-   See what all the fuss is about.

Since I'm already familiar with programming concepts, the first thing I did was
pick up the highly recommended book: [Javascript: The Good Parts][]. I read
through that a few weeks ago, and subsequently failed to find a good way to
apply my newly found Javascript knowledge to anything practical.

**NOTE**: If you're an experienced programmer looking to learn Javascript, you
probably can't do any better than reading [Javascript: The Good Parts][]. It's
extremely short, concise, and enjoyable to read. Highly recommended.

To actually practice my newly learned Javascript-fu, I decided to
give [nodejs][] a try. Since I learn best by writing command line applications
(who actually enjoys reloading their browser to test code?), I figured node
would be a good choice.

If you aren't familiar with node (why are you reading this?), it's a server-side
Javascript interpreter. That means you can use it just like python, ruby, or
perl--directly from the command line (no browser required). It also comes with
an extremely elegant package manager ([npm][]), that makes building and sharing
reusable node modules extremely simple.

After reading so many negative things about [nodejs][1], I'm completely
surprised to report that it is actually pretty damn cool. While I'm not
(currently) using it for buliding websites or anything like that, it really
makes Javascript a lot simpler for people like me: backend developers who want
to learn Javascript without all the barriers to entry (I'm looking at you, web
browsers).

node has a great package manager, tons of extremely awesome modules, a large
developer community, and great documentation. I was able to write my first
reusable nodejs module ([node-opencnam][]) in less than 1 hour. That's pretty
insane.

I went from 0 knowledge of nodejs to a working, publicly available, reusable
module, in less than 60 minutes. Booya.

After playing around with nodejs a bit, I've decided to continue learning it. So
far it has been extremely useful in my quest to learn Javascript, and as I've
continued to build things using it, I've been slowly getting more and more
familiar with Javascript as a language.

As I learn more about Javascript, I'll keep you all updated!

**NOTE**: I'm going to start reading through the [Node Beginner Book][] this
weekend. You should check it out.

 

#### Tags

programming, javascript, nodejs

#### 25021 views and 23 responses

-   Apr 7 2012, 8:44 PM

    Rich Jones responded:

    Great post, Randall!

    Node is a blast to write stuff in - wait till you try out Socket.io!!

-   Apr 7 2012, 8:52 PM
    Lucas Cooper responded:
    After you learn a bit of JavaScript, go look at Coffeescript. ;)
-   Apr 7 2012, 11:25 PM

    Hasen responded:

    (I wrote this comment also on HN)

    The problem with node is it decreases your productivity tremendously.

    The most important thing about the choice of programming tools is
    productivity. Node.JS does not promise you any productivity gains. Node
    sells itself as a solution to "slowness" caused by "blocking IO". What's the
    solution? All I/O is evented! This means you have to write everything with
    callbacks.

    You may see some very nice libraries/tools coming out around node.js, like
    jade, coffeescript, and stylus. These are all nice and good, and they do
    increase your productivity, but, only on the client side.

    If you're looking for performance and non-blocking IO, use Go, it's much
    better at that.

-   Apr 7 2012, 11:47 PM
    Isaac Z Schlueter responded:
    Glad you're finding it useful :) JavaScript is an extremely important
    language, so it's worth mastering, warts and all. If you can make friends
    with the language itself, then you realize that the browser is basically
    just a very terribly designed JavaScript interface. The fact that we seem to
    use it for just about every UI has made me scared to dig into any other UI
    platform.
-   Apr 8 2012, 12:22 AM
    Alleagra responded:
    That's a beautiful drawing that fronts your blog! \
    Can I ask you where you got it from - presumably it's public domain?
-   Apr 8 2012, 12:38 AM
    Manuel Kiessling responded:
    Great post! Thanks for recommending my book, much appreciated!
-   Apr 8 2012, 1:11 AM

    filipminev (Twitter) responded:

    Great article! I believe in sharing what you learned, the reasons behind it
    and the process itself. It helps us and it helps you get a better
    understanding on the subject. So thank you for sharing with us what you went
    through.

    The reasons above are why I built this website ([http://www.tiljs.com/)][].
    It's all about what JavaScript tricks you learned today and sharing it with
    the rest of the community. Most certainly you can already contribute with a
    useful JS trick or two.

-   Apr 8 2012, 1:41 AM
    yusen liked this post.
-   Apr 8 2012, 1:41 AM
    yusen liked this post.
-   Apr 8 2012, 1:41 AM
    yusen liked this post.
-   Apr 8 2012, 1:50 AM

    Tore Green responded:

    Congratulations, I think you have found your calling. Digging into
    javascript to brush up on front-end development -- only to ditch the browser
    for a command line interface first chance you get. Now, that's the mark of a
    true back-end developer.

    Anyway, you have to start somewhere ;-)

-   Apr 8 2012, 2:16 AM
    optiss (Twitter) responded:
    Um, but after learning node you still don't know anything about frontend
    coding, so your problem isn't really solved. Okay, javascript knowledge is
    better, but as you pointed out - browsers render HTML with CSS and JS. And
    truth be told, DOM complicates stuff when it comes to JS.
-   Apr 8 2012, 3:12 AM

    hmans responded:

    "it's a server-side Javascript interpreter"

    Nope, it's an event-based framework for developing networked applications.

    People were writing back-end JavaScript applications back in 2001 with
    frameworks like Helma.

-   Apr 8 2012, 3:26 AM

    Hakan Bilgin responded:

    Agree with optiss. With Node you will probably get good idea of how the
    Javascript language works; the event loop and you will get an excersize
    writing the syntax. Writing in a non-blocking manner is a shift in mind that
    doesn't come easy to a traditionally schooled serverside programmer (not
    implying that you're one).

    HTML structure highly affects how the CSS is written. Both HTML and CSS
    affect both the programming structure and speed of JS...if not well
    considered, they can affect the JS performance and you will end up
    troubleshooting in JS...without realizing the fact that the first two can be
    optimized.

    With that said, I'm really happy to read this post.\
    Good luck in your endeavours :)

    PS: it doesn't sound like you will dive into the browser environment any
    time soon (understandable :)). Regardless, I would like to nudge you towards
    Less CSS (http://lesscss.org/).\>

-   Apr 8 2012, 6:40 AM
    tony-brown-357 liked this post.
-   Apr 8 2012, 7:46 PM
    karmellapineda responded:
    Very interesting details you have noted on [fake nails][]and [dermal
    piercings][], thank you for putting [goes][] up. "Whatever one man is
    capable of conceiving, other men will be able to achieve." by Jules Verne.
-   Apr 9 2012, 6:50 AM
    supremelydisappointing liked this post.
-   Apr 10 2012, 6:40 PM
    narcelio (Twitter) responded:
    What do you use instead of reload the browser? iwatch?
-   Apr 17 2012, 1:44 AM

    Ryan Sharp responded:

    @Isaac Z Schlueter

    JavaScript is NOT an "important language" at all. It's useful for the odd
    AJAX request or sortable table in the browser. That's about it. Node.js was
    just well-timed to appeal to the latest generation on clueless hipsters, but
    it's nethier revolutionary nor "good".

    You've made an investment learning and writing a lot of JavaScript and now
    you've got buyer's remorse. ES5 is about as good as backward-compatible
    JavaScript is gonna get. Harmony will never make it. If you open up the door
    to adding backwards-incompatible changes you may as well just use a
    different language alltogther. One that isn't the awful piece of shit that
    JavaScript is.

-   Apr 17 2012, 8:01 AM

    Isaac Z Schlueter responded:

    @Ryan Sharp

    How do you define "importance" with respect to programming languages?

    You ought not to tell me what I am remorseful about. It is unthinkably rude
    to tell someone else what they're feeling. Your comment only serves to
    diminish your own reputability and helps to establish a reputation for
    yourself as a childish fool. Is that your desire?

-   Apr 17 2012, 9:05 AM

    Tony Brown responded:

    @Issac well put, I couldn't have said it better.

    I am learning node.js being new to both client and server side programming I
    figured it would be beneficial working with one code base

-   Jun 26 2012, 12:41 PM
    Anton Shuvalov responded:
    Перевод на русский (ranslated into Russian) [http://it-prm.com/?p=390][]
-   Nov 8 2012, 11:11 PM

    johnny-john responded:

    ​1. Node is not a "server side JS interpreter", it is using V8.

    ​2. Callbacks suck.

    ​3. I could join the mafia in 60 minutes or less, but that doesn't mean I
    should.

    Best wishes,\
    John Dobronski

  [Previous]: ../../../posts/2012/04/successful-github-development.html
  [Index]: ../../../index-3.html
  [Next]: ../../../posts/2012/04/im-working-on-a-startup.html
  []: ../../../image/2012/04/39847963-jsninja.jpg
  [Javascript: The Good Parts]: http://www.amazon.com/gp/product/0596517742/ref=as_li_ss_tl?ie=UTF8&tag=rdegges-20&linkCode=as2&camp=1789&creative=390957&creativeASIN=0596517742
    "Javascript: The Good Parts"
  [nodejs]: http://nodejs.org/ "nodejs"
  [npm]: http://npmjs.org/ "npm"
  [1]: http://teddziuba.com/2011/10/node-js-is-cancer.html "nodejs is cancer"
  [node-opencnam]: https://github.com/telephonyresearch/node-opencnam
    "node-opencnam"
  [Node Beginner Book]: http://www.nodebeginner.org/ "Node Beginner"
  [http://www.tiljs.com/)]: http://www.tiljs.com/)
  [fake nails]: http://www.fakenails.ca
  [dermal piercings]: http://www.dermalpiercings.net
  [goes]: http://www.goes.ca
  [http://it-prm.com/?p=390]: http://it-prm.com/?p=390
