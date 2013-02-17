# Randall Degges

## This is an archived post This is an archived post

[Previous][]   [Index][]   [Next][]

### The Perfect Django Settings File

April 29 2011, 1:25 AM  by Randall Degges

I know this isn't the best way to start an article, but *I lied*. This article
won't show you how to make the perfect Django settings file. Instead, it will
show you how to build the perfect Django settings *module*.

When I'm first starting a new Django project, I like to make sure that my
settings file(s) are crafted in an organized, ideal manner. My settings file(s)
should allow me to:

-   Maintain as many specific deployment environment settings as I choose in a
    clear and separate manner. For example: *production*, *staging*,
    *development*, etc.
-   Maintain common project settings that are used by all specific deployment
    environments. For example: maybe *production*, *staging*, and *development*
    all share a common **ADMINS** setting. I don't want to duplicate this code
    in all of my enviornment specific settings files.
-   Easily test and deploy code to each specific environment.

Old Habits Die Hard

I'd like to quickly discuss why I think most people design their settings
file(s) wrong.

The approach to settings that I see many people take is simplistic--they'll
define all of their values in their settings.py file, and then at the bottom of
the file write something like this:

[https://gist.github.com/948029][]

What sucks about this approach is that you now need to maintain a file--in this
case **settings\_local.py** for each of your environments, and it isn't going to
be easy to version control. Why? Because you have two choices in this scenario
of fail:

1.  Don't version control the **settings\_local.py** files on your various
    servers. This sucks because now you've got stuff like database credentials
    that will lay around, and gradually break future deployments when you forget
    to update them. It also sucks because you've got to constantly worry about
    that file. It isn't part of your source repository, so you have to back it
    up manually, and take special care of it.
2.  Version control the **settings\_local.py** files for each environment you
    have, and then manually change the **settings.py** file (or
    **\_\_init\_\_.py** file) in your project folder on each server. Now you've
    got the same problem as before--you've got to manually manage some source
    files, and constantly worry about breaking shit.

There's a better way.

Write a Settings Module

Instead of maintaining multiple flat settings files, build a settings module. Go
ahead and **rm** your **settings.py** file, and in its place, create a new
directory, called **settings**, and put in a blank **\_\_init\_\_.py** file to
start.

The goal here will be to do meet all the criteria that I defined in the
beginning of this article. In order to meet all those requirements, we need to:

1.  Define a **common.py** file inside of our settings module. This file will
    contain all of our 'shared' settings between all environments. For example:\

    [https://gist.github.com/948038][]

2.  Define as many environment-specific settings files as you need. Just make
    sure to import from **common** at the top of your file. For example--here's
    a **dev.py** file:\

    [https://gist.github.com/948040][]

3.  When you're using any sort of management command, just specify the settings
    module you want to use. For example: instead of running **python manage.py
    syncdb**, you would do **python manage.py syncdb --settings=settings.dev**.
    Same goes with runserver, or any other command.

As you can see, the benefits of a settings module are numerous, and meet all of
our requirements. We're able to have our cake (by version controlling all
environment specific settings) and eat it too (by choosing which one to use
simply via our application server).

I've been working on numerous Django projects for around 2 years now, and I've
not found any better ways to do it.

Got Suggestions?

If so, I'd love to hear them. Seriously.

#### Tags

programming, python, django

#### 10413 views and 15 responses

-   Apr 29 2011, 9:30 AM

    tryptid (Twitter) responded:

    This is nice. This is essentially what we do (after trying the
    settings\_local way and finding it as awful as you said) except we haven't
    been keeping them in a settings package, which is a nice idea (instead we've
    just been keeping settings.py, development.py, production.py, etc in the
    project root, which I must say isn't as clean looking).

    One thing you could do is store your common settings in
    settings/\_\_init\_\_.py (or import settings.common in that file) and have
    your common settings work as sane defaults for a local environment (sqlite,
    in-memory or no caching, etc), which would make running manage.py without a
    --settings flag work fine locally since it loads myproject.settings by
    default.

    Another thing that makes things even nicer is if you set up your project
    with buildout, then you get a bin/django script which replaces manage.py and
    is already set up to use the correct settings file for the environment
    you're in (this is what we do). Buildout definitely seems like some black
    magic but once you get it set up it's quite nice.

-   Apr 29 2011, 10:12 AM

    Randall Degges responded:

    @tryptid the \_\_init\_\_.py idea is pretty cool. I sometimes do that when
    I'm working on projects by myself. I typcailly keep it in the structure I
    defined for big projects though, that way there is no ambiguousness for
    other developers.

    Also--I use virtualenv, and virtualenvwrapper, but I do like buildout too!
    Sounds like you've got a pretty sweet setup as well :)

-   Apr 29 2011, 1:16 PM

    odwyerrob (Twitter) responded:

    I like this idea.

    One suggestion for making it easier to use management commands (who wants to
    type in the settings file every time?):

    alias manage="python manage.py --settings=settings.dev"

    Define this alias differently in each virtualenv or bashrc file so that each
    project will automatically use the right one. For a virtualenv template, you
    could even hardcode the path to manage.py so that it can be used from any
    folder!

-   May 3 2011, 12:19 AM
    bel\_alex (Twitter) responded:
    And what about database credentials, personal keys using such an approach ?\
    You are probably creating a dev.py file and a similar file with filled
    passwords and keys (which is not controlled by VCS)
-   May 25 2011, 3:58 AM
    pacek responded:
    Great article, thanks. I'm using it from now :)
-   Jun 21 2011, 3:38 PM

    vvalbergg (Twitter) responded:

    Hi Randall!

    I'm really intrigued by your configuration - and that you've put it up for
    grabs on github (i've already made a fork).

    After playing a bit around with your configuration I wished I there was some
    sort of "walkthrough" on how you use it. Here I'm thinking examples like why
    you've defined something in settings.dev and not in settings.prod, and even
    how you deploy using this setup.

    So hereby an actual blogpost request on how you use your setup found at
    [https://github.com/rdegges/django\_project][] :)

    If you don't have the time or simply don't want to, I understand - it's at
    least worth the try :)

    Regards\
    Víðir Valberg Guðmundsson

-   Jun 22 2011, 11:36 AM
    Randall Degges responded:
    @vvalbergg Definitely. I'll do a follow-up post in the next few days
    describing it in more detail :)
-   Oct 1 2011, 1:26 PM
    Michele responded:
    You've lied :) however it's a really nice article, thanks.
-   Oct 1 2011, 1:27 PM
    Michele responded:
    You've lied :) however it's a really nice article, thanks.
-   May 16 2012, 1:06 AM

    mbaechtold (Twitter) responded:

    As bel\_alex points out, how do you handle database credentials? Those
    values should not go into a VCS.

    And how would you craft a settings module if you have to support a multi
    site setup, i.e. how to have common settings for all sites and all
    environments, common settings for all environments but just a specific site,
    common settings for all sites but just a specific environment?

-   Oct 10 2012, 11:40 AM
    Tadeo responded:
    Hi, I've been using a similar approach some time ago, I even used specific
    settings files to features branches.\
    To avoid having to define the --settings flag I called the folder
    settings\_files where include common.py, dev.py, deploy.py, etc. In dev.py
    and deploy.py I imported common.py and finally I just make a symbolic
    settings.py link by hand in each environment to the desired specific file.
    The symbolic link was ignored including it in .gitignore. Simple and
    effective.\
    Anyway, as was pointed out versioning sensible information is not a good
    practice. So I moved to the plain local\_settings.py approach, where I
    include just the sensible data as SECRET\_KEY and DATABASES and a few
    specifics settings.\
    Probably a mix of the two approaches was "the perfect" django settings, but
    I think the development environment should be as equal as possible to the
    deploy one, so the particular settings must be really a few.\
    Thank you.
-   Nov 8 2012, 3:08 PM

    Mat responded:

    With buildout usage, use 'djangoprojectrecipe', that use the same approche
    from base.py to server\_live.py and how many env you need.

    All your settings are located in \<project\>/settings/

    [http://pypi.python.org/pypi/djangoprojectrecipe/1.1][]

-   Nov 20 2012, 4:30 PM

    Bifx responded:

    How do you deal with FilePathFields in models, where the paths are different
    in production and development, e.g. this used to work:\
    datafile = models.FilePathField(path=settings.DATA\_DIR, max\_length=500)

    But now I get a 'module' object has no attribute 'DATA\_DIR' because the
    prefixes are now:\
    settings.production.DATA\_DIR\
    or\
    settings.development.DATA\_DIR\
    etc...

    I don't see how the perfect django settings fits with this, any pointers
    greatly appreciated.

-   Feb 8 2013, 4:08 PM

    Bevis responded:

    Bifx: you should be importing settings like:

    from django.conf import settings

    If you do, then that problem goes away.

-   Feb 8 2013, 5:43 PM

    bifx responded:

    Bevis: this is not a simple import issue. The issue is that the path will
    always be wrong.

    The solution I've implemented is Tadeo's symbolic link.

  [Previous]: ../../../posts/2011/05/diet-updates.html
  [Index]: ../../../index-5.html
  [Next]: ../../../posts/2011/04/how-to-not-be-lean.html
  [https://gist.github.com/948029]: https://gist.github.com/948029
  [https://gist.github.com/948038]: https://gist.github.com/948038
  [https://gist.github.com/948040]: https://gist.github.com/948040
  [https://github.com/rdegges/django\_project]: https://github.com/rdegges/django_project
  [http://pypi.python.org/pypi/djangoprojectrecipe/1.1]: http://pypi.python.org/pypi/djangoprojectrecipe/1.1
