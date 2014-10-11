---
layout: post
title: Sencha Touch – Google Analytics with PhoneGap
excerpts: This post focuses on integration of Google Analytics with PhoneGap into Sencha Touch android Application using plugin GAPlugin along with code snippets.
---
Recently I integrated Google Analytics into our [MySmartPrice Android Application ](https://play.google.com/store/apps/details?id=com.MySmartPrice.MySmartPrice "MySmartPrice Android APP"). The application was built using [Sencha Touch framework](http://www.sencha.com/products/touch "Sencha Touch") and packaged using [PhoneGap](https://build.phonegap.com/ "PhoneGap"). So this post is about integrating Google Analytics with PhoneGap into Sencha Touch Android application. 

It is simple to add Google Analytics tracking into your Android Application through phonegap. PhoneGap provides a plugin named **GAPlugin** which needs to be included into your project. Once included, with less than 10 lines of code, your basic tracking will be available through Google Analytics.

#### Google Analytics with PhoneGap

I assume you are already familiar with _config.xml_ in PhoneGap. In the file, you need to add the following line of xml,

    <gap:plugin name="Analytics"></gap:plugin>

Once added, save and close your file. Open the index.html where you have included the phonegap.js script. Add the extra lines in to the html file [Do not include phonegap.js twice].

    <script src="phonegap.js"></script>
    <script src="GAPlugin.js"></script>

Now we have integrated the plugin into our project. It is time to initialize the component and start pushing information to our analytics account. In the html file, before closing the head, add the following script.

    <script type="text/javascript">
        var gaObj;
        function initialize() {
            document.addEventListener("deviceready", onDeviceReady, true);
        }
        function onDeviceReady() {
            gaObj = window.plugins.gaPlugin;
            gaObj.init( function() {console.log('success');}, function() {console.log('failed');}, "UA-12345678-1", 15);
            TrackPageView("androidapp/index.html");
        }
        function TrackEvent(category,action,label,value) {
            gaObj.trackEvent( function() {console.log('success');}, function() {console.log('failed');}, category, action, label, value);
        }
        function TrackPageView(pageurl) {
            gaObj.trackPage(function() {console.log('success');}, function() {console.log('failed');}, pageurl);
        }
    </script>

#### What does the above code do?

On initialization, a listener is added to _deviceready_ event. On device ready, we are initializing the plugin object with our Google Analytics tracking code and time interval between sending events to server. Keep this less to minimize the overhead of sending data from mobile devices. All the functions take first two parameters as functions which will be called on success and failure respectively. In this case, we are just writing an anonymous function which prints the text to console. If you want to take some action based on the feedback, write a function and replace the anonymous function with the function name.

After init, we are logging the page view with the corresponding URL. The function **TrackPageView** sends an URL to Google analytics which can be found under Content/Site Content/All pages in your analytics account. Another function, **TrackEvent** sends an event along with different parameters. If you want to log an event whenever someone clicks on certain actions like Login, make the following javascript call on the click event of the button,

<pre lang="javascript">TrackEvent('Login','Click','Some Value','any value');</pre>

You should use _TrackEvent_ in case if you wish to track ajax requests and events. The information can be found under Content/Events/Overview in your analytics dashboard. If you wish to know more information on how events work, look at this [post](http://www.arunchinnachamy.com/google-analytics-track-ajax-requests/ "Google Analytics: Track Ajax Requests and Javascript"). 
