# Randall Degges

## This is an archived post This is an archived post

[Index][]   [Next][]

### Service Oriented Flask

October 12 2012, 3:35 PM  by Randall Degges

![][]

This article is the first of a series which discusses the ins and outs of
building service oriented [Flask][] web applications. If you aren't famliar with
web application architecture, service oriented development, or the python
programming language, you may want to skip this.

If you'd like a quick refresher on service oriented application architecture,
you may want to first read:

-   [Service Oriented Side Effects][]
-   [Service Oriented Problems][]

In this arcitle, I'll be discussing some reasons why you may want to use Flask
for your next service oriented web application (over other technologies), and
how it can help you build your application with as little pain as possible.

Why Flask?

![][1]

In the python world, there are lots of web frameworks available. Of all the
frameworks, however, I'd argue that the top two (at least in terms of
popularity) are [Django][] and [Flask][].

While Django has a larger userbase, more available third party apps, and much
richer feature set--Flask has a smaller userbase, less third party apps, and a
much smaller feature set. Both projects are well maintained and have many active
developers, and both projects have excellent documentation. The main difference
between the two is their approach to web development.

Django takes the *standard* approach to web development, making the following
assumptions:

-   Your website will have a single codebase.
-   You will store all of your data in a realtional database (preferably
    PostgreSQL).
-   You will primarily develop your Django website as a composition of third
    party Django applications.
-   You will use Django's authentication framework for handling user
    authentication.

Flask takes the *razor blade* approach to web development, making the following
assumptions:

-   You will have many small codebases (if not, you will regret it!).
-   You will use a combination of database engines in your applications (or none
    at all).
-   You will build your website as a composition of HTTP services.

Django's approach is excellent for a majority of web developers. If you're
building small to medium sized web applications, throwing together a Django site
can be an excellent (and quick!) way to get your site online withour sacrificing
code quality.

Flask's approach is excellent for complex and medium to large scale projects
that require the usage of multiple database technologies, multiple services
(maybe a website and an API), and projects that need to be maintained long-term
by multiple people.

Let's analyze the benefits of using Flask individually.

​1. Small Codebases

Flask is built around the idea of having a small codebase. While this is not
explicitly documented anywhere that I'm aware of--it is true regardless. While
it is *possible* to build a Flask applications with thousands of lines of code,
doing so would be more pain that it is worth.

Flask (at the time of writing) recommends you build small, simple web services.
All the Flask documentation, sample code, and community presentations support
this. The Flask framework itself seems to be built around this idea as well.

While there are many framework implementation details to support this, a great
example of this is the [route][] decorator.

\<script src="https://gist.github.com/3882273.js?file=views.py"\>\</script\>

Compare this to the Django version below.

\<script src="https://gist.github.com/3882259.js?file=urls.py"\>\</script\>

The Flask example combines the view code and the URL routing logic in the same
place (*yes, it is possible to break it up like Django does, but this is the
common use pattern*), while the Django version separates URL patterns and view
code completely.

Having your routing code and view code in the same place means that managing
larger projects would become painful as you'd have to grep for URL patterns
through lots of view code as opposed to opening up a separate module and viewing
the decoupled URL routing logic.

For further comparisons, I'd recommend looking at Django's [settings
documentation][] vs Flask's [settings documentation][2].

​2. Database Independence

While Django is built with relational databases in mind, including a
non-optional ORM and many third party applications that rely on it--Flask comes
with nothing out of the box.

Flask instead requires you to pick and choose your database technologies
(PostgreSQL, Redis, Cassandra, MongoDB, etc.) and integrate them via one of the
available [Flask extensions][], or via a lower-level driver of some sort.

For complex and large scale web applications, using a single database technology
is rarely an option. While it may be ideal to store your user account data in a
realtional database like PostgreSQL, it is often times faster and simpler to
store other data in other types of datastores--for instance, maybe you'd like to
store character data for your HTML5 game in a quick, scalable key-value store
like Amazon's DynamoDB.

Using Flask, you can pick and choose the technologies you'd like to interact
with, and freely mix and match them together without having any choices made for
you. When working on large projects, this is critical as more often than not,
separate pieces of your application will have completely separate requirements,
and therefore require different types of data storage.

​3. Maintainability

Assuming you've built your website as a composition of small Flask applications,
you will likely find that maintenance becomes a lot easier. Instead of having to
dig through a single monolithic codebase with thousands of lines of code, all
reliant upon one another--you can instead narrow your focus to the small web
service responsible for the functionality you'd like to change.

When working with a group of developers, this can be particularly useful as any
developer can easily jump into a small Flask codebase, find what they need to
change, and change it--without the risk that typically goes along with making
chnages to a large codebase:

-   Breaking related application code.
-

 

 

 

 

#### Tags

programming, python, flask

#### 6 views and 0 responses

  [Index]: ../../../index-2.html
  [Next]: ../../../posts/2012/10/no-regrets.html
  []: ../../../image/2012/10/45068134-kenpachi.jpg
  [Flask]: http://flask.pocoo.org/ "Flask"
  [Service Oriented Side Effects]: http://rdegges.com/service-oriented-side-effects
    "Service Oriented Side Effects"
  [Service Oriented Problems]: http://rdegges.com/service-oriented-problems
    "Service Oriented Problems"
  [1]: ../../../image/2012/10/45068432-flask.png
  [Django]: https://www.djangoproject.com/ "Django"
  [route]: http://flask.pocoo.org/docs/api/#flask.Flask.route "Flask route"
  [settings documentation]: https://docs.djangoproject.com/en/dev/topics/settings/
    "Django Settings Documentation"
  [2]: http://flask.pocoo.org/docs/config/ "Flask Configuration Documentation"
  [Flask extensions]: http://flask.pocoo.org/extensions/ "Flask Extensions"
