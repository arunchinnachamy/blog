---
layout: post
title: How To - Update/ Install NGINX in Ubuntu or Debian
excerpts: This post describes how to install NGINX in Ubuntu or Debian systems through simple and easy apt-get method. 
---

If you are looking for ways to upgrade or install NGINX in Ubuntu or Debian systems, you are in right place. You can install them easily using the aptitude or apt-get command.

The process only takes couple of minutes and requires minimal Linux knowledge.

##### NGINX Installation procedure:

Open Terminal. Open the Sources file for aptitude using vi. If you are not familiar with vi, open the file in nano or gedit.
<pre lang="bash">sudo gedit /etc/apt/sources.list</pre>
If you are using Debian Squeeze (version 6.x),

<pre>deb http://nginx.org/packages/debian/ squeeze nginx
deb-src http://nginx.org/packages/debian/ squeeze nginx</pre>

If you are using Ubuntu, select the respective lines from below,
For Ubuntu Lucid (Version 10.04),
<pre>deb http://nginx.org/packages/ubuntu/ lucid nginx 
deb-src http://nginx.org/packages/ubuntu/ lucid nginx</pre>

For Ubuntu Oneiric (Version 11.10),
<pre>deb http://nginx.org/packages/ubuntu/ oneiric nginx 
deb-src http://nginx.org/packages/ubuntu/ oneiric nginx</pre>

For Ubuntu Precise (Version 12.04),

<pre>deb http://nginx.org/packages/ubuntu/ precise nginx
 deb-src http://nginx.org/packages/ubuntu/ precise nginx</pre>

Add the lines applicable to you in the sources.list file. Now save and close the file.

In order to authenticate the repository and to avoid warnings about missing gpg key during installation of the nginx package, it is advised to add the key used to sign the nginx packages and repository to the apt program keyring.

<span style="color: #ff0000;"></span>

Run the following commands in the terminal,
<pre lang="bash">wget http://nginx.org/keys/nginx_signing.key
cat nginx_signing.key | sudo apt-key add -</pre>
Once added, update the aptitude with new repository,
<pre lang="bash">sudo apt-get update</pre>
Now to install NGINX in Ubuntu or Debian systems, run the following command in terminal,
<pre lang="bash">sudo apt-get install nginx</pre>
Comment below if you find it useful or came across any issues when installing NGINX server. If you are looking to install or upgrade PHP, Visit this [HOWTO page](http://www.arunchinnachamy.com/howto-upgrade-php-version-in-ubuntu-or-debian-using-apt-get/ "HOW TO :: Upgrade PHP version in Ubuntu or Debian using apt-get").
