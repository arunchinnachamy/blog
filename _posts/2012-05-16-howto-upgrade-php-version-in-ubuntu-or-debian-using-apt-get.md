---
layout: post
title: HOW TO - Upgrade PHP version in Ubuntu or Debian using apt-get
excerpts: This post explains how to install the latest PHP version through apt-get in Debian or upgrade the existing php installation.
---

Few days back, i was looking around for ways to upgrade PHP version in Ubuntu or Debian using apt-get. Earlier, I have built the module from the source and i found this easier alternative using apt-get.

I found this convenient way to upgrade which took me only couple of minutes.
Open the Sources file for aptitude.

     sudo vim /etc/apt/sources.list

Add the following line at the end of the file.

    deb http://php53.dotdeb.org stable all
    deb-src http://php53.dotdeb.org stable all

In order to prevent the aptitude from issuing warning, add the key through following command.

    wget http://www.dotdeb.org/dotdeb.gpg
    cat dotdeb.gpg | sudo apt-key add -

### Update the aptitude source

    apt-get update

### Install  Latest PHP Version

    apt-get install libapache2-mod-php5 php5 php5-common php5-curl php5-dev php5-gd php5-imagick php5-mcrypt php5-memcache php5-mhash php5-mysql php5-pspell php5-snmp php5-sqlite php5-xmlrpc php5-xsl

### Upgrade the PHP Version

    apt-get dist-upgrade

The PHP version in the ubuntu or Debian will be upgraded to the latest stable version available. Hope this helps!!!
