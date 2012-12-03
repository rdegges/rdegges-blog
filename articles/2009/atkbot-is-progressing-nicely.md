Title: atkbot is Progressing Nicely
Date: 2009-12-12 05:00
Author: Randall Degges
Tags: programming, irc


My IRC takeover bot, atkbot, is progressing nicely. Some of the features it now
has are:

-   Simple configuration file.
-   Can connect to IRC networks that are password protected.
-   Can join multiple channels by default (specified in the configuration file),
    or none at all (just lurk on the server).
-   Will automatically re-connect if kicked from the network, or if the internet
    goes down.
-   Can be instructed to join one or many IRC channels via PMs from bot
    operators.
-   Will maintain operator status once it has taken over a channel. EG: If it
    gets ops and kicks everyone out of the room (and bans them!), it will also
    actively watch for new people joining the channel and remove them as well.
-   Will auto op any bot operators who enter the channel (or who are already in
    the channel when the takeover happens).
-   Can be instructed to join all channels on the current IRC server.
-   Has a customizable part message which it will use when instructed to leave a
    channel.
-   Supports variable command prefixes for bot operators to use. You can put in
    any string: (~, !, etc.)
-   If the bot’s nick is already taken, it will automatically choose another
    nick.
-   Alert’s all bot operators via channel invite and private message when a
    channel is taken over, easily alerting the owners so they can take action.

Overall, I think atkbot is going pretty damn good. I have been casually working
on it for a few weeks now, and I’m liking it more and more. I still plan on
adding a lot of features, like more advanced takeover options, support for
multiple IRC networks, etc. — but I plan on building this into a working release
soon.

One of the things I’m currently debating is packaging. How should I package
atkbot? I’m open to suggestions.

The way I do my development uses GNUmake to build, and run the bot. I’m debating
myself on whether I should port atkbot to a systems-level type program (that
installs to a global path), or whether it should remain as a standalone program
which must be downloaded and ran from within its local directory structure.

Anyhow, development and progress will continue! If you have any input, I’d be
happy to have it.
