Author: Randall Degges
Date: 2010-03-02 06:00
Slug: pycall-14-released
Tags: programming, python, asterisk
Title: pycall 1.4 Released


Earlier today I received a patch from EvilZluk. The problem he pointed out to me
is that when moving the temporary call files to the Asterisk spooling directory,
if the spooling directory is on a separate device, an exception would be raised
as the `os.rename` function cannot support moving files across devices. In order
to resolve this issue, I instead used the `shutil.move` function which is safe
for cross-device file operations.

This can be extremely useful, especially in situations where there are NFS
mounts for the spooling directory shared across multiple boxes.

So if you're using pycall, please update your install and visit [pycall's
website][] for more information / examples!


  [pycall's website]: http://pycall.org/
