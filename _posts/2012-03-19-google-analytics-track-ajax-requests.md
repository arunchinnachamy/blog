---
layout: post
title: Google Analytics - Track Ajax Requests and Javascript
excerpts: This post describes how to track AJAX requests and javascript Actions and analyze the activities through Google Analytics.
---

Almost everyone uses Google Analytics. It is an amazing tool that has revolutionized web statistics.
The Code provided by Google when we sign up is enough to track all standard sites.

    <script type="text/javascript">
    var _gaq = _gaq || [];  
    _gaq.push(['_setAccount', 'UA-XXXXXXXX-X']); // your ID/profile  
    _gaq.push(['_trackPageview']);  
    (function() {  
         var ga = document.createElement('script'); ga.type = 'text/javascript'; ga.async = true;  
             ga.src = ('https:' == document.location.protocol ? 'https://ssl' : 'http://www') + '.google-analytics.com/ga.js';  
                 var s = document.getElementsByTagName('script')[0]; s.parentNode.insertBefore(ga, s);  
                 })();
    </script>

Why do we need more than this? There are this growing category of sites which are strongly dependent on loveliness of Ajax and elegance of Javascript. The code above make it impossible to track Ajax and Javascript events in Google Analytics. So how to track Ajax Requests and Javascript Actions through Google Analytics?

Google Analytics has a way to track Ajax Requests and Javascript actions through Event Tracking. Lets have a look into how to track Ajax requests and Javascript Events in Google Analytics.

Lets consider the case when you want to record number of times a particular Javascript powered application has delivered some image. The Event Tracking takes four arguments using which you want to track the event.

1. Category
2. Action
3. Label (Optional)
4. Value (Optional and Integer)
5. non-interaction (Optional)

In order to trigger an event on the click event on Images, we need to add the following code.

    onClick="_gaq.push(['_trackEvent', 'Images', 'Click', 'BirthDay Picture']);"

Here the category name is Images, the event Action is Click and the optional Label is BirthDay Picture.
Or in case you want to track how many AJAX request has been processed with specific parameters.

    _gaq.push(['_trackEvent','AJAX','Request',$Parameters]);

The data will be available in Google Analytics within 24 hours. This way, we can completely track ajax requests in Google analytics and Google Analytics event tracking can help you monitor user activity at a deeper level than standard page views by allowing you to track AJAX requests and Javascript Actions. More detailed information and examples can be found in [Google Documentation](http://code.google.com/apis/analytics/docs/tracking/eventTrackerGuide.html){:target="_blank"}.

There are few considerations to keep in mind when implementing the Event Tracker like How this will impact our bounce rate and 500 Events per session limit imposed by Google Analytics.



