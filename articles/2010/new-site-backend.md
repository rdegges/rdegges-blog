Author: Randall Degges
Date: 2010-05-30 20:30
Slug: new-site-backend
Tags: programming
Title: New Site Backend


Last night I replaced the old projectb14ck with a newer, more minimalistic
backend and frontend. In this post I’m going to explain a bit about the move,
why it was needed, and my experiences along the way.


## History of projectb14ck

projectb14ck has been around for almost 5 years now. When it first came online,
it was running an extremely simple ASCII-based frontend, all hand-coded in HTML
with inline CSS, and no functionality what-so-ever. It was originally used as a
sort of hackish information dump of random files, bits of information, and code
snippets.

After several months, I decided to turn projectb14ck into a blog (to share
coding information), and messed around with several CMS systems, eventually
settling on wordpress. I picked wordpress at the time because I found a nice
looking dark theme, and it was easy to use.

Using wordpress felt like a *sell out* move, because everyone knows that real
programmers write their own blog engines and don’t go with mainstream trends. I
really hated that part, but decided to keep using it because it was so simple
and just worked.

Approximately 6 months later, I revisited the ‘write your own’ engine thoughts I
originally had, and decided to turn projectb14ck back into an ASCII-based
website. I decided to keep it simple, and just write a PHP script to grab text
files from a directory, output them as simple HTML, and dump them into a list on
the main page. This worked out horribly, because the process of writing posts
was absolutely brutal—I handcoded all of the HTML for each post, and it killed
my drive to write.

projectb14ck remained (mostly) unmaintained until October 2009, when I decided
to switch back to wordpress, and begin writing once again. After moving back to
wordpress, I started writing again, and publishing several articles.

Which brings us to the present…


## Decision to Move from Wordpress to Jekyll

One of the things that really worried me about using wordpress was the security
aspect. There are hundreds of wordpress exploits floating around on the internet
(see [milworm][] and [packetstorm][] for examples). As a programmer, it really
bothered me using something that I knew could be taken offline at any time, and
made me a bit paranoid.

In addition to security problems, wordpress also has a lot of bloat. This in
particular bothered me: I didn’t like using such a large CMS with a lot of 3rd
party plugins—it seemed unnecessary for what I was doing.

So, I began looking for alternatives to wordpress once again. Some of my
favorite blogs ([Ted Dziuba][] and [Extra Cheese][]) were using [Jekyll][], the
static site generator, so I thought I’d investigate what the hype was all about.

As it turns out, static site generators are really awesome:

-   There are no security issues (because you only serve static HTML files).
-   There are no performance issues (you just serve HTML files).
-   There are no deployment issues (you just serve HTML files).
-   It is easy to backup (you just version control your markdown files).
-   Easy to write content (just write markdown).
-   Simple to make style changes (you can use templating languages).

So after checking out Jekyll for myself, I knew I had found a good backend to
switch to.


## The Moving Process

Moving from wordpress to Jekyll was surprisingly easy. I simply dumped my
wordpress posts into markdown format (had to make some manual changes, but
nothing serious), then set up a two line configuration file.

Deploying was even easier. All I had to do was copy my `_site/` files to my
webhost, and bam, everything instantly worked.


## Jekyll Automation

One of the things I’m really enjoying so far about Jekyll, is how easy it is to
perform automation tasks when paired with version control.

I version control my entire weblog with Git (see the [Github project page][]),
and I make all changes / tests locally. Whenever I’m ready to deploy a new post
or configuration change, I simply push to my Github public repo.

On my webhost, I setup a cronjob to perform a `git pull` hourly, to pull in any
new changes that I make. This way, the entire process is fully automated.


## Final Thoughts

projectb14ck has had a long history, and has switched from format-to-format over
the years. After last night’s move to Jekyll, I feel, for the first time, that
I’ve finally found a suitable backend for projectb14ck.

What do you think of the new site? [Drop me a line][] with any comments or
suggestions.


  [milworm]: http://www.milw0rm.com/
  [packetstorm]: http://packetstormsecurity.org/
  [Ted Dziuba]: http://teddziuba.com/
  [Extra Cheese]: http://blog.extracheese.org/
  [Jekyll]: http://github.com/mojombo/jekyll
  [Github project page]: http://github.com/comradeb14ck/projectb14ck
  [Drop me a line]: mailto:rdegges@gmail.com
