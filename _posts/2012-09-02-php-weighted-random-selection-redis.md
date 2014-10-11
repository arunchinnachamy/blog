---
layout: post
title: Weighted Random Selection in PHP using Redis
excerpts: This post describes how to perform weighted random selection in PHP using Redis as data house. The algorithm can be applied to C#, Python and others.
---
Recently, I came across a problem to make not a random selection from a set but to perform weighted random selection from a Dictionary which contains the item and the score of the items. As i did not find any inbuilt functions in PHP to perform weighted random, I was forced to write one. The data for me is already in Redis so planned to perform the weighted selection using redis, if possible. Here my primary programming language is PHP but the same random generation algorithm can be used in any other languages like python or C#. So lets dive into PHP Weighted random selection based on Redis.

Following is bit of background in what we are trying to do. 

##### Why Weighted Random Selection?

In Weighted Random selection, the aim is to select a random member from a set but providing more weightage to the item with more score. So lets consider a set,
<pre lang="php">array('a'=>'10','b'=>'2','c'=>'14','d'=>'7')</pre>
In typical random selection, all the four items (a,b,c,d) have the same and equal probability to be selected. But what we are trying to do here is to select 'c' more times than 'a' and select 'b' the least based on their individual score data. So there should be randomness but based on the weightage. 

##### How to calculate weighted random?

The biggest question is whether the scores of the individual items changes over time. If they stay fixed or if you can batch process them periodically, the solution is simple using Cumulative Frequency. I assume you already have redis, If you are looking how to install redis, look into my [redis installation guide](http://www.arunchinnachamy.com/awesomeness-of-redis-redis-installation-and-configuration/ "Awesomeness of Redis – Redis Installation and Configuration") or [build redis from source](http://www.arunchinnachamy.com/howto-install-latest-redis-version-2-4-17/ "HOWTO: Install Latest Redis from source – Version 2.4.17"). I suggest to build from source to ensure latest stable build. If you are unsure how to connect from PHP to redis, follow the instructions in my [Predis tutorial](http://www.arunchinnachamy.com/awesomeness-of-redis-redis-and-php/ "Awesomeness of Redis – Redis and PHP"). 

<span style="color: #ff0000;"></span>

##### Step 1: Loading Data into Redis

Load the Redis with your items and scores as a Sorted Set.

<pre lang="php">$redis->flushdb(); //Flushes the existing keys
$cumulativeweight = 0;
foreach ($items as $item=>$score) {
    $cumulativeweight += $score;
    $redis->zadd('items',"$cumulativeweight","$item");
}</pre>
What we have done is, take all the items you want to include in weighted random sample along with their scores. Now store the members into a Sorted Set but their scores being cumulative. Taking our sample data,
<pre lang="php">array('a'=>'10','b'=>'2','c'=>'14','d'=>'7')</pre>
will be stored with scores of
<pre lang="php">array('a'=>'10','b'=>'12','c'=>'26','d'=>'33')</pre>
This will be our cumulative frequency population on which we like to select our random member.

##### Step 2: Weighted random Selection

In time of selection, you should generate a random number between the 1 and total cumulative score. In this case, between 1 and 33. The following piece of code should do the trick where function takes redis connection instance as input.
<pre lang="php">function get_weighted_random($redis) {
    $set = $redis->zrevrangebyscore('items', '+inf', '-inf', array('withscores' => true));
    $total_weight = $set[0][1]; //gives the score of first element in the set.
    $random_no = mt_rand(1,$total_weight);
    $random_member = $redis->zrevrangebyscore('items', "$random_no", '+inf', array('limit' => array('offset' => 0, 'count' => 1)));
    return $random_member;
}</pre>
To explain, we are getting the maximum score available in the sample population, in our case the value will be 33. Then a random number is picked between 1 and 33. Lets consider the number to be 17. Then the members with score in between random number (17) and infinity is selected but restricted to one item which is the member which we want as result of our weighted random selection. If you look closer, Now the probability of each item to be chosen is equal to their weight/total weight.

Leave your comments if you find this useful or if you have any other better solution to find weighted random number in PHP with or without redis. To conclude, we have used cumulative frequency to solve PHP weighted random selection problem with the help of redis. 
