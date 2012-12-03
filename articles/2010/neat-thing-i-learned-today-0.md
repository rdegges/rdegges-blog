Title: Neat Thing I Learned Today
Date: 2010-01-08 04:00
Author: Randall Degges
Tags: c, programming


In C, you can index arrays with either the array subscript inside of the typical
square brackets `arr[0]` or with the less common (and somewhat mistifying):
`0[arr]`.

Now, I’ve been using C for quite a while, but never realized that you could do
this. It is definitely awesome, and could be really useful in some obfuscated
programs. I wrote a really small sample program that runs a test (if it doesn’t
work, please leave a comment with your compiler as I’d be interested to know if
it doesn’t work on all platforms:

``` c
/*
 * This example program demonstrates a weird way to reference array
 * elements in C. All credit goes to the Stack Overflow 'Strangest language
 * feature' discussion page:
 * http://stackoverflow.com/questions/1995113/strangest-language-feature
 */

#include <stdio.h>

int main(int argc, char **argv) {

    int arr[10];

    arr[0] = 1;
    printf("arr[0] = %d\n", arr[0]);
    printf("0[arr] = %d\n", 0[arr]);

    return 0;

}
```

The really interesting thing about this, is why it works. On the Stack Overflow
discussion page for Strangest language feature, the user Michel explains why it
works.

> … that’s because a[10] means *(a+10) … and 10[a] means*(10+a) :)

Pretty awesome. Anyhow, hope you learned something new, I did. Thanks Stack
Overflow :)
