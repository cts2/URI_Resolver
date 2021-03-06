URI Resolver REST Interface For CTS2
====================================

Table of Contents
-----------------
* [Installation Overview](#installation_overview)
* [Configure Database Connection Values](#db_connection)
* [Configure User Accounts](#user_accounts)
* [Errors/Warning](#errors_warnings)

##<a name="installation_overview"></a>Installation Overview
1. Install war file, <code>URI_Resolver-0.0.1-SNAPSHOT.war</code> to JBOSS or Tomcat server.
2. Configure database connection values (see below)
3. Configure user accounts (see below)
4. Test sample using url: [http://localhost/URI_Resolver-0.0.1.SNAPSHOT](http://localhost/URI_Resolver-0.0.1.SNAPSHOT)

##<a name="db_connection"></a>Configure Database Connection Values
##### To use default database connection values
1. Install MySQL server on the local machine to run on port 3306
2. Create a MySQL account: `uriuser` with the password `uriuser`
3. Create a MySQL database named `uriresolver`, give uriuser permissions to write to this database.
4. Download `uriresolver.sql` from github project. File is located in [src/main/resources](https://github.com/cts2/URI_Resolver/tree/master/src/main/resources)
5. Using MySQL, import data from the uriresolver.sql export file into the uriresolver database

##### To use user specified database connection values
1. Create a folder: `<USER_HOME>/.cts2_uri`
	* USER_HOME refers to the home directory of the account running JBOSS server (i.e. user will be `jboss` if running on ubuntu)
2. Create a file: `uriresolver.properties` and store this file in the USER_HOME/.cts2_uri folder
3. Edit uriresolver.properties to contain the following:
<pre><code>db.driverClassName=\<Driver for database\>
db.url=\<URL to connect to database\>
db.username=\<Database username\>
db.password=\<Database username\'s password\>
db.editable=false
log.level=0
log.filelocation=
</code></pre>
4. Example uriresolver.properties file:
<pre><code>jdbc.driverClassName=com.mysql.jdbc.Driver
db.url=jdbc:mysql://localhost:3306/uriresolver
db.username=uriuser
db.password=uriuser
db.editable=false
log.level=0
log.filelocation=
</code></pre>

##### To use In-memory database
An in-memory database will be utilized if the two previous options are not available.
Nothing is required to configure this database.  It will only exist during the REST connection.
	

##<a name="user_accounts"></a>Configure User Accounts
##### To use default user accounts
1. Create the uriresolver.properties file, described above, and set db.editable to true.
2. By default one account will be available with `cts2/mayo+nih` as the username/password.  

##### To use user specified user accounts
1. Create the uriresolver.properties file, described above, and set db.editable to true.
2. Create `user.properties` (save in USER_HOME/.cts2_uri folder)
3. Enter username/passwords in user.properties file, one per line with a space seperating the username/password.
4. Example user.properties file:
<pre><code>jdoe abcd1234
bspringsteen born2run
</code></pre>
5. If USER_HOME/.cts2_uri/user.properties file exist then only accounts listed in this file will be enabled.
The default cts2 account, mentioned above, will not be enabled.

##<a name="errors_warnings"></a>Errors & Warnings
###MySQL Errors
    13:06:52,434 ERROR [edu.mayo.cts2.uriresolver.dao.UriJDBCTemplate] (http--0.0.0.0-8180-1) Error connecting to data source: Access denied for user 'uriuser'@'localhost' (using password: YES)
    13:06:52,438 ERROR [edu.mayo.cts2.uriresolver.controller.ResolveURI] (http--0.0.0.0-8180-1) Unknown error while checking tables exist: Access denied for user 'uriuser'@'localhost' (using password: YES)
    13:06:52,439 INFO  [edu.mayo.cts2.uriresolver.controller.ResolveURI] (http--0.0.0.0-8180-1) Creating an in memory database
    
This error is generated when the program cannot connect a local mysql server with the given credentials.  These errors will only be generated when attempting to connect to MySQL, not the in-memory database.
Errors can be ignored unless you were expecting the MySQL connection to be successfull.  If this is the case check the credentials provided in the error messages.

###MySQL Warnings
    13:06:52,261 WARN  [org.springframework.beans.factory.config.PropertyPlaceholderConfigurer] (http--0.0.0.0-8180-1) Could not load properties from URL [file:/home/jboss/.cts2_uri/uriresolver.properties]: /home/jboss/.cts2_uri/uriresolver.properties (No such file or directory)

This warning is generated when the user's uriresolver.properties files is not found.  At this point the program will use the in-memory database solution.


###JBOSS Warnings
Spring Framework Warning (created when war file is loaded into JBoss.  This warning can be ignored.)

<pre><code>13:05:56,077 WARN  [org.jboss.as.ee] (MSC service thread 1-3) JBAS011006: Not installing optional component org.springframework.web.context.request.async.StandardServletAsyncWebRequest due to exception: org.jboss.as.server.deployment.DeploymentUnitProcessingException: JBAS011054: Could not find default constructor for class org.springframework.web.context.request.async.StandardServletAsyncWebRequest
at org.jboss.as.ee.component.ComponentDescription$DefaultComponentConfigurator.configure(ComponentDescription.java:606)
at org.jboss.as.ee.component.deployers.EEModuleConfigurationProcessor.deploy(EEModuleConfigurationProcessor.java:81)
at org.jboss.as.server.deployment.DeploymentUnitPhaseService.start(DeploymentUnitPhaseService.java:113) [jboss-as-server-7.1.1.Final.jar:7.1.1.Final]
at org.jboss.msc.service.ServiceControllerImpl$StartTask.startService(ServiceControllerImpl.java:1811) [jboss-msc-1.0.2.GA.jar:1.0.2.GA]
at org.jboss.msc.service.ServiceControllerImpl$StartTask.run(ServiceControllerImpl.java:1746) [jboss-msc-1.0.2.GA.jar:1.0.2.GA]
at java.util.concurrent.ThreadPoolExecutor$Worker.runTask(ThreadPoolExecutor.java:886) [rt.jar:1.6.0_12]
at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:908) [rt.jar:1.6.0_12]
at java.lang.Thread.run(Thread.java:619) [rt.jar:1.6.0_12]
</code></pre>

###JBOSS Errors
Sun Rowset ClassNotFound Error (Must be resolved, see below)

<pre><code>12:55:20,312 ERROR [org.apache.catalina.core.ContainerBase.[jboss.web].[default-host].[/URI_Resolver-0.0.1-SNAPSHOT].[uri_resolver]] (http--0.0.0.0-8180-3) Servlet.service() for servlet uri_resolver threw exception: java.lang.ClassNotFoundException: com.sun.rowset.CachedRowSetImpl from [Module "deployment.URI_Resolver-0.0.1-SNAPSHOT.war:main" from Service Module Loader]
at org.jboss.modules.ModuleClassLoader.findClass(ModuleClassLoader.java:190)
at org.jboss.modules.ConcurrentClassLoader.performLoadClassUnchecked(ConcurrentClassLoader.java:468)
at org.jboss.modules.ConcurrentClassLoader.performLoadClassChecked(ConcurrentClassLoader.java:456)
...
</code></pre>


* To fix this error edit `$JBOSS_HOME/modules/sun/jdk/main/module.xml` file and add the following to paths section.
<pre><code>path name="com/sun/rowset/"
path name="com/sun/rowset/internal/"
path name="com/sun/rowset/providers/"
</code></pre>

* If this does not correct the error then download `rowset` jar file from Oracle sun website and extract to `$JBOSS_HOME/modules/com/sun/rowset`


### You will find more details in the following links:
*  [LexEVS Home](https://tracker.nci.nih.gov/browse/LEXEVS)
*  [LexEVS Servers and API's](https://wiki.nci.nih.gov/x/0BwhAQ)
*  [Source Code](https://github.com/NCIP/URI_Resolver>)



