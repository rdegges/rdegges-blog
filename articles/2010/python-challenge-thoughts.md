% Python Challenge Thoughts
%
%

# Python Challenge Thoughts

If you didn’t read my previous post, you’ll want to check that out first: [The
Purely Functional Python Brainfuck Challenge][].

So, today was the first day I really invested some thought and time into the
challenge. I decided that before just diving into code I’d spend some time
reading up on functional programming in python, and learn a bit.

The first thing I read through was the official python functional programming
page which you can find [here][]. This had a bunch of useful information
regarding functional programming tools in python, but didn’t really have any
examples of *actual* functional programs.

I learn by reading, and code samples are very helpful to me, so today I spent a
while looking for some better functional programming docs, and found an
extremely useful series: [Charming Python: Functional Programming in Python][]
(written by IBM engineers) which not only has code samples, but also contains a
list of tricks and hints for building purely functional programs in python! Just
what I needed \>:)

I learned about the python [ternary operator trick][] yesterday, and used that
to generate my `main()` function like so:

    (__name__ == '__main__' and main())

Which is a *purely* functional way to check for the program being ran, and then
call our `main` function. But what if the program is included instead of ran? I
doubt anyone would do this, but just incase, I want to print a message. The way
I could implement this (logically) would be to do something like:

    (__name__ == '__main__' and main()) or print 'Run me!'

But, unfortunately, python won’t let you use print (as it doesn’t return
anything) in that context. So, the way I eventually figured it out was to use
`sys.stdout.write`, as you can see here:

    (__name__ == '__main__' and main()) or sys.stdout.write('Run me!')

Which returns a value, and therefore is acceptable to use in a ternary operation
like the one above.

That’s all for today, but tomorrow I’ll work on impelementing functional file
reading and parsing, and I’ll make another post detailing my findings.

Best luck to all challengers!

  [The Purely Functional Python Brainfuck Challenge]: http://projectb14ck.org/2010/07/05/the-purely-functional-python-brainfuck-challenge/
  [here]: http://docs.python.org/howto/functional.html
  [Charming Python: Functional Programming in Python]: http://www.ibm.com/developerworks/library/l-prog.html
  [ternary operator trick]: http://neverfear.org/blog/view/134/Ternary_operator_in_Python
