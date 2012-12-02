Title: Asterisk Git Mirrors
Date: 2010-08-22 08:30
Author: Randall Degges
Tags: programming, asterisk, git


If you’re a developer in the telephony scene, you probably use and work with
[Asterisk][], at least a little bit. Asterisk is the largest open source PBX
system available. It’s written in C, has numerous modules, books, and plenty of
other material built around it.

The Asterisk development team uses SVN for their source code management. As a
developer myself, who frequently needs to read the source, make changes, and
submit patches, this is somewhat inconvenient as I prefer to use git for all my
source code management.

Using git gives me the ability to locally reference all logs, patches, and
previous project history without ever talking back to the very slow central SVN
SVN repository.

So today I created several unofficial git repositories to mirror the SVN
repositories for Asterisk and some related projects. I put the new git
repositories up on [Github][], the most popular git hosting service, in hopes
that other git developers may find them useful. Github in particular makes
social coding extremely easy (and fun), so I’d encourage any of you reading this
post to follow the projects on Github and stay up to date with Asterisk
development.

**All Asterisk git repos will be updated every 30 minutes.** I went ahead and
provisioned a dedicated [linode][] server for the mirroring scripts to run on,
so there should be no issues maintaining the mirrors over time.

Here’s a list of the Asterisk repos that are now mirrored, and the links to
their respective Github repositories:

-   [Asterisk][1]
-   [asterisk-addons][]
-   [DAHDI][]
-   [libpri][]
-   [libss7][]

These projects were chosen as they are the most popular Digium open source
projects, which seem to have the largest developer following and most active
development teams.

My hope is that more developers, whether they use SVN or git, will be able to
participate in Asterisk development and help move the projects forward!

  [Asterisk]: http://www.asterisk.org/
  [Github]: https://github.com/
  [linode]: http://www.linode.com/?r=b35609c0093d8d027dc678ee6169b3cd78118be1
  [1]: http://github.com/comradeb14ck/asterisk
  [asterisk-addons]: http://github.com/comradeb14ck/asterisk-addons
  [DAHDI]: http://github.com/comradeb14ck/dahdi
  [libpri]: http://github.com/comradeb14ck/libpri
  [libss7]: http://github.com/comradeb14ck/libss7
