Title: Service Oriented Development
Date: 2012-07-12 22:47
Author: Randall Degges
Tags: programming


![Skeleton][]

As time goes by, I find myself writing more and more of my projects in a service
oriented manner.

Surprisingly, I haven't spent much time reflecting on this process, as the shift
from monolithic codebases to service oriented codebases happend gradually and
naturally. What I'd like to do in this article, is explain my development
journey which eventually led me to writing service oriented applications.


## Background

I started writing web applications somewhere around 6 years ago. At the time
(and long before), I was primarily interested in backend development--writing
servers, command line utilities, and helper programs that had nothing to do with
HTTP.

It was around this time that I started to become interested in actually writing
user facing sites, so I started the way any backend developer with no web
experience would: hacking together PHP CGI scripts.

After suffering through what felt like a needlessly complicated development
model, I eventually discovered the MVC pattern, and began using the [Django][]
web framework. Compared to its CGI predecessors, Django felt like the right way
to do things.


## It Feels Good

 After writing several small hobby projects in Django, I was ready to start
using it for more serious matters.

The first major Django project I built (with a significant number of users) was
a port of an old PHP site that my work used.


  [Skeleton]: http://getfile5.posterous.com/getfile/files.posterous.com/temp-2012-07-12/oxknkiuBCezoBBzkwrzGzpmhEJyEIBJGFCfijwqJvcjCcEyGJCrljpxDkqjy/skeleton.jpg.scaled696.jpg
  [Django]: https://www.djangoproject.com/ "Django"
