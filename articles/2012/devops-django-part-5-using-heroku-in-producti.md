# Randall Degges

## This is an archived post This is an archived post

[Previous][]   [Index][]   [Next][]

### DevOps Django - Part 5 - Using Heroku in Production

January 8 2012, 7:38 PM  by Randall Degges

Hi there, I'm writing a series of articles which discuss some Django deployment
practices I've discovered. If you're new to the series, I'd recommend reading
[DevOps Django - Part 1 - Goals][], and then working your way back to this post,
since each article builds on the ones before it.

In the [last article][] of the series, I discussed how I made the decision to
swap my company's entire infrastructure over to Heroku. In this article, I'll be
describing how I actually moved from Rackspace to Heroku, and what I learned
along the way.

A Brief Note

After I decided to move my company's infrastructure to Heroku, I knew I had my
work cut out for me. Even though I'd already had ample experience with Heroku,
having ported my work's entire production Django application to run on it in
only an hour or so, I still thought I'd have a hard time actually making the
transition from Rackspace to Heroku.

Assuming I'd have a difficult time making the transition, I reserved an entire
work week for the task. I made plans to spend Monday through Friday preparing
our new production Heroku instances, and then do rigorous testing afterwards
before finally switching the DNS and making the leap.

Luckily for me, this was not the case, and the entire process was actually a
walk in the park.

Managing the Heroku App

The first thing I actually did to start the process was to create a company
Heroku account.

If you're at all familiar with [Github][], you're likely familiar with the
concept of organizations. Github organizations are fantastic. Essentially, if
you have a Github account (eg: [rdegges][]), you can create as many
"organizations" as you like. Each organization has their own billing
information, their own repositories, and their own URL namespace. Furthermore,
organizations can have multiple members (other Github users). Github
organizations are, in my opinion, the best executed group management solution
for programming teams.

The way Heroku manages permissions is, unfortunately, not quite as good. Heroku
has no concept of organizations like Github, which means that developers (like
me) tend to need direct access to the company account (email and password), in
order to make changes to the application.

While Heroku does allow you to add "collaborators" to an application,
collaborators are only allowed to push code, not make any changes that can
affect billing (eg: adding a memcached server, resizing a database, etc.).

In mid to large companies, this may not be a problem, since it is likely that
developer should never have access to manage infrastructure components (resizing
dbs, etc.). In my situation, however, working at a small company with only a few
people, this makes it somewhat annoying to use.

As a helpful workaround for the permissions issue, I found a cool open source
application called [heroku-accounts][], which is maintained by a Heroku
employee. heroku-accounts allows you to easily switch between Heroku accounts on
the CLI--this is great, because it means that you can use your company account
while working on your company projects, then immediately swap back to your
personal account for your personal projects--with no overlap.

To actually setup my project's Git repository and get a Heroku app created, I
did the following:

 

While the Heroku group management solution is not perfect, heroku-accounts makes
the process a whole lot easier.

Provision a Database

After getting my Git r

Preparing the Django Project

 

The actual process of modifying my Django code to run on Heroku was surprisingly
simple. To make things as clear as possible, I'll break this section up into
sub-sections, clearly explaining each change I made along the way.

Step 1 - Check Your Requirements

As I've mentioned before, Heroku automatically manages your python requirements
for your project, so that you don't have to. To make this work, Heroku requires
you to have a top-level **\`\`requirements.txt\`\`** file defined, which
contains a list of your python requirements in standard **pip** format.

Since most Django projects use a **\`\`requirements.txt\`\`** file anyhow
(including my work project), I didn't have to change anything here:

[https://gist.github.com/1827824][]

**NOTE:** As you can see in the gist above, I always define the exact versions
of my required python packages. This is good for several reasons:

1.  You always know exactly which versions of which packages are required for
    your project to run as expected.
2.  Heroku will always install the versions you define, ensuring your project
    won't break randomly.
3.  Having versions defined allows Heroku to update your packages when you
    specify new version numbers. If you have "**Django==1.3**" defined (and
    running on Heroku), then change your file to read "**Django==1.3.1**", when
    you push your code to Heroku it'll detect the version change and update the
    package for you automatically.

Step 2 - Change Your Database Settings

Since you're going to be using 

 

#### Tags

programming, devops, django, heroku

#### 37 views and 0 responses

  [Previous]: ../../../posts/2012/01/dogs-are-great-programming-companions.html
  [Index]: ../../../index-3.html
  [Next]: ../../../posts/2012/01/tools-i-use-tmux.html
  [DevOps Django - Part 1 - Goals]: http://rdegges.com/devops-django-part-1-goals
    "DevOps Django - Part 1 - Goals"
  [last article]: http://rdegges.com/devops-django-part-4-choosing-heroku
    "DevOps Django - Part 4 - Choosing Heroku"
  [Github]: https://github.com/ "Github"
  [rdegges]: https://github.com/rdegges "My Github Account"
  [heroku-accounts]: https://github.com/ddollar/heroku-accounts
    "heroku-accounts"
  [https://gist.github.com/1827824]: https://gist.github.com/1827824
