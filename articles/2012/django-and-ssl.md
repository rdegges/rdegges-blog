Author: Randall Degges
Date: 2012-05-31 20:46
Slug: django-and-ssl
Tags: django, programming, python
Title: Django and SSL


![Cloaked Man][]

I'm a huge proponent of [encrypting everything][]. Why make it easy for the
government (or other nasty organizations) to snoop on your personal data? As
such, over the past several months I've been slowly migrating all my Django
sites to SSL. Along the way, I realized how horrifically underserved the SSL
library market actually is for Django.

A quick search for 'Django + SSL' on Google returned nothing useful. I couldn't
find any libraries or tools that made 'forcing HTTPs' a one liner. The only
useful thing I was able to dig up after a bit of hunting was this
[Stack Overflow][] post that contains some custom middleware code that forces
HTTPs redirection.

After a bit of unit testing (and some [PyPI][] action), I published
[django-sslify][]--a simple Django app that forces all SSL across your Django
site.

Setting up django-sslify is a piece of cake. Essentially--all you have to do is
install django-sslify (duh!) and then modify your Django settings.py file like
so:

``` python
# ...

MIDDLEWARE_CLASSES = (
    'sslify.middleware.SSLifyMiddleware',
    # ...
)

# ...
```

Once you've done that, all HTTP requests to your site will be perma-redirected
to their HTTPs equivalents, forcing SSL encryption.

**NOTE**: If you're using [Heroku][] to host your applications (like I am), be
sure to use their new [SSL endpoint][] addon. The new SSL endpoint addon makes
using SSL as easy as possible.


  [Cloaked Man]: /static/images/cloaked-man.png "Cloaked Man Sketch"
  [encrypting everything]: http://www.codinghorror.com/blog/2012/02/should-all-web-traffic-be-encrypted.html "Encrypt Everything"
  [Stack Overflow]: http://stackoverflow.com/questions/8436666/how-to-make-python-on-heroku-https-only "Django + SSL"
  [PyPI]: http://pypi.python.org/pypi "PyPI"
  [django-sslify]: https://github.com/rdegges/django-sslify "django-sslify"
  [Heroku]: http://www.heroku.com/ "Heroku"
  [SSL endpoint]: https://devcenter.heroku.com/articles/ssl-endpoint "Heroku SSL Endpoint"
