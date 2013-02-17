# Randall Degges

## This is an archived post This is an archived post

[Previous][]   [Index][]   [Next][]

### pycall 1.5 Released

March 21 2010, 10:00 PM  by Randall Degges

I just released pycall version 1.5! 1.5 contains a new feature, which allows
users to specify a tmpdir (temporary directory) which pycall will use to create
the call files. This can be useful in several situations:

1.  You want to write call files to a cached location, eg: tmpfs.
2.  You need to write the temporary files to the same filesystem that your
    Asterisk spooling directory resides in. This may help avoid permissions
    issues in certain situations.
3.  You’d like to store the temporary files, or set up some other hooks to
    happen when call files are created.

In addition to the new tmpdir feature, pycall 1.5 also contains a lot of style
changes and fixes, and now the code base looks amazing. So be sure to check out
the source on pycall’s [github project page][]!

And of course, this post wouldn’t be complete without me making a plug for
[pycall’s website][] (which now contains a link to the latest version of pycall,
available for download).

## what’s next for pycall?

In the near future, I’ll be adding more demos, examples, and re-writing some
documentation.

I’m also planning on possibly re-writing the pycall website, adding some color,
and making it shiny and web2.0-ish.

So stay tuned, pycall is still going strong!

## want to help?

I accept code, feature requests, copy writing (web and error), and just general
suggestions. So if you’d like to help out, find something you don’t like–and let
me know about it!

#### Tags

programming, python, asterisk

#### 578 views and 0 responses

  [Previous]: ../../../posts/2010/05/announcing-django-quotes.html
  [Index]: ../../../index-7.html
  [Next]: ../../../posts/2010/03/basic-xml-parsing-with-python-and-lxml.html
  [github project page]: http://github.com/comradeb14ck/pycall
  [pycall’s website]: http://pycall.org/
