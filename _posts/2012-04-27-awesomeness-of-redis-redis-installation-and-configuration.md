---
layout: post
title: Awesomeness of Redis – Redis Installation and Configuration
excerpts: Redis Installation - How to Install and configure this crazy fast database engine and when to use it. Advantages and Disadvantages of Redis.
---
Redis - A Key-Value pair High Performance NoSQL Database. Why you should consider it? and Redis Installation in Ubuntu and Debian System.

In search of High Performance NoSQL database, I came across Redis few months ago. I installed in my Work Machine and started exploring the possibilities and How to use Redis for implementing Product Search Filters at [MySmartPrice.com](http://www.mysmartprice.com). The results were mind blowing and exactly as promised in the [Redis official website](http://redis.io/). In this post, I will try to explain the installation procedure for Redis in Debian/Ubuntu and also help you decide when to use Redis.

**Why should you consider Redis?**

*   Redis is crazy fast key-value engine
*   Using the Collection types, you can create complex data structures required for your applications
*   The Asynchronous snapshot of the data is a huge boon. It gives the persistence with little penalty.
*   Auto expiry of the keys is another big addition when dealing with cache data.

**Few Drawbacks of Redis:**

*   Being a in memory data storehouse, you need to have a huge RAM if you want to handle huge data banks. Even a huge data store will fit into redis comfortably. Check the benchmarking results [here.](http://nosql.mypopescu.com/post/1010844204/redis-memory-usage) Even a 10000 sets, each with 1000 Records takes little more than a GB of RAM. With RAM prices going down, I do not find this as a major drawback.
*   Second thing is, loss of data when your system fails between the snapshots. This could be avoided through the proper design of your system and configuration.

**Where should you deploy Redis?**

*   **Redis will act as an excellent cache system.** You can replace your memcache with Redis which will provide you more flexibility and performance. With persistence and reduced round trips to database, it makes your cache system a better one. The auto expires and TTL are absolute plus for caching system.
*   **Redis as Real Time Analytics system.** If you want to track some events or data realtime, you will not be failed by Redis's performance. It will provide the ultra fast inserts and access to your data.
*   **Redis as Counter.** Redis with all the inbuilt Queue, Sets, Sorted sets etc will act as the best NOSQL Database for handling leader boards, Counters, voting systems and real time stats. The union and Intersection provided in sets comes handy.
Now coming to the installation procedure of Redis, I will explain how to install and configure redis in Ubuntu/Debian.

If you want to take the road less travelled by building the redis from source and do the configuration manually rather than letting aptitude to do that for you, you should go [here](http://www.arunchinnachamy.com/howto-install-latest-redis-version-2-4-17/ "HOWTO: Install Latest Redis from source – Version 2.4.17"). The advantage being you will be installing the latest stable version of redis rather than usual old version in Debian Repository.

### Redis Installation and Configuration

You can install redis in Ubuntu using the aptitute package manager. Type the following command in the terminal.
<pre lang="bash">sudo apt-get install redis-server</pre>
Once finished, the default configuration file will be available in
<pre>/etc/redis/redis.conf</pre>
There are couple of variables which you might find it interesting and necessary to configure based on the application you work.

Depending on your preference, Change the port. This is the port in which Redis will listen for tcp connections from applications.
<pre lang="text">port 6379</pre>
If you want Redis to listen only to localhost, uncomment the following line. If not, the server will listen to all available networks.
<pre>bind 127.0.0.1</pre>
I personally prefer to timeout the idle connections to prevent stale connections. If you think the same way, set the timeout something around 30 seconds.
<pre>timeout 30</pre>
If you like to protect your data with a password authentication, you need to uncomment and provide the password in the following line
<pre>requirepass {password}</pre>
By default, Redis server has 16 databases. If you are not planning to use 16 databases, set the value accordingly.
<pre>databases 16</pre>
As told above, Redis takes a snapshot of data depending on the configuration as a file. The persistence can be controlled using the following lines

    #   In the example below the behaviour will be to save:
    #   after 900 sec (15 min) if at least 1 key changed
    #   after 300 sec (5 min) if at least 10 keys changed
    #   after 60 sec if at least 10000 keys changed
    #
    #   Note: you can disable saving at all commenting all the "save" lines.

    save 900 1
    save 300 10
    save 60 10000</pre>

Restart the redis server by running the following command in terminal
<pre>sudo /etc/init.d/redis restart</pre>
You can test the redis server by telnetting to the post specified in the configuration.

In terminal,
   <pre>telnet 127.0.0.1 {port}</pre>

If you have set the password, you need to authenticate before executing any commands using AUTH command. For complete set of data types and command, visit [Redis Command Reference](http://redis.io/commands).

Now your redis server is up and running, I will explain how to connect to Redis server through _PHP using Predis_ in my next post. If you come across any difficulty, feel free to post a comment, I will be happy to assist you.
