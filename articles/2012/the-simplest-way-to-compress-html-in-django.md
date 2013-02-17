# Randall Degges

## This is an archived post This is an archived post

[Previous][]   [Index][]   [Next][]

### The Simplest Way to Compress HTML in Django

March 4 2012, 11:09 PM  by Randall Degges

![][]

I've been working on a lot of website optimization stuff recently for my Django
projects, and thought I'd share a cool utility I found for compressing your
entire site's HTML code.

If you're running any site that could benefit from reduced page load times (who
wouldn't want that?) you may want to consider giving this a go. Essentially,
what we're going to do is take your normal Django template code:

[https://gist.github.com/1977185][]

And compress it so that when the page is rendered for users, it looks like this:

[https://gist.github.com/1977192][]

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

[https://gist.github.com/1977241][]

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

#### Tags

programming, python, django

#### 5139 views and 2 responses

-   Mar 12 2012, 6:14 PM
    SaulShanabrook (Twitter) liked this post.
-   Jan 7 2013, 10:45 AM
    IhoTea responded:
    Thanks, I didn't know about this app.\
    I use just spaceless + gzip middleware.\
    To be true I think thoroughly about every app added to the middleware list.
    How do you think is deleting comments on-the fly worth of it on a middle
    size web application?\
    Maybe decorator proposed is a better way?

  [Previous]: ../../../posts/2012/03/do-the-right-thing.html
  [Index]: ../../../index-3.html
  [Next]: ../../../posts/2012/03/writing-habit-thoughts-continued.html
  []: ../../../image/2012/03/38653843-electron.jpg
  [https://gist.github.com/1977185]: https://gist.github.com/1977185
  [https://gist.github.com/1977192]: https://gist.github.com/1977192
  [django-htmlmin]: https://github.com/cobrateam/django-htmlmin "django-htmlmin"
  [https://gist.github.com/1977241]: https://gist.github.com/1977241
