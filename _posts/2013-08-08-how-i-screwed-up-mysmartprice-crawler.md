---
layout: post
title: How I screwed up MySmartPrice Crawler?
excerpts: How i ended up messing the MySmartPrice crawlers because of a simple JOIN condition in a MySQL query.
---
We are a startup who are working towards better product. Last month, about 2% of our user base who have subscribed for Price drop alerts got an mail blast mentioning the product is available for 14,499 INR. What does this mean? Samsung galaxy S4 is available for one-third of its original price. All the televisions, Laptops and appliances in our inventory were available for the same price. As anybody will imagine, we got the maximum Open Rate and Click-through rate for that email blast. Equally, We received negative response from our users blaming us for their disappointment and few calling us as fraudsters. This post is about how i messed up the MySmartPrice crawler.

#### Why did this happen?

At this same time, I was working with our new engineer in a feature, we call it Priority Crawler. What is means is, we try to prioritize the product which is popular to be crawled frequently when compared to be products with less interest from the user. For this feature, we ended up creating a new table which will serve as a temporary table before the crawler dumps the data into our really big product information table. 

#### What went wrong?

It was very silly on my part (and also the new engineer :P) for not concentrating on the new SQL query which dumps the data from new temporary table to the final table. He has done a very small mistake by missing the JOIN condition between the table. I was supposed to act responsible, review the source code and test it in our staging server to make sure it does not break any core functionality of our precious crawler. In the contrary, I was bit (too?) confident and _lazy_ to wait for the long crawler to complete and updated our production crawler to check it. I ran the crawler for one of the product and checked that the temporary table was good and price was indeed updated into the final table. What I did not realize is, all the lines in the final table is now with the price of this single line, which was Rs. 14,499. I closed my laptop and went to sleep only to find my inbox having price alert of Samsung Galaxy S3 for Rs. 14,499 next morning.

#### How we pledged to prevent this in future?

As a result of the small error, We ended sending more than 20K emails. After this incident, we worked on ways to prevent such issues. We configured a crawling server which will be used to test any minor changes to the crawler. At least I am determined to make sure this kind of event does not happen again.
