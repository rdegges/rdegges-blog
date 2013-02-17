# Randall Degges

## This is an archived post This is an archived post

[Previous][]   [Index][]   [Next][]

### pycall 1.4 Released

March 1 2010, 10:00 PM  by Randall Degges

Earlier today I received a patch from EvilZluk. The problem he pointed out to me
is that when moving the temporary call files to the Asterisk spooling directory,
if the spooling directory is on a separate device, an exception would be raised
as the `os.rename` function cannot support moving files across devices. In order
to resolve this issue, I instead used the `shutil.move` function which is safe
for cross-device file operations.

This can be extremely useful, especially in situations where there are NFS
mounts for the spooling directory shared across multiple boxes.

So if you’re using pycall, please update your install and visit [pycall’s
website][] for more information / examples!

#### Tags

programming, python, asterisk

#### 412 views and 0 responses

  [Previous]: ../../../posts/2010/03/transparent-telephony-part-3-making-and-recei.html
  [Index]: ../../../index-7.html
  [Next]: ../../../posts/2010/02/transparent-telephony-part-2-installing-aster.html
  [pycall’s website]: http://pycall.org/
