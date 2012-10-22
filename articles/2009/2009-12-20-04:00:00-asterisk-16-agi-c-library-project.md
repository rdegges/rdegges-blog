# Asterisk 1.6 AGI C Library Project


I'm announcing my Asterisk 1.6 C AGI library for developers [(cAGI)][cagi]. cAGI
is a fully compliant Asterisk 1.6 AGI library for C developers. It can be used
in production (I've been using it on several production boxes for the past 5
months), and has been generally well tested.

cAGI was originally part of my [WildPBX][wildpbx] project, but I've decided to
release it as an individual project to help speed up development, and loosely
couple the entire WildPBX project.

Currently, I'm cleaning up the cAGI code base, and building a good Makefile for
installing the library (so that the includes can be used from anywhere). It will
be ready soon, I promise!

Stay tuned for more updates :)


[cagi]: http://github.com/comradeb14ck/cagi "cAGI"
[wildpbx]: http://github.com/comradeb14ck/wildpbx "WildPBX"
