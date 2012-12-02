Title: New Project atkbot for IRC Takeovers
Date: 2009-12-05 06:00
Author: Randall Degges
Tags: programming, irc


A little over a week ago I started working on a new project for fun in my
freetime: [atkbot][].

atkbot is a cross platform IRC bot written in Java, designed to takeover IRC
channels.

Those of you who know me know that I’ve been using IRC forever. IRC (if you
aren’t aware) stands for internet relay chat. It’s a pretty old protocol for
chatting (in text) with friends. Tons of people still use IRC today, and it is a
popular way to communicate with people.

IRC bots are programs which simulate a person, and provide certain functionality
to IRC users. Most IRC bots are trivia games, or chat room monitoring tools, but
there are also takeover bots which try to take over chat rooms and remove users
from them. And this is the goal of atkbot.

atkbot doesn’t make use of any exploits, or anything like that, it simply joins
IRC channels, and waits to be given operator status. Once it is given operator
status, it bans and kicks all users from the room, sends channel invites to the
bot operators (specified in the bot config files), and then private messages
them as well to let them know that the channel has been taken over.

atkbot is cross-platform (will run on anything with Java), and is going to be
highly configurable. I’m currently cleaning up the development code, and plan on
pushing a first release at some point this weekend. The configs are simple and
straight forward (see the config file for information), and should be completely
independent of any other system packages.

I decided to write atkbot for fun as a little side project to make me laugh a
bit. Please don’t use it for anything truly evil :)

I’ll post some updates once the first version is released. If you have any
suggestions or features that you think should be added, please let me know!

  [atkbot]: http://github.com/comradeb14ck/atkbot
