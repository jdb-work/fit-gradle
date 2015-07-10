# Everest Application Security

Vulnerabilities
---------------

- __JBoss4.2.1.GA__ -
This version of JBoss AS has a known vulnerability in its default JMX console security constraint configuration.
If applied as suggested, the restraint fails to secure http verbs other than GET and POST.

- __JBoss services__ -
Additional steps for securing and/or removing JBoss services are detailed in the following sections.

- The __Java Runtime Environment__
needs to be updated to a supported release.
*Tested with jdk7u45.*

Setup location reference
------------------------

__JAR dependencies__

```
$JBOSS_HOME/servers/default/libs
```

__EAR and \*-ds.xml files__

```
$JBOSS_HOME/servers/default/deploy
```

__Pricing\*.xml files__

```
C:\
```

__Transfer content__
*(Added after successful deployment)*
```
$JBOSS_HOME/servers/default/tmp/deploy/tmp*everest.ear-contents/everest-Web-exp.war
```

Securing the JMX Console
------------------------

JBoss AS community releases prior to version 6.0.0.M3 are potentially vulnerable to a flaw detailed in CVE-2010-0738.

http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2010-0738

Redhat addressed the flaw in a security update to their EAP :
> The JMX Console configuration only specified an authentication requirement for requests that used the GET and POST HTTP "verbs". A remote attacker could create an HTTP request that does not specify GET or POST, causing it to be executed by the default GET handler without authentication. This release contains a JMX Console with an updated configuration that no longer specifies the HTTP verbs. This means that the authentication requirement is applied to all requests. (CVE-2010-0738)

A remedy for the affected community release versions is available at the JBoss community website.

https://community.jboss.org/wiki/SecureTheJmxConsole



Securing additional JBoss services
----------------------------------

#### JbossWS

*$JBOSS_HOME/server/default/deploy/jbossws.sar/jbossws.beans/META-INF/jboss-beans.xml*

Comment out the property element for webServiceHost:

```XML
    <bean name="ServiceEndpointManager" class="org.jboss.ws.core.server.ServiceEndpointManager">
    
    <!--
        The WSDL, that is a required deployment artifact for an endpoint, has a <soap:address>
        element which points to the location of the endpoint. JBoss supports rewriting of that SOAP address.
      
        If the content of <soap:address> is a valid URL, JBossWS will not rewrite it unless 'alwaysModifySOAPAddress' is true.
        If the content of <soap:address> is not a valid URL, JBossWS will rewrite it using the attribute values given below.
        
        If next line (webServiceHost) is commented, JBossWS uses requesters protocolo, host and port when rewriting the <soap:address>.
    -->
    <!--  
        Commented out to avoid leaking jboss.bind.address value.
        <property name="webServiceHost">${jboss.bind.address}</property>
    __jdb__ 2014-01-21 
    -->
    <property name="alwaysModifySOAPAddress">true</property>
```

Removing additional JBoss services
----------------------------------

Removed the following unused services:

__JMX Console__
```
$JBOSS_HOME/server/default/deploy/jmx-console.war
```

__Http Invoker__
```
$JBOSS_HOME/server/default/deploy/http-invoker.sar
```

__Web Console__
```
$JBOSS_HOME/server/default/deploy/management
```
