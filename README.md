# About stav #

stav is a text visualisation tool, developed for the purpose of visualising
data from the BioNLP'11 Shared Task:

https://sites.google.com/site/bionlpst/

# License #

Please see the file LICENSE.

# Installation #

This page describes how to install stav and third-party dependencies.

## stav ##

Check out stav using git.

    git clone git@github.com:TsujiiLaboratory/stav.git

When running stav it needs to write data to several directories,
let's create them.

    mkdir svg sessions data

We now need to set the permissions of these directories so that they can be
read and written by Apache. The command sequence below is likely to give you
the Apache 2 group if you have Apache currently running. Otherwise refer to
your operating system manual and look-up Apache 2 for their instructions on
how to get/set the Apache group:

    groups `ps aux | grep apache | grep -v 'grep' \
            | cut -d ' ' -f 1 | grep -v 'root' | head -n 1` \
        | cut -d : -f 2 | sed 's|\ ||g'

Then simply change the group of the directories (change `${YOUR_APACHE_GROUP}`
into the output you got above) and set the correct permissions:

    sudo chgrp -R ${YOUR_APACHE_GROUP} data sessions svg
    chmod -R g+rwx data sessions svg

If you can't succeed with the above or you are not concerned with security
(say that you are on a single-user system), you can run the command below
instead. This will make the directories write-able and read-able by every
user on your system:

    chmod 777 data sessions svg

Extract all the library dependencies.

    (cd server/lib && tar xfz simplejson-2.1.5.tar.gz)

Put a configuration in place.

    cp config_template.py config.py

Edit the configuration to suit your environment (there are instructions for
each variable in the file).

    vim config.py

Your installation should now be ready!

## Third-party Stuff ##

This part largely focuses on Ubuntu, but use your *NIX-foo to turn it into
what you need if you don't have the misfortune to have
the "Brown Lunix Distribution".

### git ###

Hopefully you are lucky enough a sensible OS and can get it through
a package manager:

    sudo apt-get install git

### Apache 2 ###

Install apache2, maybe you have a package manager.

    sudo apt-get install apache2

Let's edit the httpd.conf.

    sudo vim /etc/apache2/httpd.conf

If you are installing stav into your home directory,
add the following four lines.

    <Directory /home/*/public_html>
        AllowOverride Options Indexes
        AddHandler cgi-script .cgi
        AddType application/xhtml+xml;q=0.8 .xhtml
    </Directory>

And make sure that you have the userdir module enabled.

    sudo a2enmod userdir

Finally tell apache2 to load your new config.

    sudo /etc/init.d/apache2 reload

### On a Mac ###

On a Mac, Apache configuration is quite different, and Aptitude is not available.

#### Getting Git ####

A binary package is available from here:
[[http://code.google.com/p/git-osx-installer/downloads/list]].
Alternately, you can get it through a package manager:
Homebrew ([[https://github.com/mxcl/homebrew/wiki/installation]])
or MacPorts ([[http://www.macports.org/install.php]]). You will need XCode
(from your Mac OS X disk,
or from Apple at [[http://developer.apple.com/xcode/]]).
Then you can use one of these commands to install Git:

    brew install git

or

    port install git

#### Setting up Apache ####

Clone this repository into `~/Sites`.
Edit `/private/etc/apache2/users/$USER.conf`.
Then invoke `sudo apachectl reload`.
The default user and group name for Apache is `_www`
(as found in `/private/etc/apache2/httpd.conf`), for use in `chgrp`.
