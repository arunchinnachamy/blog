---
layout: post
title: HOWTO - Install Latest Redis from source – Version 2.6.2 NOT *2.4.17*
excerpts: This post explains how to install latest Redis server version from Source rather than installing old version from Debian repository using apt-get.
---
If you have followed by [Redis Installation guide using apt-get](http://www.arunchinnachamy.com/awesomeness-of-redis-redis-installation-and-configuration/ "Awesomeness of Redis – Redis Installation and Configuration"), you would have installed 1.2 version of Redis which is pretty old and does not contains few useful commands like **ZREVRANGEBYSCORE** and ** ZREMRANGEBYRANK**. Unfortunately, the latest version is not available to be installed from Debian repository. So the best option to install latest redis server is to download the source and build the code. It does not take long to do that.

Just follow the steps below. Provide the commands in the terminal,
<pre lang="bash">wget http://download.redis.io/redis-stable.tar.gz
tar xvzf redis-stable.tar.gz
cd redis-stable
make</pre>

Check whether the compilation was successful. If so, congratulations. You have successfully built redis from source else please post the error on the comment section below.


On successful compilation, the **_src_** directory will contain the following executable files.

*   **redis-server** - The server executable
*   **redis-cli** - command line interface
*   **redis-benchmark** - For performance checking purpose
*   **redis-check-aof** - For debugging corrupt data files.
*   **redis-check-dump** - Debugging the corrupt data files.

Copy the _redis.conf_ file under the redis-stable directory.
<pre lang="bash">sudo cp redis.conf /etc/redis/redis.conf</pre>
If the directory is not available under /etc/, create a folder named redis and copy the configuration file.

Copy the server executable into bin folder,
<pre lang="bash">cd src
sudo cp redis-server /usr/bin/</pre>

To start the redis server with the configuration file which starts the server in port 6379,
<pre lang="bash">redis-server /etc/redis/redis.conf</pre>

Looking for various configuration parameters in Redis, check out [Redis Installation guide](http://www.arunchinnachamy.com/awesomeness-of-redis-redis-installation-and-configuration/ "Awesomeness of Redis – Redis Installation and Configuration"). 

In Debian, you can add the following file into /etc/init.d/redis-server which starts the redis server at startup using the configuration file placed in /etc/redis/redis.conf.
<script src="https://gist.github.com/257298.js"></script>
<div style="text-align:center;"></div>

You can Start and stop redis server using,
<pre lang="bash">sudo /etc/init.d/redis-server start
sudo /etc/init.d/redis-server stop</pre>

Leave your comment if you find this useful or something wrong. If you are using PHP as programming language, You might want to look into How to [connect Redis from PHP](http://www.arunchinnachamy.com/awesomeness-of-redis-redis-and-php/ "Awesomeness of Redis – Redis and PHP").

Update: Now that Redis 2.6.2 is released and stable, now you can follow the same procedure to install Redis 2.6.2 and enjoy support for Lua scripting, milliseconds precision expires, improved memory usage, unlimited number of clients, improved AOF generation and better performance. 
