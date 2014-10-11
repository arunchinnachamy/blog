---
layout: post
title: Apache SOLR – SOLR MySQL Data Import
excerpts: This post shows how to configure SOLR MySQL Data import. Also provides the possible commands to control DataImportHandler and document index.
---

Recently I wrote a blog post on How to [install and configure SOLR](http://www.arunchinnachamy.com/apache-solr-installation-and-configuration/ "The Seach Platform, Apache SOLR Installation and Configuration"). While I am planning to write series of posts on SOLR, I found it important to provide few extra details about Spell Checker, DataImportHandler and other similar areas which is not covered in the post. If you are like me who want to perform MySQL data import into SOLR as Documents, this post (Apache SOLR - SOLR MySQL Data Import) is for you.

If you want to configure Spell Checker in SOLR, visit my another post on [Index Based Spellchecker in SOLR](http://www.arunchinnachamy.com/apache-solr-spellchecker-configuration/ "Apache SOLR: SOLR Spellchecker and Configuration").

#### How to Import MySQL to SOLR?

Solr comes with **DataImportHandler** which provides a configuration driven way to load SOLR with the data imported from relational database or XML in both "full builds" and using incremental delta imports.

### <span style="color: #ff0000;"></span>

To configure DataImportHandler, Edit the solrconfig.xml file which can be found in _/var/lib/tomcat6/solr/conf_ if you have not configured multiple cores, else go to the solrconfig.xml in the conf directory inside your Core instance Directory. Add the request handler into the configuration file.
<pre lang="xml" escaped="true">&lt;requestHandler name="/dataimport" class="org.apache.solr.handler.dataimport.DataImportHandler"&gt;
&lt;lst name="defaults"&gt;
 &lt;str name="config"&gt;data-config.xml&lt;/str&gt;
&lt;/lst&gt;
&lt;/requestHandler&gt;</pre>
Now save and close the file. Next step is to create a file named **data-config.xml** and save it inside the conf directory along with solrconfig.xml. Add the following lines into the files.
<pre lang="xml" escaped="true">&lt;dataConfig&gt;
 &lt;dataSource type="JdbcDataSource" driver="com.mysql.jdbc.Driver"
                     url="jdbc:mysql://{DB_HOST}/{DB_NAME}" 
                        user="{DB_USER}" password="{DB_PASS}"/&gt;
 &lt;document&gt;
     &lt;entity name="id" 
          query="select col1,col2,col3 from mytable"&gt;
        &lt;field column="col1" name="solr_field1"/&gt;
        &lt;field column="col2" name="solr_field2"/&gt;
        &lt;field column="col3" name="solr_field3"/&gt;
     &lt;/entity&gt;
 &lt;/document&gt;
&lt;/dataConfig&gt;</pre>

This is the configuration for DataImportHandler in SOLR. Replace the values appropriately with Database credentials. In case of field section, you need to match the column name in the database table to the field name which you have configured in schema.xml of SOLR. In this case, it is assumed that the field names in your schema.xml are solr_field1, solr_field2 and solr_field3.

Last and final step is to copy the MySQL JDBC driver JAR file into the _lib_ directory in your SOLR home or CORE instance directory. You can download the driver file from [MySQL Download Page](http://dev.mysql.com/downloads/connector/j "mysql jdbc connector").

Once copied, you can run a full import by going to the URL
<pre>http://{SERVER IP}:{SERVER PORT}/solr/dataimport?command=full-import</pre>
if you have no cores else
<pre>http://{SERVER IP}:{SERVER PORT}/solr/{CORE NAME}/dataimport?command=full-import</pre>
If the server harasses you with errors, Check the latest log file of tomcat which in my case is under _/var/lib/tomcat6/logs/_. If you are not able to find the problem, feel free to post the error message. I will help if I can. Apart from full-import, You can also perform delta-imports.

**Delta Import in Solr**:

Instead of full imports, you can perform delta imports if your data size is huge. Mention the query which will get the delta or new documents after the last import. This is quite useful if your index size is huge and takes lot of time.

For Example, look at the sample query below
<pre lang="xml" escaped="true">&lt;entity name="id" pk="ID" query="SELECT * FROM table1"
 deltaImportQuery="SELECT * FROM table1 WHERE id = '${dataimporter.delta.id}'"
 deltaQuery="SELECT id FROM table1 WHERE last_modified &gt; '${dataimporter.last_index_time}'"&gt;
&lt;/entity&gt;</pre>
Now that the SOLR MySQL data import is complete, there are other commands which will let you control the DataImportHandler. The important ones are listed below.

### <span style="color: #ff0000;"></span>

To start **Delta imports **hit the url, Without cores,
<pre>http://{SERVER IP}:{SERVER PORT}/solr/dataimport?command=delta-import</pre>
with cores,
<pre>http://{SERVER IP}:{SERVER PORT}/solr/{CORE NAME}/dataimport?command=delta-import</pre>
To know the **status of the current command**,
Without cores,
<pre>http://{SERVER IP}:{SERVER PORT}/solr/dataimport</pre>
with cores,
<pre>http://{SERVER IP}:{SERVER PORT}/solr/{CORE NAME}/dataimport</pre>
To **reload the data config** file without restarting the SOLR,
Without Cores,
<pre>http://{SERVER IP}:{SERVER PORT}/solr/dataimport?command=reload-config</pre>
With Cores,
<pre>http://{SERVER IP}:{SERVER PORT}/solr/{CORE NAME}/dataimport?command=reload-config</pre>
You can even **abort the import**,
Without Cores,
<pre>http://{SERVER IP}:{SERVER PORT}/solr/dataimport?command=abort</pre>
With Cores,
<pre>http://{SERVER IP}:{SERVER PORT}/solr/{CORE NAME}/dataimport?command=abort</pre>

Please provide your comments if you find this useful or if you come across any issues. 

[Updated on August 13, 2012]
**Error Debugging:**
If you come across the following error when configuring the SOLR with mysql,
<pre>Caused by: java.sql.SQLException: Illegal value for setFetchSize().</pre>
You can resolve the issue by adding batchSize="-1" to your data source declaration like shown below.
<pre lang="xml" escaped="true">&lt;dataConfig&gt;
 &lt;dataSource type="JdbcDataSource" driver="com.mysql.jdbc.Driver"
                     url="jdbc:mysql://{DB_HOST}/{DB_NAME}" 
                        user="{DB_USER}" password="{DB_PASS}" batchSize="-1"/&gt;
 &lt;document&gt;
     &lt;entity name="id" 
          query="select col1,col2,col3 from mytable"&gt;
        &lt;field column="col1" name="solr_field1"/&gt;
        &lt;field column="col2" name="solr_field2"/&gt;
        &lt;field column="col3" name="solr_field3"/&gt;
     &lt;/entity&gt;
 &lt;/document&gt;
&lt;/dataConfig&gt;</pre>
