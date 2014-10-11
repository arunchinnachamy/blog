---
layout: post
title: HowTo - Install Pear Packages & Pear Installer in Debian
excerpts: This post describes how to install Pear packages and Pear Installer in Debian or Ubuntu using Command line Terminal with fast installs & no configuration.
---
I find it sometimes useful and easy to install the PHP packages using PEAR (PHP Extension and Application Repository) than to install them manually. I thought of posting the procedure on how to install Pear Installer and also how to install pear packages in Debian as a reminder to myself and someone like you might find it useful.

The commands are tested in Debian Lenny and found working fine. If not, Please post a comment along with the error message. The same commands will work fine in Ubuntu also. 

#### PEAR Installer

To install the Pear Installer, run the following command in terminal,
<pre lang="bash">sudo apt-get install php-pear</pre>

The command should install the PHP Pear Package installer in your system. After this, Installing pear packages from repository will be very easy in no time. 

You can check for packages available in the repository in the [Pear Packages Page](http://pear.php.net/packages.php "Pear Packages"). 

<span style="text-align:center; color: #ff0000;"></span>

#### PEAR Package Installation

Lets say you want to install XML Serializer from [this page](http://pear.php.net/package/XML_Serializer "XML Serializer"). I find this package quite useful when I want to convert XML to Json via PHP Array or vice versa. To install the package, Just issue this command in your command line.
<pre lang="bash"> sudo pear install XML_Serializer</pre>

If your system throws up with some errors complaining about the beta version not supported like shown below, 
<pre>Failed to download pear/XML_Serializer within preferred state "stable",
 latest release is version 0.X.X, stability "beta",
 use "channel://pear.php.net/XML_Serializer-0.X.X" to install 
Cannot initialize 'channel://pear.php.net/XML_Serializer', invalid or missing package file
Package "channel://pear.php.net/XML_Serializer" is not valid install failed</pre>
You just need to override the setting by mentioning the version which you want to install. Just mention the beta version to override the system.

<pre lan="bash"> sudo pear install XML_Serializer-beta</pre>

Similar to this, you can install any pear packages available in the PEAR repository in matter of seconds with little to no trouble shooting and configurations. 

#### UnInstall Pear Packages

In case you want to uninstall a pear package, you can issue the following command in the terminal,
<pre lang="bash">sudo pear uninstall [package name]
sudo pear uninstall XML_Serializer-beta</pre>

Let me know if you find this useful or if I have missed something.
