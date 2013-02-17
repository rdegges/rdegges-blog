# Randall Degges

## This is an archived post This is an archived post

[Previous][]   [Index][]

### Django and SSL

May 31 2012, 1:46 PM  by Randall Degges

![][]

I'm a huge proponent of [encrypting everything][]. Why make it easy for the
government (or other nasty organizations) to snoop on your personal data? As
such, over the past several months I've been slowly migrating all my Django
sites to SSL. Along the way, I realized how horrifically underserved the SSL
library market actually is for Django.

A quick search for 'Django + SSL' on google returned nothing useful. I couldn't
find any libraries or tools that made 'forcing HTTPs' a one liner. The only
useful thing I was able to dig up after a bit of hunting was this
[StackOverflow][] post that contains some custom middleware code that forces
HTTPs redirection.

After a bit of unit testing (and some [PyPI][] action), I published
[django-sslify][]--a simple Django app that forces all SSL across your Django
site.

Setting up django-sslify is a piece of cake. Essentially--all you have to do is
install django-sslify (duh!) and then modify your Django settings.py file like
so:

[https://gist.github.com/2845467][]

Once you've done that, all HTTP requests to your site will be perma-redirected
to their https equivalents, forcing SSL encryption.

**NOTE**: If you're using [Heroku][] to host your applications (like I am), be
sure to use their new [SSL endpoint][] addon. The new SSL endpoint addon makes
using SSL as easy as possible.

#### Tags

programming, python, django

#### 4316 views and 4 responses

-   Jun 1 2012, 5:54 PM
    tony-brown-357 liked this post.
-   Jun 2 2012, 4:08 AM
    Leon Matthews responded:
    Is it practical yet to do name-based virtual web-hosting while using SSL? I
    understand that it's possible, given proper browser support -- or do we need
    to stick to IP-based virtual hosts?
-   Jun 4 2012, 5:30 AM
    Domantas responded:
    I don't think it's up to middle-ware to redirect. I's a lot more efficient
    to use virtualhost rewrites since it will be redirect even before passing
    request to wsgi/fcgi utility.Sure if you want lazy one liner - that's also
    an option.
-   Jun 4 2012, 11:23 AM

    Luke Plant responded:

    Your installation instructions are a bit oversimplified, and could do a
    disservice for people in the very common situation of the Django app running
    behind a proxy. You should at least include a link to
    [https://docs.djangoproject.com/en/dev/ref/settings/\#secure-proxy-ssl-header][]
    for the sake of these people.

    You should also remove the hard coded 'HTTP\_X\_FORWARDED\_PROTO', which
    actually opens up a vulnerability. For an explanation of the problem, see
    second paragraph in comment 8 on this ticket -
    [https://code.djangoproject.com/ticket/14597\#comment][]:8

    There are dangers with both false positive and false negatives for checking
    SSL status.

  [Previous]: ../../../posts/2012/06/how-to-have-fun-programming.html
  [Index]: ../../../index-2.html
  []: ../../../image/2012/05/41659257-cloaked.jpg
  [encrypting everything]: http://www.codinghorror.com/blog/2012/02/should-all-web-traffic-be-encrypted.html
    "Encrypt Everything"
  [StackOverflow]: http://stackoverflow.com/questions/8436666/how-to-make-python-on-heroku-https-only
    "Django + SSL"
  [PyPI]: http://pypi.python.org/pypi "pypi"
  [django-sslify]: https://github.com/rdegges/django-sslify "django-sslify"
  [https://gist.github.com/2845467]: https://gist.github.com/2845467
  [Heroku]: http://www.heroku.com/ "Heroku"
  [SSL endpoint]: https://devcenter.heroku.com/articles/ssl-endpoint
    "SSL Endpoint"
  [https://docs.djangoproject.com/en/dev/ref/settings/\#secure-proxy-ssl-header]:
    https://docs.djangoproject.com/en/dev/ref/settings/#secure-proxy-ssl-header
  [https://code.djangoproject.com/ticket/14597\#comment]: https://code.djangoproject.com/ticket/14597#comment
