# Randall Degges

## This is an archived post This is an archived post

[Previous][]   [Index][]   [Next][]

### Tools I Use - tmux

January 7 2012, 5:34 PM  by Randall Degges

![][]

I love reading about tools other programmers use in their day-to-day existence.
There are so many great pieces of software out there that it's impossible to
hear about them all. Whenever I read articles (or watch screencasts) that other
developers make, I tend to learn a lot.

So, I decided to start my own series here on my site, dedicated to talking about
the tools I use, and how I use them. Unfortunately, I probably don't have much
useful information to give in this area--but I figure that if I start writing
about it, I'm bound to learn more about the tools I use along the way. So this
is a win-win for me.

As this is the first article in the series, I figured what better time to
discuss [tmux][], my favorite terminal multiplexer!

Terminal Multiplexers

If you've ever had the need to run multiple terminal windows inside an existing
terminal window, you've probably heard of [GNU screen][].

GNU screen is one of the most popular terminal multiplexers of all time. What it
allows you to do is have a large amount of virtual terminals that you can
create, move around, and use at will. What's great about tools like GNU screen
is that they allow you to quickly move between your terminal windows using
keyboard shortcuts, which can often times be a lot quicker than alt+tab'ing to a
new terminal window on your OS.

The other great thing about terminal multiplexers like GNU screen and tmux is
that they give you the power to tile your windows as you please. For instance,
you can run two terminals side by side, one window with code, and another with a
shell for testing your code on the command line. This can be a great help in
many situations.

When I first made the jump from using my OS terminal program to using GNU screen
a few years ago, my productivity shot up enormously, as I was able to quickly
move around through terminals like I never could before. I instantly started
writing code faster, finding problems quicker, and spending much less time
creating and moving terminal windows around on my desktop.

tmux - A Cool Terminal Multiplexer

While I was using GNU screen for a few years, I always had some issues with it
that didn't sit well with me. For one, while I had my GNU screen software highly
configured, I was still unable to perform some seemingly basic tasks like create
horizontally split windows. This meant that I could only open columns of
terminals, restricting the usefulness of my terminal.

Furthermore, GNU screen had a lot of complex keybindings that tended to make it
difficult for me to get used to. I remember spending quite a bit of time when
first learning GNU screen just memorizing the basics.

One frustrating day while I was struggling to adjust some screen keybindings, I
decided to look for alternatives. After a bit of research I stumbled upon tmux.
What a great day that was.

tmux, just like screen, is a terminal multiplexer. What's great about tmux is:

-   It's a lot simpler than GNU screen. It has sane defaults configured, and
    requires much less configuration to be useful.
-   It supports both vertical AND horizontally split windows.
-   It has a really awesome keybinding that allows you to magically rearrange
    your terminal windows in a variety of patterns. This is extremely useful for
    situations where you open multiple windows, and then want them to be moved
    to a decent looking pattern, but don't want to configure each window
    manually.
-   It has great documentation.

tmux in Action

A picture is worth a thousand words. I won't bore you any further, here are some
screenies of tmux in action.

This first picture is just a simple display of tmux with multiple windows open
(note the window names at the bottom):

![][1]

Next, we have a single tmux window open, broken into three panes: one is
vertically split, while the other is horizontally split. As you can see, this
makes coding quite convenient since I can code, look at tests, and browse
documentation all in the same window:

![][2]

Here I just hit a single key, and had tmux automatically re-arrange my panes a
few times in a row. Nice, huh?

![][3] ![][4] ![][5]

tmux - Configuration

One of the great things about tmux is that it requires almost no configuration
to be useful. Unlike GNU screen which requires quite a bit of tweaking (in my
opinion) to be useful, tmux has sane defaults out of the box.

Below is my **\~/.tmux.conf** file. As you can see, it's very simple:

[https://gist.github.com/1576911][]

The changes I made are simply to remap keys for my own preferences. Here's what
my changes do:

-   Set the tmux command key to CTRL+a (just like GNU screen). That means all
    tmux commands are prefixed with **C-a**.
-   To create vertical split windows, I setup keymappings for **C-a |**. To me,
    since the pipe key (|) looks like a vertical split, it makes sense to have
    that key open a new split window.
-   Likewise, I also bound the horizontal split key to **C-a -** (the dash key),
    since the dash key looks like a horizontal split.
-   The last few changes make moving between split windows easy. To navigate
    through windows, I assigned vim-like keybindings. EG:
    -   **C-a h** will move you one window to the left.
    -   **C-a j** will move you one window down.
    -   **C-a k** will move you one window up.
    -   **C-a l** will move you one window right.

As you can see, I had to make very few changes to tmux in order for it to be
useful to me.

tmux - Resources

If you'd like to give tmux a try, here are some resources to get you going. tmux
has become one of my favorite and most used tools since I started using it
earlier last year. I'd highly recommend it to anyone who does a lot of terminal
work.

-   [The main tmux website.][tmux]
-   **man tmux** - has great information and is highly readable.
-   [tmux crash course][], a really great introduction to using tmux.
-   [tmux][6], a series of blog posts explaining tmux in depth. A great read.

#### Tags

programming, linux

#### 31378 views and 20 responses

-   Jan 7 2012, 8:15 PM
    Steve Salkin responded:
    I use tmux every day and prefer to keep mostly the default settings. But one
    change I do make, and one I recommend to others, is to set the command
    prefix to C-\\ and make C-\\ C-\\ go to the last used window. C-\\ is one of
    the few keys that doesn't have a really useful existing binding in emacs,
    and if you've remapped caps lock as an additional control key, it's just a
    little stretch with the pinkies when you're on the home row. Giving up C-a
    start of line, C-b back one character, etc in the shell, emacs, and any
    readline program is just too much :)
-   Jan 7 2012, 9:16 PM
    Chris O'Donnell responded:
    It seems like you really like the screen navigation that tmux offers you. In
    the same vein, I would definitely recommend giving a tiling window manager a
    try. Awesome wm has, in my opinion, sane defaults and offers much of the
    functionality you seem to value.
-   Jan 7 2012, 11:28 PM
    ben responded:
    I've got a nice set of custom tmux config files you could check out here:\
    [https://github.com/lordnibbler/dotfiles][]
-   Jan 8 2012, 12:22 AM
    Randall Degges responded:
    @ben awesome dude. I'm watching your repo. I'm going to steal your
    knowledge! Thanks for sharing :)
-   Jan 8 2012, 12:40 AM

    ben responded:

    You might also want to check out tmuxinator -- lets you save tmux sessions,
    including SSH, and resume whenever.

    [https://github.com/aziz/tmuxinator][]

-   Jan 8 2012, 12:57 AM
    18bytes (Twitter) responded:
    You can do the same with a tiling window manger with less hassle, and more
    powerful.
-   Jan 8 2012, 1:43 AM

    virtualeyes responded:

    @18bytes, completely agree. Compiz on Linux and voila, snap to grid, move
    grid to desktop X, etc.

    The solution is in fact, to sell your Mac and setup a "real" \*nix system
    ;-)

-   Jan 8 2012, 2:39 AM
    n0mad responded:
    I've startet using byobu - which is less annoying than tmux.
-   Jan 8 2012, 4:03 AM

    ev responded:

    @18bytes, @chris: Tiling wms and tmux/screen does not serve the same
    purposes. The tiling wm will allow you to correctly handle local windows.
    While starting screen/tmux on a remote server will allow you to handle,
    organize and \_keep\_ remote sessions between connections. IMHO, both are
    needed to be 100% productive but if i'm forced to drop one it would be the
    tiling wm because I can't work without permanent (between reboot) remote
    sessions.

    @n0mad: byobu use either tmux or screen as a backend. And is really resource
    intensive compare to the two other, I don't recomment using it on a laptop.

-   Jan 8 2012, 7:44 AM
    Eric responded:
    Isn't C-a S the shortcut for horizontal split in GNU Screen? Does your
    version not have that feature?
-   Jan 8 2012, 12:20 PM

    Randall Degges responded:

    @Eric yeah--I just remapped the keys so that they'd be on more logical keys
    (in my opinion). For splitting, I prefer to use the | and - keys, since they
    look like the type of split I'm trying to make.

    For the movement keys, I prefer to use the familiar vim keys h, j, k, and l,
    since it feels natural to me, and I can navigate both vim splits and tmux
    splits the same way (just with different prefix keys).

-   Jan 8 2012, 12:20 PM

    Randall Degges responded:

    @Eric yeah--I just remapped the keys so that they'd be on more logical keys
    (in my opinion). For splitting, I prefer to use the | and - keys, since they
    look like the type of split I'm trying to make.

    For the movement keys, I prefer to use the familiar vim keys h, j, k, and l,
    since it feels natural to me, and I can navigate both vim splits and tmux
    splits the same way (just with different prefix keys).

-   Jan 10 2012, 10:51 PM
    Another Eric responded:
    Wow, this is great. My friend once told me I \*must\* use screen, and I
    could never get into it. You and your shortcuts have already got me humming
    on tmux, thanks!
-   Jan 16 2012, 12:44 AM
    E.A. Orak responded:
    I use terminator for the same purpose. tmux is also looks cool, but I
    suggest everyone to give a try to terminator.
-   May 8 2012, 5:16 AM
    Guillermo Álvarez responded:
    @virtualeyes Mac is more UNIX than linux. But far far away :-P
-   May 16 2012, 5:11 AM
    Bobby O responded:
    Hi RD. Thanks for the post. I really love the font you use in coding. Can
    you please tell the name of the font? Thanks in advance.
-   May 16 2012, 5:02 PM
    Randall Degges responded:
    @Bobby O It's just the standard Ubuntu desktop font, not sure what they have
    it set as :o
-   May 20 2012, 11:52 AM

    Bobby O responded:

    Hello RD, thanks for the response. But to know the name of the font, you
    could check from your terminal Edit -\> Profile Preference and under the
    Font: The name would be written there, hopefully..

    Would love to try it out. It looks really beautiful.

-   May 24 2012, 4:17 PM
    Randall Degges responded:
    Hey @Bobby O, it's Monospace (12pt) font. :o
-   Jul 2 2012, 10:17 AM
    andsens responded:
    I have fine tuned my tmux configuration quite a bit.\
    I removed all the default bindings, because -intuitively- they do not make
    any sense to me. So I rebound most of them.\
    Check it out: [https://gist.github.com/3034376][]\
    Theres stuff like alt+\<cursor\> to switch a window left.\
    | to split a window vertically\
    \_ to split it horizontally\
    ctrl+A followed by alt+\<cursor\> to resize panes\
    ctrl+A follow by \< or \> to swap windows left and right.

  [Previous]: ../../../posts/2012/01/devops-django-part-5-using-heroku-in-producti.html
  [Index]: ../../../index-3.html
  [Next]: ../../../posts/2012/01/going-to-war.html
  []: ../../../image/2012/01/36475083-tools.png
  [tmux]: http://tmux.sourceforge.net/ "tmux"
  [GNU screen]: http://www.gnu.org/software/screen/ "GNU screen"
  [1]: ../../../image/2012/01/36475535-tmux_simple.png
  [2]: ../../../image/2012/01/36475600-tmux_multiple_panes.png
  [3]: ../../../image/2012/01/36475885-tmux_auto_arrange_1.png
  [4]: ../../../image/2012/01/36475888-tmux_auto_arrange_2.png
  [5]: ../../../image/2012/01/36475892-tmux_auto_arrange_3.png
  [https://gist.github.com/1576911]: https://gist.github.com/1576911
  [tmux crash course]: http://robots.thoughtbot.com/post/2641409235/a-tmux-crash-course
    "tmux Crash Course"
  [6]: http://blog.hawkhost.com/2010/06/28/tmux-the-terminal-multiplexer/
    "tmux series"
  [https://github.com/lordnibbler/dotfiles]: https://github.com/lordnibbler/dotfiles
  [https://github.com/aziz/tmuxinator]: https://github.com/aziz/tmuxinator
  [https://gist.github.com/3034376]: https://gist.github.com/3034376
