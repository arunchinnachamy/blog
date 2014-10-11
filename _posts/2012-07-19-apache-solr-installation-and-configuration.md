---
layout: post
title: The Seach Platform, Apache SOLR Installation and Configuration
excerpts: Apache Solr - Introduction and Solr Installation process and how to configure multiple cores in Debian or Ubuntu systems.
---

[Solr](http://lucene.apache.org/solr/ "solr homepage"), the enterprise search platform built over Apache Lucene project is a stunning indexing engine with capabilities like full text search, faceted search, hit highlighting and caching capabilities. It is written in Java and very powerful with less memory print. If you are looking forward to find what solr is, the [solr project page ](http://lucene.apache.org/solr/ "Solr Project page")is the right place to start.

Recently, I have been looking into how to use solr for our search functionality. Initially, we were hesitant to use Solr as we were afraid we lack java knowledge. But You can perform all supported functionalities through the REST-like API. Now it is time to explain how to install and configure solr. so here is the Solr Installation guide.

### Solr Installation:

I will explain how to install Tomcat6 and Solr in Debian or Ubuntu.  In order to install Tomcat6, Run the following command in terminal. It is assumed you are installing in localhost, if not, replace the url with IP Address.
<pre>sudo apt-get install tomcat6 tomcat6-admin</pre>
To confirm Tomcat6 installation, Open
<pre lang="text">http://localhost:8080</pre>
Get and Install the latest stable version of solr,
<pre lang="bash">wget http://apache.cict.fr/lucene/solr/3.6.0/apache-solr-3.6.0.tgz
tar xvfz apache-solr-3.6.0.tgz
cd apache-solr-3.6.0
cp dist/apache-solr-3.6.0.war /var/lib/tomcat6/webapps/solr.war
cp -fr example/solr /var/lib/tomcat6/
chown -R tomcat6:tomcat6 /var/lib/tomcat6/solr</pre>
Now restart the Tomcat to load the Solr Application.
<pre lang="bash"> sudo /etc/init.d/tomcat6 restart</pre>
You have successfully installed and running the example application came in the package. You can check whether it is working or not, open
<pre lang="text">http://localhost:8080/solr</pre>
. Now you should see the Solr Welcome page. This is just the example application. Now lets configure the installation to have two cores.

### Configuration of Solr Cores:

Now that the installation of SOLR is complete, Lets see how you can configure multiple cores in same installation. Each core will have its own configuration and indexes. In order to create cores, edit
<pre lang="bash">/var/lib/tomcat6/solr/solr.xml</pre>
The following text will create two cores named core1 and core2 in the same solr installation. Make a copy of the Solr example directory as a template for your project which makes it easy to start with.
<pre lang="xml" escaped="true">&lt;solr persistent="false"&gt;
 &lt;cores adminPath="/admin/cores" defaultCoreName="core1"&gt;
     &lt;core name="core1" instanceDir="core1" /&gt;
     &lt;core name="core2" instanceDir="core2" /&gt;
 &lt;/cores&gt; 
&lt;/solr&gt;</pre>
The core1 and core2 are the respective directories where the configuration files and indexes will be searched. To simplify the configuration, Copy the directories (bin conf data lib) in
<pre lang="bash">/var/lib/tomcat6/solr/</pre>
in to the core1 and core2. So now the core1 and core2 exactly the same configuration.

if you go to
<pre lang="text">http://localhost:8080/solr</pre>
the page will present you with options to select the core to use. Now the installation of SOLR and setting up the cores is done but still the data needs to be imported and indexed to use the functions. For more information about the cores, visit [Core Admin page](http://wiki.apache.org/solr/CoreAdmin "Core Admin").

Look forward for my other posts on how to configure a core and also on how to import data from MySQL to SOLR.

UPDATE: If you are looking for importing MySQL Tables into SOLR index, have a look into my other post on [SOLR MySQL Data Import](http://www.arunchinnachamy.com/apache-solr-mysql-data-import/ "Apache SOLR – SOLR MySQL Data Import").

UPDATE: If you are looking for SOLR Tutorials and other topics related to SOLR, Visit [SOLR Tutorials](http://www.installationpage.com/solr/solr-tutorials/ "SOLR Tutorials") at InstallationPage.com
