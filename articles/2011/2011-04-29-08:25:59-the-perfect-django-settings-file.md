% The Perfect Django Settings File
%
%

# The Perfect Django Settings File

I know this isn't the best way to start an article, but *I lied*. This article
won't show you how to make the perfect Django settings file. Instead, it will
show you how to build the perfect Django settings *module*.

When I'm first starting a new Django project, I like to make sure that my
settings file(s) are crafted in an organized, ideal manner. My settings file(s)
should allow me to:

-   Maintain as many specific deployment environment settings as I choose in a
    clear and separate manner. For example: *production*, *staging*,
    *development*, etc.
-   Maintain common project settings that are used by all specific deployment
    environments. For example: maybe *production*, *staging*, and *development*
    all share a common **ADMINS** setting. I don't want to duplicate this code
    in all of my enviornment specific settings files.
-   Easily test and deploy code to each specific environment.

Old Habits Die Hard

I'd like to quickly discuss why I think most people design their settings
file(s) wrong.

The approach to settings that I see many people take is simplistic--they'll
define all of their values in their settings.py file, and then at the bottom of
the file write something like this:

~~~~ {.line_numbers}
1
2
3
4
~~~~

try:

    from settings\_local import \*

except ImportError:

    pass

What sucks about this approach is that you now need to maintain a file--in this
case **settings\_local.py** for each of your environments, and it isn't going to
be easy to version control. Why? Because you have two choices in this scenario
of fail:

1.  Don't version control the **settings\_local.py** files on your various
    servers. This sucks because now you've got stuff like database credentials
    that will lay around, and gradually break future deployments when you forget
    to update them. It also sucks because you've got to constantly worry about
    that file. It isn't part of your source repository, so you have to back it
    up manually, and take special care of it.
2.  Version control the **settings\_local.py** files for each environment you
    have, and then manually change the **settings.py** file (or
    **\_\_init\_\_.py** file) in your project folder on each server. Now you've
    got the same problem as before--you've got to manually manage some source
    files, and constantly worry about breaking shit.

There's a better way.

Write a Settings Module

Instead of maintaining multiple flat settings files, build a settings module. Go
ahead and **rm** your **settings.py** file, and in its place, create a new
directory, called **settings**, and put in a blank **\_\_init\_\_.py** file to
start.

The goal here will be to do meet all the criteria that I defined in the
beginning of this article. In order to meet all those requirements, we need to:

1.  Define a **common.py** file inside of our settings module. This file will
    contain all of our 'shared' settings between all environments. For example:\

    ~~~~ {.line_numbers}
    1
    2
    3
    4
    5
    6
    7
    8
    9
    10
    11
    12
    13
    14
    15
    16
    17
    18
    19
    20
    21
    22
    23
    24
    25
    26
    27
    28
    29
    30
    31
    32
    33
    34
    35
    36
    37
    38
    39
    40
    41
    42
    43
    44
    45
    46
    47
    48
    49
    50
    51
    52
    53
    54
    55
    56
    57
    58
    59
    60
    61
    62
    63
    64
    65
    66
    67
    68
    69
    70
    71
    72
    73
    74
    75
    76
    77
    78
    79
    80
    81
    82
    83
    84
    85
    86
    87
    88
    89
    90
    91
    92
    93
    94
    95
    96
    97
    98
    99
    100
    101
    102
    103
    104
    105
    106
    107
    108
    109
    110
    111
    112
    113
    114
    115
    116
    117
    118
    119
    120
    121
    122
    123
    124
    125
    126
    127
    128
    129
    130
    131
    132
    133
    134
    135
    136
    137
    138
    139
    140
    141
    142
    143
    144
    145
    146
    147
    148
    149
    150
    151
    152
    153
    154
    155
    156
    157
    158
    159
    160
    161
    162
    163
    164
    165
    166
    167
    168
    169
    170
    171
    172
    173
    174
    175
    176
    177
    178
    179
    180
    181
    182
    183
    184
    185
    186
    187
    188
    189
    190
    191
    ~~~~

    """Common settings and globals."""

    \

    \

    import sys

    from os.path import abspath, basename, dirname, join, normpath

    \

    from helpers import gen\_secret\_key

    \

    \

    \#\#\#\#\#\#\#\#\#\# PATH CONFIGURATION

    \# Absolute filesystem path to this Django project directory.

    DJANGO\_ROOT = dirname(dirname(abspath(\_\_file\_\_)))

    \

    \# Site name.

    SITE\_NAME = basename(DJANGO\_ROOT)

    \

    \# Absolute filesystem path to the top-level project folder.

    SITE\_ROOT = dirname(DJANGO\_ROOT)

    \

    \# Absolute filesystem path to the secret file which holds this project's

    \# SECRET\_KEY. Will be auto-generated the first time this file is
    interpreted.

    SECRET\_FILE = normpath(join(SITE\_ROOT, 'deploy', 'SECRET'))

    \

    \# Add all necessary filesystem paths to our system path so that we can use

    \# python import statements.

    sys.path.append(SITE\_ROOT)

    sys.path.append(normpath(join(DJANGO\_ROOT, 'apps')))

    sys.path.append(normpath(join(DJANGO\_ROOT, 'libs')))

    \#\#\#\#\#\#\#\#\#\# END PATH CONFIGURATION

    \

    \

    \#\#\#\#\#\#\#\#\#\# DEBUG CONFIGURATION

    \# Disable debugging by default.

    DEBUG = False

    TEMPLATE\_DEBUG = DEBUG

    \#\#\#\#\#\#\#\#\#\# END DEBUG CONFIGURATION

    \

    \

    \#\#\#\#\#\#\#\#\#\# MANAGER CONFIGURATION

    \# Admin and managers for this project. These people receive private site

    \# alerts.

    ADMINS = (

    ('Your Name', 'your\_email@example.com'),

    )

    \

    MANAGERS = ADMINS

    \#\#\#\#\#\#\#\#\#\# END MANAGER CONFIGURATION

    \

    \

    \#\#\#\#\#\#\#\#\#\# GENERAL CONFIGURATION

    \# Local time zone for this installation. Choices can be found here:

    \# http://en.wikipedia.org/wiki/List\_of\_tz\_zones\_by\_name although not
    all

    \# choices may be available on all operating systems. On Unix systems, a
    value

    \# of None will cause Django to use the same timezone as the operating
    system.

    \# If running in a Windows environment this must be set to the same as your

    \# system time zone.

    TIME\_ZONE = 'America/Los\_Angeles'

    \

    \# Language code for this installation. All choices can be found here:

    \# http://www.i18nguy.com/unicode/language-identifiers.html.

    LANGUAGE\_CODE = 'en-us'

    \

    \# The ID, as an integer, of the current site in the django\_site database
    table.

    \# This is used so that application data can hook into specific site(s) and
    a

    \# single database can manage content for multiple sites.

    SITE\_ID = 1

    \

    \# If you set this to False, Django will make some optimizations so as not

    \# to load the internationalization machinery.

    USE\_I18N = False

    \

    \# If you set this to False, Django will not format dates, numbers and

    \# calendars according to the current locale.

    USE\_L10N = True

    \#\#\#\#\#\#\#\#\#\# END GENERAL CONFIGURATION

    \

    \

    \#\#\#\#\#\#\#\#\#\# MEDIA CONFIGURATION

    \# Absolute filesystem path to the directory that will hold user-uploaded
    files.

    MEDIA\_ROOT = normpath(join(DJANGO\_ROOT, 'media'))

    \

    \# URL that handles the media served from MEDIA\_ROOT.

    MEDIA\_URL = '/media/'

    \#\#\#\#\#\#\#\#\#\# END MEDIA CONFIGURATION

    \

    \

    \#\#\#\#\#\#\#\#\#\# STATIC FILE CONFIGURATION

    \# Absolute path to the directory static files should be collected to. Don't
    put

    \# anything in this directory yourself; store your static files in apps'
    static/

    \# subdirectories and in STATICFILES\_DIRS.

    STATIC\_ROOT = normpath(join(DJANGO\_ROOT, 'static'))

    \

    \# URL prefix for static files.

    STATIC\_URL = '/static/'

    \

    \# URL prefix for admin static files -- CSS, JavaScript and images.

    ADMIN\_MEDIA\_PREFIX = '/static/admin/'

    \

    \# Additional locations of static files.

    STATICFILES\_DIRS = (

    normpath(join(DJANGO\_ROOT, 'assets')),

    )

    \

    \# List of finder classes that know how to find static files in various

    \# locations.

    STATICFILES\_FINDERS = (

    'django.contrib.staticfiles.finders.FileSystemFinder',

    'django.contrib.staticfiles.finders.AppDirectoriesFinder',

    \#'django.contrib.staticfiles.finders.DefaultStorageFinder',

    )

    \#\#\#\#\#\#\#\#\#\# END STATIC FILE CONFIGURATION

    \

    \

    \#\#\#\#\#\#\#\#\#\# TEMPLATE CONFIGURATION

    \# List of callables that know how to import templates from various sources.

    TEMPLATE\_LOADERS = (

    'django.template.loaders.filesystem.Loader',

    'django.template.loaders.app\_directories.Loader',

    \#'django.template.loaders.eggs.Loader',

    )

    \

    \# Directories to search when loading templates.

    TEMPLATE\_DIRS = (

    normpath(join(DJANGO\_ROOT, 'templates')),

    )

    \#\#\#\#\#\#\#\#\#\# END TEMPLATE CONFIGURATION

    \

    \

    \#\#\#\#\#\#\#\#\#\# MIDDLEWARE CONFIGURATION

    MIDDLEWARE\_CLASSES = (

    'django.middleware.common.CommonMiddleware',

    'django.contrib.sessions.middleware.SessionMiddleware',

    'django.middleware.csrf.CsrfViewMiddleware',

    'django.contrib.auth.middleware.AuthenticationMiddleware',

    'django.contrib.messages.middleware.MessageMiddleware',

    )

    \#\#\#\#\#\#\#\#\#\# END MIDDLEWARE CONFIGURATION

    \

    \

    \#\#\#\#\#\#\#\#\#\# APP CONFIGURATION

    INSTALLED\_APPS = (

    'django.contrib.auth',

    'django.contrib.contenttypes',

    'django.contrib.sessions',

    'django.contrib.sites',

    'django.contrib.messages',

    'django.contrib.staticfiles',

    \

    \# Admin panel and documentation.

    'django.contrib.admin',

    'django.contrib.admindocs',

    \

    \# South migration tool.

    'south',

    \

    \# Celery task queue.

    'djcelery',

    \

    \# django-sentry log viewer.

    'indexer',

    'paging',

    'sentry',

    'sentry.client',

    )

    \#\#\#\#\#\#\#\#\#\# END APP CONFIGURATION

    \

    \

    \#\#\#\#\#\#\#\#\#\# CELERY CONFIGURATION

    import djcelery

    djcelery.setup\_loader()

    \#\#\#\#\#\#\#\#\#\# END CELERY CONFIGURATION

    \

    \

    \#\#\#\#\#\#\#\#\#\# URL CONFIGURATION

    ROOT\_URLCONF = '%s.urls' % SITE\_NAME

    \#\#\#\#\#\#\#\#\#\# END URL CONFIGURATION

    \

    \

    \#\#\#\#\#\#\#\#\#\# KEY CONFIGURATION

    \# Try to load the SECRET\_KEY from our SECRET\_FILE. If that fails, then
    generate

    \# a random SECRET\_KEY and save it into our SECRET\_FILE for future
    loading. If

    \# everything fails, then just raise an exception.

    try:

    SECRET\_KEY = open(SECRET\_FILE).read().strip()

    except IOError:

    try:

    with open(SECRET\_FILE, 'w') as f:

    f.write(gen\_secret\_key(50))

    except IOError:

    raise Exception('Cannot open file \`%s\` for writing.' % SECRET\_FILE)

    \#\#\#\#\#\#\#\#\#\# END KEY CONFIGURATION

2.  Define as many environment-specific settings files as you need. Just make
    sure to import from **common** at the top of your file. For example--here's
    a **dev.py** file:\

    ~~~~ {.line_numbers}
    1
    2
    3
    4
    5
    6
    7
    8
    9
    10
    11
    12
    13
    14
    15
    16
    17
    18
    19
    20
    21
    22
    23
    24
    25
    26
    27
    28
    29
    30
    31
    32
    33
    34
    35
    36
    37
    38
    39
    40
    41
    42
    43
    44
    45
    46
    47
    48
    49
    50
    51
    52
    53
    54
    55
    56
    57
    58
    59
    60
    61
    62
    63
    64
    65
    66
    67
    68
    69
    70
    71
    72
    73
    74
    75
    76
    77
    78
    79
    80
    81
    82
    83
    84
    85
    86
    87
    88
    89
    90
    91
    92
    93
    94
    95
    96
    97
    ~~~~

    """Development settings and globals."""

    \

    \

    from common import \*

    from os.path import join, normpath

    \

    \

    \#\#\#\#\#\#\#\#\#\# DEBUG CONFIGURATION

    DEBUG = True

    TEMPLATE\_DEBUG = DEBUG

    \#\#\#\#\#\#\#\#\#\# END DEBUG CONFIGURATION

    \

    \

    \#\#\#\#\#\#\#\#\#\# EMAIL CONFIGURATION

    EMAIL\_BACKEND = 'django.core.mail.backends.dummy.EmailBackend'

    \#\#\#\#\#\#\#\#\#\# END EMAIL CONFIGURATION

    \

    \

    \#\#\#\#\#\#\#\#\#\# DATABASE CONFIGURATION

    DATABASES = {

    'default': {

    'ENGINE': 'django.db.backends.sqlite3',

    'NAME': normpath(join(SITE\_ROOT, 'db', 'default.db')),

    'USER': '',

    'PASSWORD': '',

    'HOST': '',

    'PORT': '',

    }

    }

    \#\#\#\#\#\#\#\#\#\# END DATABASE CONFIGURATION

    \

    \

    \#\#\#\#\#\#\#\#\#\# CACHE CONFIGURATION

    CACHES = {

    'default': {

    'BACKEND': 'django.core.cache.backends.dummy.DummyCache',

    }

    }

    \#\#\#\#\#\#\#\#\#\# END CACHE CONFIGURATION

    \

    \

    \#\#\#\#\#\#\#\#\#\# DJANGO-DEBUG-TOOLBAR CONFIGURATION

    MIDDLEWARE\_CLASSES += (

    'debug\_toolbar.middleware.DebugToolbarMiddleware',

    )

    \

    INSTALLED\_APPS += (

    'debug\_toolbar',

    )

    \

    \# IPs allowed to see django-debug-toolbar output.

    INTERNAL\_IPS = ('127.0.0.1',)

    \

    DEBUG\_TOOLBAR\_CONFIG = {

    \# If set to True (default), the debug toolbar will show an intermediate

    \# page upon redirect so you can view any debug information prior to

    \# redirecting. This page will provide a link to the redirect destination

    \# you can follow when ready. If set to False, redirects will proceed as

    \# normal.

    'INTERCEPT\_REDIRECTS': False,

    \

    \# If not set or set to None, the debug\_toolbar middleware will use its

    \# built-in show\_toolbar method for determining whether the toolbar should

    \# show or not. The default checks are that DEBUG must be set to True and

    \# the IP of the request must be in INTERNAL\_IPS. You can provide your own

    \# method for displaying the toolbar which contains your custom logic. This

    \# method should return True or False.

    'SHOW\_TOOLBAR\_CALLBACK': None,

    \

    \# An array of custom signals that might be in your project, defined as the

    \# python path to the signal.

    'EXTRA\_SIGNALS': [],

    \

    \# If set to True (the default) then code in Django itself won't be shown in

    \# SQL stacktraces.

    'HIDE\_DJANGO\_SQL': True,

    \

    \# If set to True (the default) then a template's context will be included

    \# with it in the Template debug panel. Turning this off is useful when you

    \# have large template contexts, or you have template contexts with lazy

    \# datastructures that you don't want to be evaluated.

    'SHOW\_TEMPLATE\_CONTEXT': True,

    \

    \# If set, this will be the tag to which debug\_toolbar will attach the
    debug

    \# toolbar. Defaults to 'body'.

    'TAG': 'body',

    }

    \#\#\#\#\#\#\#\#\#\# END DJANGO-DEBUG-TOOLBAR CONFIGURATION

    \

    \

    \#\#\#\#\#\#\#\#\#\# CELERY CONFIGURATION

    INSTALLED\_APPS += (

    'djkombu',

    )

    \

    BROKER\_BACKEND = 'djkombu.transport.DatabaseTransport'

    \#\#\#\#\#\#\#\#\#\# END CELERY CONFIGURATION

3.  When you're using any sort of management command, just specify the settings
    module you want to use. For example: instead of running **python manage.py
    syncdb**, you would do **python manage.py syncdb --settings=settings.dev**.
    Same goes with runserver, or any other command.

As you can see, the benefits of a settings module are numerous, and meet all of
our requirements. We're able to have our cake (by version controlling all
environment specific settings) and eat it too (by choosing which one to use
simply via our application server).

I've been working on numerous Django projects for around 2 years now, and I've
not found any better ways to do it.

Got Suggestions?

If so, I'd love to hear them. Seriously.
