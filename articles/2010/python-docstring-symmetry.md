# Randall Degges

## This is an archived post This is an archived post

[Previous][]   [Index][]   [Next][]

### Python Docstring Symmetry

August 16 2010, 7:00 PM  by Randall Degges

If you’ve been doing python for more than a month, then I’m sure you’re familiar
with [PEP-8][], the *official* python style guide. If you look at PEP-8, it
doesn’t explicitly define any docstring style guidelines, but instead recommends
following [PEP-257][]’s rules.

In the python community, it is considered a sin if you don’t strictly follow
PEP-8. This is one of the things I really enjoy about python, the community
helps encourage best practices, and good coding style. Nothing wrong with that.

However, I hate to say it, but: **PEP-257 is bullshit**.

## What I Like About PEP-257

I don’t hate *everything* about PEP-257, so before I get to the bad part, let me
start by talking briefly about what I like.

-   PEP-257 encourages developers to write docstrings for all modules,
    functions, and classes. This is awesome. I totally agree. Docstrings are
    extremely useful for developers, and help show off some of python’s awesome
    self-documenting and introspection capabilities.

-   PEP-257 encourages the use of triple double quotes (“”“) as docstring
    delimiters. I agree again. It is nice to have consistency across multiple
    programs.

-   PEP-257 suggests that very short one line docstrings be placed on a single
    line, but still use triple double quotes (“”“) as delimiters. Again, this
    rocks. Let’s say you’re writing a private method for a class, and it is
    pretty self explanatory, don’t kill yourself writing long docstrings:

        def _something_simple(x, y):
            """Adds x and y."""
            return x+y

-   PEP-257 says that you should NOT put your function signatures into your
    docstring, as they are already available through introspection. Makes
    perfect sense: don’t be redundant:

        # Don't do this, please.
        def stupid_function(a):
            """stupid_function(a) -> int"""
            return 4

So, as you can (hopefully) see, I’m not some crazy rebel who hates standard
conventions or anything. I love order just as much as the next programmer.

## Where PEP-257 Goes Crazy

The main bulk of PEP-257 is describing how multi-line docstrings should look,
and this is where things get ugly.

Basically, PEP-257 wants your multi-line docstrings to start immediatley after
the opening triple quotes (“”“), and end with a blank line. Here’s an example:

    # This example was stolen directly from PEP-257.
    def complex(real=0.0, imag=0.0):
        """Form a complex number.

        Keyword arguments:
        real -- the real part (default 0.0)
        imag -- the imaginary part (default 0.0)

        """
        ...

Here’s another example with a more full docstring:

    def do_something(x=0.0, y=5):
        """Calculates your best friend's birthday by multiplying two numbers
        together, then returning 4.

        Keyword Arguments:
        x -- anything, I don't care
        y -- seriously

        """
        ...

Now, like most programmers I know, I’m a bit of an organizational freak. I like
symmetry in my code, comments, and I have to have things perfectly aligned.
PEP-257’s suggested multi-line docstring format drives me crazy.

I have two big problems with PEP-257’s suggested format:

1.  PEP-257 wants me to start my docstring **immediately** after the triple
    quotes.

2.  PEP-257 wants me to leave a blank line at the end of my docstring.

It doesn’t look *clean* or *organized* putting your comments immediately after
the opening triple quotes. It also seems unnecessary to add an extra blank line
at the end of each docstring.

## What I Propose

Instead of listening to PEP-257, I suggest that pyton developers instead adapt
my style:

    def complex(real=0.0, imag=0.0):
        """
        Forms a complex number.

        :param real:    The real part (default 0.0).
        :param imag:    The imaginary part (default 0.0).
        :return:        An imaginary number, or False.
        """
        ...

This takes up the same amount of lines, except looks more symmetrical. We now
have an uncluttered docstring, that is easier to scan over with your eyes, and
looks nicer.

The description of what you’re documenting goes on the line directly below the
opening triple quotes, and there is no blank line before the end of the
docstring.

## Thoughts?

I will personally continue to keep using my own style for docstrings, but I’d
like to know what you think. Am I crazy? Is PEP-257 crazy? Do you not care?

#### Tags

programming, python

#### 4715 views and 4 responses

-   Aug 17 2010, 12:05 AM

    switchology (Twitter) responded:

    I am like you with multiline comments.

    However 257 is reasonable, it wants your comment to start immediately after
    the triple quote for the purpose of avoiding unnecessary heading whitespace.
    So when other components of your application query the doc string of an
    element it is more predictable.

    I still put a new line in though.

-   Aug 17 2010, 12:53 AM
    Randall Degges responded:
    I get that PEP-257 is reasonable with the whitespace in the heading thing,
    but since PEP-257 also suggests parser code for displaying docstrings
    properly ([http://www.python.org/dev/peps/pep-0257/\#id20][]), which already
    remove the leading newline character, it seems silly to still recommend
    putting text directly after the triple quotes.
-   Sep 28 2012, 8:58 AM

    Blah responded:

    PEP-257 states:

    """Multi-line docstrings consist of a summary line just like a one-line
    docstring, followed by a blank line, followed by a more elaborate
    description.[...] The summary line may be on the same line as the opening
    quotes or on the next line."""\
     \
    Was that added after this blog post or something, or was it just missed, or
    am I missing something? (maybe just the fact multi-line examples in PEP-257
    don't use the next line... but even so the next line style is IMO clearly
    permitted by the above)

    And since when you run such a docstring through the provided PEP257 trim()
    function (also, where the hell is that in the stdlib? textwrap.dedent()
    /almost/ does it, but messes up the first line), it picks up such a
    next-line summary line fine, so I think it's fine to use next-line.

    Regarding the ending extra blank line, that's indeed a bit weirder. The
    rational given is dubious, too - firstly emacs has other fill (emacs jargon
    for word wrapping) functions to restrict to region so it's something of a
    case of "doctor it hurts when I...then don't...", and secondly you could
    mess about with the paragraph-start and/or paragraph-separate emacs settings
    so that fill-paragraph won't mess it up anyway.

-   Nov 17 2012, 8:22 AM

    AndrewBC responded:

    I not only completely agree with you, I settled on the exact same convention
    you have independently.

    I'm not going to alter a logical and readable format for the sake some
    specific tool in some specific editor that people somehow can't work around.
    If you want no leading whitespace, remove it before you use whatever tool
    you desire. Same goes for desiring trailing whitespace, and adding it.

    Isn't this language supposed to be about readable source code, and not
    kowtowing to source wrangling tools that think "Ugh, it's too hard!"?

  [Previous]: ../../../posts/2010/08/asterisk-expression-truthiness.html
  [Index]: ../../../index-6.html
  [Next]: ../../../posts/2010/08/how-to-streamline-asterisk.html
  [PEP-8]: http://www.python.org/dev/peps/pep-0008/
  [PEP-257]: http://www.python.org/dev/peps/pep-0257
  [http://www.python.org/dev/peps/pep-0257/\#id20]: http://www.python.org/dev/peps/pep-0257/#id20
