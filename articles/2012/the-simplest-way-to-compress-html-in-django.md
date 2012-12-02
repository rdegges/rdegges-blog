Title: The Simplest Way to Compress HTML in Django
Date: 2012-03-05 07:09
Author: Randall Degges
Tags: programming, python, django


![Electron][]

I've been working on a lot of website optimization stuff recently for my Django
projects, and thought I'd share a cool utility I found for compressing your
entire site's HTML code.

If you're running any site that could benefit from reduced page load times (who
wouldn't want that?) you may want to consider giving this a go. Essentially,
what we're going to do is take your normal Django template code:

~~~~ {.line_numbers}
1
2
3
4
5
6
~~~~

\<!doctype html\>

\<html lang="en"\>

  \<head\>

    \<meta charset="utf-8" /\>

  \</head\>

\</html\>

And compress it so that when the page is rendered for users, it looks like this:

~~~~ {.line_numbers}
1
~~~~

\<!doctype html\>\<html lang="en"\>\<head\>\<meta
charset="utf-8"/\>\</head\>\</html\>

Obviously this is a simple example, but when you have pages with lots of content
on them, compressing your pages can lead to a really big page load performance
boost, since you're sending significantly less data to the end user.

The Django app you'll be using to do this is [django-htmlmin][]. To get started,
you really only need to do two things:

1.  pip install django-htmlmin
2.  Add 'htmlmin.middleware.HtmlMinifyMiddleware' to your MIDDLEWARE\_CLASSES
    setting.

By default, django-htmlmin will only compress HTML when your DEBUG setting is
set to False (eg: when your site is running in production)--this way, while
you're developing and testing your code, you'll still have your HTML
uncompressed so you can look at it in its original form.

Here's a quick snippet from my settings.py which shows my MIDDLEWARE\_CLASSES
(for reference):

~~~~ {.line_numbers}
1
2
3
4
5
6
7
8
9
~~~~

MIDDLEWARE\_CLASSES = (

    'django.middleware.gzip.GZipMiddleware',

    'htmlmin.middleware.HtmlMinifyMiddleware',

    'django.middleware.common.CommonMiddleware',

    'django.contrib.sessions.middleware.SessionMiddleware',

    'django.middleware.csrf.CsrfViewMiddleware',

    'django.contrib.auth.middleware.AuthenticationMiddleware',

    'django.contrib.messages.middleware.MessageMiddleware',

)

**NOTE**: If you're concerned about compressing HTML, you should probably also
enable Django's GZipMiddleware (as you can see in my snippet above). GZip
compression will greatly reduce the size of your page's data for transfer to the
end user, further decreasing page load time.

**IMPORTANT**: You should always have both GZipMiddleware and
HtmlMinifyMiddleware defined before all other middleware classes. The ordering
of the MIDDLEWARE\_CLASSES tuple matters in Django, and since both of these
middleware modify HTML after it's already been passed through the other
middleware classes, it is necessary to have them executed last by Django (which
means defining them first in MIDDLEWARE\_CLASSES).

  [Electron]: http://getfile5.posterous.com/getfile/files.posterous.com/temp-2012-03-04/jrraHofpiJyflmqhaurrvAaEdeJsvnfFwicjIDBlcxyJGuglnvzaJiIigEqa/electron.jpg.scaled696.jpg
  [django-htmlmin]: https://github.com/cobrateam/django-htmlmin "django-htmlmin"
