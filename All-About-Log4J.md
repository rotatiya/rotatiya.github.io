#### What is Log4J
Apache Log4j is a Java-based logging utility library.It is part of the Apache Logging Services, a project of the Apache Software Foundation. Log4j is one of several Java logging frameworks.
Please note that Log4j 1.x is not recomended at all.
The Apache Logging Services™ Project Management Committee (PMC) has announced that the Log4j™ 1.x logging framework has reached its end of life (EOL) and is no longer officially
supported.
Logging behavior using Log$j can be controlled by editing a configuration file, without touching the application binary(JAR).

#### current Version
Latest version of Apache Log4j is [2.17.1](https://logging.apache.org/log4j/2.x/index.html)
You can refer latest version [here](https://logging.apache.org/)

#### Microsoft reply to Log4J:
We have scanned AFD, Application Gateway, WAF services (all SKUs) and determined that by default Azure App Service or Azure Spring Cloud platform itself do not have this Log4j 
vulnerability, and do not distribute Log4J in the managed runtimes such as Tomcat, Java SE, JBoss EAP, or the Functions Runtime. 
However, your applications may use Log4J and be susceptible to this vulnerability. (In other words, if your application is using the problematic log4j version you may 
potentially be impacted by this vulnerability).

Official MSRC post and full details: [Microsoft’s Response to CVE-2021-44228 Apache Log4j 2 – Microsoft Security Response Center](https://msrc-blog.microsoft.com/2021/12/11/microsofts-response-to-cve-2021-44228-apache-log4j2/)

#### Who should worry
Basically if you are not using this logging library in your Application you don't need to do any changes, the notification you've received was to bring attention to this problem
and check if your Java Application is not using this library version (Apache log4j Versions: 2.0 <= 2.14.1) with the exploit.

#### Package that may use Log4J
- org.apache.logging.log4j
- java.util.logging

 
#### Where & Who generally uses Log4J
Log4J is logging library design by apache server and the application hosted on apacje server can make use of it. THis library can either use by application developer or monitor solutions. for e.g. Azure monitor solution service, App Insight may use Log4J in their SDK. If you're not aware of log4j implemention then its work to check your all configuration files and some piece of code in your devops pipe line artifact:
<dependency>
    <groupId>org.apache.logging.log4j</groupId>
    <artifactId>log4j-core</artifactId>
    <version>2.16.0</version>
</dependency>

#### Azure Application Insights & Log4j
Application Insights 3.x Java agent - does not log to log4j2, or pull in log4j2 transitively.
Application Insights 2.x SDK - does not log to log4j2, but does have one artifact applicationinsights-logging-log4j2 which pulls in log4j2-core transitively for the application to use and send logs to Application Insights.
If you are using the applicationinsights-logging-log4j2 artifact, make sure that you are bringing your own version of log4j-core.
If you are using the applicationinsights-logging-log4j2 artifact and unsure whether you are bringing your own version of log4j-core, update the applicationinsights-logging-log4j2 artifact to 2.6.4 and add the latest log4j-core dependency to your pom.xml, e.g.
<dependency>
    <groupId>org.apache.logging.log4j</groupId>
    <artifactId>log4j-core</artifactId>
    <version>2.16.0</version>
</dependency>


#### Remediation
If you confirm that your Application is in fact using the problematic package this is our recommendations to mitigate/avoid the exploit:
Check if your Server.xml Configuration file and if you're using the Affected Apache log4j Versions: 2.0 <= 2.14.1
If that is the case, you can follow the Apache TSG to mitigate the issue:
- https://www.lunasec.io/docs/blog/log4j-zero-day/#permanent-mitigation
- https://www.lunasec.io/docs/blog/log4j-zero-day/#temporary-mitigation

Our recommendation to mitigate is as follows:
#### Recommended approach – 
immediately upgrade Log4j2 to version 2.15.0 from all older version.
If above cannot be done and you are running Log4j2 version 2.10 and above, you can do either of the following two
- Set log4j2.formatMsgNoLookups to true
- Set environment variable  LOG4J_FORMAT_MSG_NO_LOOKUPS to true
- If log4j2 version is less than 2.10 then you will need to remove JndiLookup class from classpath. Do this via zip -q -d log4j-core-*.jar org/apache/logging/log4j/core/lookup/JndiLookup.class.

The details of above recommendation can be found in [Log4j2 status page](https://logging.apache.org/log4j/2.x/security.html)
