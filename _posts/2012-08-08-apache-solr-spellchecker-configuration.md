---
layout: post
title: Apache SOLR - SOLR Spellchecker and Configuration
excerpts: spell checking in Search made easy with in-built solr spellchecker. This post is about types of spell checker and its configuration in solr.
---

Now a days Spell checking has become an important feature in Search be it Google or any E-Commerce site. The famous and most used lucene based Search index engine SOLR comes with Spell checker in-built. If you are interested in performing spell checking with SOLR, you won't be disappointed with what it has under its folds. Lets start out tutorial on How to configure SOLR spellchecker and use it for spell correction and search suggestions.

If you looking for How to install SOLR in Debian/Ubuntu, visit [SOLR Installation Guide](http://www.arunchinnachamy.com/apache-solr-installation-and-configuration/ "The Seach Platform, Apache SOLR Installation and Configuration").

#### About SOLR SpellChecker

Be aware that the Spell checker in SOLR is not really a spell checker but a spell suggester. It provides inline spell checking for which you do not have to issue a separate request. Frankly, it should be named Spell Suggestions or Query suggestions rather than Spell Checker. So If you find the query to be spelled wrong, you need to query again with the correct spell. But this really helps in implementing features like providing_ Do you mean XXX or YYY _ suggestions to users.

##### SOLR Configuration for Spell Checker:

There are more than one way to implement the spell checker in SOLR. The important two being,

*   Dictionary Based Spell checker
*   Index Based Spell checker

##### Index Based Spell checking in Solr

I find this to be very effective and useful. Solr uses one of the configured field in the indexed document as Dictionary input and uses it for spell suggestions. The configuration is simple and requires very less effort.
<pre lang="xml" escaped="true">&lt;searchComponent name="spellcheck" class="solr.SpellCheckComponent"&gt;
   &lt;str name="queryAnalyzerFieldType"&gt;string&lt;/str&gt; &lt;!-- Replace with Field Type of your schema --&gt;
   &lt;lst name="spellchecker"&gt;
       &lt;str name="name"&gt;default&lt;/str&gt;
       &lt;str name="field"&gt;productname_spell&lt;/str&gt; &lt;!-- Replace with field name as per your scheme --&gt;
       &lt;str name="spellcheckIndexDir"&gt;./spellchecker&lt;/str&gt;
       &lt;str name="buildOnOptimize"&gt;true&lt;/str&gt;
       &lt;str name="buildOnCommit"&gt;true&lt;/str&gt;
   &lt;/lst&gt;
   &lt;!-- a spellchecker that uses a different distance measure --&gt;
   &lt;lst name="spellchecker"&gt;
       &lt;str name="name"&gt;jarowinkler&lt;/str&gt; 
       &lt;str name="field"&gt;spell&lt;/str&gt;
       &lt;str name="distanceMeasure"&gt;org.apache.lucene.search.spell.JaroWinklerDistance&lt;/str&gt;
       &lt;str name="spellcheckIndexDir"&gt;./spellchecker2&lt;/str&gt;
   &lt;/lst&gt;

   &lt;!-- a file based spell checker --&gt;
   &lt;lst name="spellchecker"&gt;
       &lt;str name="classname"&gt;solr.FileBasedSpellChecker&lt;/str&gt;
       &lt;str name="name"&gt;file&lt;/str&gt;
       &lt;str name="sourceLocation"&gt;spellings.txt&lt;/str&gt;
       &lt;str name="characterEncoding"&gt;UTF-8&lt;/str&gt;
       &lt;str name="spellcheckIndexDir"&gt;./spellchecker&lt;/str&gt;
   &lt;/lst&gt;
&lt;/searchComponent&gt;</pre>
The queryAnalyzerFieldType in the above configuration is important. Make sure the field _productname_spell_ is of same type and the way you configure Analyzers and filters in that field type defines what goes inside the spell check dictionary.

Now we need to setup the spell checker request handler which will be serving all the queries, create a new request handler in solrconfig.xml as described below. There is a good chance that you might need to change the values based on your requirement. If you looking forward to use the file based spell checker, just replace the _spellcheck.dictionary_ with **file**.
<pre lang="xml" escaped="true">&lt;requestHandler name="/spell" class="solr.SearchHandler" startup="lazy"&gt;
    &lt;lst name="defaults"&gt;
        &lt;str name="df"&gt;productname&lt;/str&gt; &lt;!--The default field for spell checking. --&gt;
        &lt;str name="spellcheck.dictionary"&gt;default&lt;/str&gt; &lt;!--default or file or jarowinkler as mentioned above. --&gt;
        &lt;str name="spellcheck"&gt;on&lt;/str&gt;
        &lt;str name="spellcheck.extendedResults"&gt;true&lt;/str&gt; 
        &lt;str name="spellcheck.count"&gt;10&lt;/str&gt;
        &lt;str name="spellcheck.maxResultsForSuggest"&gt;5&lt;/str&gt; 
        &lt;str name="spellcheck.collate"&gt;true&lt;/str&gt;
        &lt;str name="spellcheck.collateExtendedResults"&gt;true&lt;/str&gt; 
        &lt;str name="spellcheck.maxCollationTries"&gt;10&lt;/str&gt;
        &lt;str name="spellcheck.maxCollations"&gt;5&lt;/str&gt; 
    &lt;/lst&gt;
 &lt;arr name="last-components"&gt;
     &lt;str&gt;spellcheck&lt;/str&gt;
 &lt;/arr&gt;
&lt;/requestHandler&gt;</pre>
Now reload the SOLR configuration of the core by visiting the URL,
<pre>http://{SOLR IP}:{SOLR PORT}/solr/admin/cores?action=RELOAD&amp;core={CORE NAME}</pre>
If you do not have multiple cores, just restart the tomcat service. If you know any better way to reload the configuration, please leave a comment.

Once the reload/restart is completed without errors, index the documents and make sure the documents index without errors. If you are using mysql and not using dataimporthandler to import documents into SOLR, you should try [importing mysql into SOLR](http://www.arunchinnachamy.com/apache-solr-mysql-data-import/ "Apache SOLR – SOLR MySQL Data Import").

<span style="color: #ff0000;"></span>

Now you can check the workings of Solr Spellchecker by visiting the url,
<pre>http://{SOLR IP}:{SOLR PORT}/solr/{CORE NAME}/spell?spellcheck=true&amp;qt=spellchecker&amp;spellcheck.accuracy=0.8&amp;spellcheck.collate=true&amp;fl=*%2Cscore&amp;extendedResults=true+&amp;q={YOUR QUERY}</pre>
In case you have single core, remove the CORE NAME from the URL. If you try to query with any wrong spelling, the SOLR returns a new section _spellcheck_ with the correct spelling.
In my case, I have few words like Mobiles, Television, Phones, Books, Cameras etc indexed in SOLR. When i execute,
<pre>http://{SOLR IP}:{SOLR PORT}/solr/{CORE NAME}/spell?spellcheck=true&amp;qt=spellchecker&amp;spellcheck.accuracy=0.8&amp;spellcheck.collate=true&amp;fl=*%2Cscore&amp;extendedResults=true+&amp;q=color teleision</pre>
I get the following output with corrected spelling for television.
<pre lang="xml" escaped="true">&lt;response&gt;
    &lt;lst name="responseHeader"&gt;
        &lt;int name="status"&gt;0&lt;/int&gt;
        &lt;int name="QTime"&gt;0&lt;/int&gt;
    &lt;/lst&gt;
    &lt;result name="response" numFound="0" start="0" maxScore="0.0"/&gt;
    &lt;lst name="spellcheck"&gt;
        &lt;lst name="suggestions"&gt;
            &lt;lst name="teleision"&gt;
                &lt;int name="numFound"&gt;1&lt;/int&gt;
                &lt;int name="startOffset"&gt;7&lt;/int&gt;
                &lt;int name="endOffset"&gt;16&lt;/int&gt;
                &lt;int name="origFreq"&gt;0&lt;/int&gt;
                &lt;arr name="suggestion"&gt;
                    &lt;lst&gt;
                        &lt;str name="word"&gt;television&lt;/str&gt;
                        &lt;int name="freq"&gt;1&lt;/int&gt;
                    &lt;/lst&gt;
                &lt;/arr&gt;
            &lt;/lst&gt;
            &lt;bool name="correctlySpelled"&gt;false&lt;/bool&gt;
            &lt;str name="collation"&gt;color television&lt;/str&gt;
        &lt;/lst&gt;
    &lt;/lst&gt;
&lt;/response&gt;</pre>

Play around with field analyzers and filters to get the desired result and what you want to index. Visit solr [spellcheckcomponent WIKI page](http://wiki.apache.org/solr/SpellCheckComponent "Solr WIKI") for more configuration details. Visit my other articles on [SOLR Installation](http://www.arunchinnachamy.com/apache-solr-installation-and-configuration/ "The Seach Platform, Apache SOLR Installation and Configuration") and [SOLR DataImportHandler configuration](http://www.arunchinnachamy.com/apache-solr-mysql-data-import/ "Apache SOLR – SOLR MySQL Data Import").

Leave a comment if you find this useful. If you come across any errors, paste the error. I will try my best to answer your queries. 
