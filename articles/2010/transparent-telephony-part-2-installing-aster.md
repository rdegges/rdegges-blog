Author: Randall Degges
Date: 2010-03-01 06:00
Slug: transparent-telephony-part-2-installing-asterisk
Tags: asterisk, linux
Title: Transparent Telephony Part 2, Installing Asterisk


Welcome back to the Transparent Telephony series. If you’re new, you may want to
check out part 1 here: [Transparent Telephony – Part 1 – An Introduction][].

This series is designed for technical people, programmers, and just general
enthusiasts who want to learn: how telephony works, how to setup your own phone
server (PBX), how to write telephony applications, and how to reduce your phone
expenses. There are tons of neat things you can do with telephony knowledge, so
keep reading!

This article will walk you through installing [Asterisk][] on your [CentOS][] or
[Ubuntu][] server. If you are going to install on a virtual machine to follow
along, I recommend using [VirtualBox][], as everything should work out of the
box. Certain virtual machine programs like [Xen][] have kernel issues which
makes installing Asterisk difficult. Also, never ever use Asterisk in production
on a virtual machine! You’ll have timing issues (this will be explained later in
the series).

Before we get started, go ahead and install either CentOS or Ubuntu on your
system. Update everything to the latest version. I’ll be showing you how to
install the latest version of Asterisk stable, so we might as well update our
operating system and packages.


## Installing Dependencies


### CentOS

You can skip this step as [Digium][] provides yum repositories which will
automatically install the latest version of Asterisk for us as well as any
dependencies.


### Ubuntu

First of all, let’s install the appropriate kernel sources. We will need these
to build against. To figure out what kernel version you’re using, run the
`uname -a` command.

Next, you’ll want to search for the appropriate kernel packages to install,
`apt-cache search 2.6.31` (where 2.6.31 is your kernel version). Once you get a
list of packages, install the appropriate linux-headers, linux-image, and
linux-source packages that correspond to your kernel version. For me this was:
`apt-get install linux-headers-2.6.31-14-generic linux-image-2.6.31-14-generic linux-source-2.6.31`.
Now install the SSL libraries required for encryption. We’ll also install SSH so
that you can remotely administrate your server:
`apt-get install openssl libssl-dev ssh`.

Next we’ll install all of the tools required to build Asterisk:
`apt-get install gcc g++ make automake autoconf build-essential bison flex libtool libncurses5 libncurses5-dev libgsm1 libgsm1-dev libnewt-dev libnewt-pic curl libcurl3 tclcurl libwww-dev mysql-common mysql-client libmysqlclient16 libmysqlclient16-dev sox libsox-fmt-all madplay libxml2 libxml2-dev doxygen`.

You may also want to install ntp as it’s a safer alternative to ntpdate which
most system administrators use to adjust the system time. DAHDI, the Digium
Asterisk Hardware Device Interface, is very picky about system time, and has
been known to crash servers which experience large time corrections. ntp will
prevent this from happening as it will periodically adjust the time in small
increments which is much safer than performing a single large time adjustment
with ntpdate.

If you’d like to install ntp, just run: `apt-get install ntp`.


## Installing Asterisk

Now that we’ve got our dependencies worked out, it’s time to actually install
Asterisk! We’ll install DAHDI, Asterisk, Asterisk Addons, and all of the
Asterisk default sound files. We’ll also ensure that Asterisk starts at boot,
and runs as a system service.


### CentOS

As a disclaimer, these instructions have been largely taken from Digium’s
website. They have excellent documentation for installing Asterisk using their
yum repositories which I will be mirroring here.

1.  Download the centos-asterisk.repo file and copy it to /etc/yum.repos.d/.
    Here is the repo file:

        ```
        [asterisk-current]
        name=CentOS-$releasever - Asterisk - Current
        baseurl=http://packages.asterisk.org/centos/$releasever/current/$basearch/
        enabled=1
        gpgcheck=0
        #gpgkey=http://packages.asterisk.org/RPM-GPG-KEY-Digium
        ```

2.  Download the centos-digium.repo file and copy it to /etc/yum.repos.d/. Here
    is the repo file:

        ```
        [digium-current]
        name=CentOS-$releasever - Digium - Current
        baseurl=http://packages.digium.com/centos/$releasever/current/$basearch/
        enabled=1
        gpgcheck=0
        #gpgkey=http://packages.digium.com/RPM-GPG-KEY-Digium
        ```

3.  Clear your current yum cache: `yum clean all`.

4.  Install Asterisk and its dependencies:
    `yum install asterisk16 asterisk16-configs asterisk16-voicemail dahdi-linux dahdi-tools libpri`.

5.  Make Asterisk and DAHDI start at boot. Then reboot the server to make sure
    they both start up: `chkconfig dahdi on; chkconfig asterisk on; reboot;`.

Once the system comes back up, run the following command to make sure DAHDI and
Asterisk are working properly:
`service dahdi status; asterisk -rx 'core show version'`.

If all is working, you should see something similar to:

```
### Span 1: DAHDI_DUMMY/1 "DAHDI_DUMMY/1 (source: Linux26) 1" (MASTER)
Asterisk 1.6.0.25 built by root @ localhost.localdomain on a i686 running Linux on 2010-02-26 20:30:00 UTC
```


### Ubuntu

To install Asterisk on Ubuntu, we need to compile it from source.

1.  First of all, download all of the source files from [the Asterisk download
    page][]. Always download the latest release. The files you will need are
    Asterisk, Asterisk-Addons, DAHDI Linux / DAHDI Tools, and Libpri. The code
    below shows what Asterisk release I’ll be downloading (current at the time
    of writing):

    ``` bash
    wget http://downloads.asterisk.org/pub/telephony/asterisk/releases/asterisk-1.6.2.5.tar.gz
    wget http://downloads.asterisk.org/pub/telephony/asterisk/asterisk-addons-1.6.2.0.tar.gz
    wget http://downloads.asterisk.org/pub/telephony/dahdi-linux-complete/releases/dahdi-linux-complete-2.2.1+2.2.1.tar.gz
    wget http://downloads.asterisk.org/pub/telephony/libpri/releases/libpri-1.4.10.2.tar.gz
    ```

2.  Now, let’s install DAHDI Linux / DAHDI Tools. Always install this before
    Asterisk:

    ``` bash
    tar zxvf dahdi-linux-complete-2.2.1+2.2.1.tar.gz
    cd dahdi-linux-complete-2.2.1+2.2.1
    make
    make install
    make config
    ```

3.  Now we’ll install Libpri:

    ``` bash
    tar zxvf libpri-1.4.10.2.tar.gz
    cd libpri-1.4.10.2/
    make
    make install
    ```

4.  Now that we’ve got both DAHDI and Libpri installed, let’s install Asterisk:

    ``` bash
    tar zxvf asterisk-1.6.2.5.tar.gz
    cd asterisk-1.6.2.5/
    ./configure
    make
    make install
    make config
    make samples
    make progdocs
    ```

5.  Finally, we’ll install Asterisk-Addons which includes some useful
    functionality:

    ``` bash
    tar zxvf asterisk-addons-1.6.2.0.tar.gz
    cd asterisk-addons-1.6.2.0/
    ./configure
    make
    make install
    make samples
    ```

Now that we’ve got everything installed, go ahead and reboot the system to make
sure that everything starts up automatically on boot. Once the system is back up
and running, run the following two commands:
`/etc/init.d/dahdi status; asterisk -rx 'core show version'`.

If all is working, you should see something similar to:

```
### Span  1: DAHDI_DUMMY/1 "DAHDI_DUMMY/1 (source: HRtimer) 1" (MASTER)
Asterisk 1.6.2.5 built by root @ ubuntu on a i686 running Linux on 2010-02-28 22:30:53 UTC
```


## Conclusion

Now that you’ve got DAHDI, Libpri, Asterisk, and Asterisk-Addons installed, your
server is ready for production usage!

Our next article will cover configuring Asterisk and making calls to the outside
world. Be sure to subscribe to our RSS feed to get notified when our next
instalment is released!

If you’re looking for some other great Asterisk documentation, you can’t do any
better than [Asterisk: The Future of Telephony, 2nd Edition][]. It’s the most
thorough book on Asterisk that you can find, and will teach you everything you
need to know to get started with Asterisk.


  [Transparent Telephony – Part 1 – An Introduction]: http://neverfear.org/blog/view/80/Transparent_Telephony_Part_1_An_Introduction
  [Asterisk]: http://www.asterisk.org/
  [CentOS]: http://centos.org/
  [Ubuntu]: http://www.ubuntu.com/
  [VirtualBox]: http://www.virtualbox.org/
  [Xen]: http://xen.org/
  [Digium]: http://www.digium.com/en/
  [the Asterisk download page]: http://www.asterisk.org/downloads
  [Asterisk: The Future of Telephony, 2nd Edition]: http://www.amazon.com/gp/product/0596510489?ie=UTF8&tag=projectb14ck-20&linkCode=as2&camp=1789&creative=390957&creativeASIN=0596510489
