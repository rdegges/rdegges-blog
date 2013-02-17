# Randall Degges

## This is an archived post This is an archived post

[Previous][]   [Index][]   [Next][]

### Generic Django Projects

April 5 2011, 4:34 PM  by Randall Degges

I've been a really big fan of [Django][] since I first started using it almost
1.5 years ago. It's my favorite python web framework ([Flask][] is second,
incase you were wondering), and I've had the good fortune of being able to use
it both for fun and work in a variety of environments.

One of Django's largest strengths is its flexible nature. You're able to
customize your project in an infinite amount of ways, and there are thousands of
third party apps you can download and use in a matter of minutes to help you add
in functionality. Through this past year, I've experimented with lots of apps
and organization strategies on my Django projects, and built what I consider to
be a pretty decent generic Django project.

My generic Django project layout uses several general-purpose Django
applications, and should be a suitable starting point for small-\>medium sized
websites. Out of the box, it provides:

-   Settings files for development, staging, and production environments.
-   Full Django 1.3 support.
-   Out of the box admin interface.
-   Out of the box error tracking interface for site admins using
    [django-sentry][].
-   Out of the box [fabric][] support for easily deploying and managing your
    site.
-   Out of the box database migration support using [South][].
-   Out of the box debug toolbar support using [django-debug-toolbar][].
-   Out of the box production webserver featuring [gunicorn][].
-   Out of the box caching support featuring [memcached][].
-   Out of the box task queue support featuring [Celery][].

I just pushed the first real release to github, which is stable to use, but
doesn't have great documentation. I plan on improving the default setup over the
coming weeks and months, and will continue to make new releases as things are
updated. If you use Django, I'd love to get your feedback to help make the
project better.

If you'd like to check it out, visit the [github project page][].

#### Tags

programming, python, django

#### 8726 views and 0 responses

  [Previous]: ../../../posts/2011/04/simple-continuous-integration-deployment-with.html
  [Index]: ../../../index-5.html
  [Next]: ../../../posts/2011/04/building-a-better-body.html
  [Django]: http://www.djangoproject.com/ "Django"
  [Flask]: http://flask.pocoo.org/ "Flask"
  [django-sentry]: https://github.com/dcramer/django-sentry "django-sentry"
  [fabric]: http://docs.fabfile.org/en/1.0.1/index.html "fabric"
  [South]: http://south.aeracode.org/ "South"
  [django-debug-toolbar]: https://github.com/robhudson/django-debug-toolbar
    "django-debug-toolbar"
  [gunicorn]: http://gunicorn.org/ "gunicorn"
  [memcached]: http://www.tummy.com/Community/software/python-memcached/
    "python-memcached"
  [Celery]: http://celeryproject.org/ "Celery"
  [github project page]: https://github.com/rdegges/django_project
    "django_project"
