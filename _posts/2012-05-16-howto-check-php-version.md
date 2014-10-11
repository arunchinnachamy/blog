---
layout: post
title: HOW TO - Check PHP Version
excerpts: This post explains the two easiest way to check the PHP version you are running in your server.
---

How to check PHP version, there are two easy ways to do this depending on what kind of access you have to the server.

- If you have SSH terminal access to the machine, type the following to know the PHP Version.
    
    php -v

This output the following in my mac.

    PHP 5.3.8 with Suhosin-Patch (cli) (built: Nov 15 2011 15:33:15)
    Copyright (c) 1997-2011 The PHP Group
    Zend Engine v2.3.0, Copyright (c) 1998-2011 Zend Technologies

Here the PHP version is 5.3.8

- In case you have a hosting service who does not provide SSH access to the machine, you can write a small PHP file with following text and save it as version.php

    <?php
    phpinfo();
    ?>

FTP the file in to your root directory or directory of your convenience in your hosting service. Open the file through your browser using http://www.yoursitename.com/version.php. If you have copied the file in any other directory, change the url accordingly.

The rendered web page will show the configuration of PHP module in the server. On the top of the page, the version of PHP will be displayed. 

If you like to install or upgrade the PHP version in Ubuntu or Debian, you can find the details on [this page](http://www.arunchinnachamy.com/howto-upgrade-php-version-in-ubuntu-or-debian-using-apt-get/) useful.
    
