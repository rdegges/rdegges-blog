Title: pycall 1.3 Released
Date: 2010-02-24 04:00
Author: Randall Degges
Tags: programming, python, asterisk


Earlier today I received an email from [Marcelo Araujo][] which contained a
patch for [pycall][] to add support for local trunks. So I patched pycall up,
updated the changelog, the builds, and pushed everything out.

The problem with the earlier versions of pycall is that they didn’t properly
support local trunks. In Asterisk, local trunks allow you to dial numbers
locally to your system, which is useful for people who want to let their
Asterisk dial plan configurations control the dialing logic of the call. I
completely overlooked that when I was first writing pycall. The reason why
(specifically) it didn’t work before was that pycall would build all dial
strings in this format `TRUNK_TYPE/TRUNK_NAME/NUMBER` so that the Channel
directive of the call file would look something like
`Channel: Local/internal/18882223333` which works great for SIP, ZAP, DAHDI,
IAX2, and other trunks—but not for local! Local trunks have to be written in the
following manner to work `Channel: Local/18882223333@internal`.

So thanks again to Marcelo for helping find and fix that annoying issue! And to
anyone else out there who finds issues with pycall, please let me know!

  [Marcelo Araujo]: mailto:marcelo@midivts.org
  [pycall]: http://pycall.org/
