# DevOpsBootCamp_TWN_2026
Preparation for DIgital Badge Certificate


# Nexus

## Install Nexus
* Install java 17 - sudo apt install openjdk-17-jre-headless
* cd /opt
* Download Nexus Unix Archive
* wget https://download.sonatype.com/nexus/3/nexus-3.89.1-02-linux-x86_64.tar.gz
* tar -zxvf nexus-3....

## Create Nexus User
* adduser nexus
* check user folders pemissions
* chown -R nexus:nexus nexus-3.89.1-02
* chown -R nexus:nexus sonatype-work
* ls -l

## Set Nexus Configuration
* /opt - vim nexus-3.89.1-02/bin/nexus.rc
* run_as_user="nexus"
* su - nexus
* Start nexus service - /opt/nexus-3.72.0-04/bin/nexus start
* ps aux | grep nexus / netstat -lnpt
* port 8081

```
nexus@student:~$ ps aux | grep nexus
root       18964  0.0  0.1  21428  4744 pts/2    S    14:52   0:00 su - nexus
nexus      18994  0.0  0.1  19744  4392 pts/2    S    14:53   0:00 -bash
nexus      19424  173 37.2 6085976 1485116 pts/2 Sl   14:53   0:43 /opt/nexus-3.89.1-02/jdk/temurin_21.0.9_10_linux_x86_64/jdk-21.0.9+10/bin/java -server -Dnexus.installer.type=linux-x86-64 -Xms2703m -Xmx2703m -XX:+UnlockDiagnosticVMOptions -XX:+LogVMOutput -XX:LogFile=../sonatype-work/nexus3/log/jvm.log -XX:-OmitStackTraceInFastThrow -Dkaraf.home=. -Dkaraf.base=. -Djava.util.logging.config.file=etc/spring/java.util.logging.properties -Dkaraf.data=../sonatype-work/nexus3 -Dkaraf.log=../sonatype-work/nexus3/log -Djava.io.tmpdir=../sonatype-work/nexus3/tmp -Djdk.tls.ephemeralDHKeySize=2048 -Dfile.encoding=UTF-8 --add-reads=java.xml=java.logging --add-opens java.base/java.security=ALL-UNNAMED --add-opens java.base/java.net=ALL-UNNAMED --add-opens java.base/java.lang=ALL-UNNAMED --add-opens java.base/java.util=ALL-UNNAMED --add-opens java.naming/javax.naming.spi=ALL-UNNAMED --add-opens java.rmi/sun.rmi.transport.tcp=ALL-UNNAMED --add-exports=java.base/sun.net.www.protocol.http=ALL-UNNAMED --add-exports=java.base/sun.net.www.protocol.https=ALL-UNNAMED --add-exports=java.base/sun.net.www.protocol.jar=ALL-UNNAMED --add-exports=jdk.xml.dom/org.w3c.dom.html=ALL-UNNAMED --add-exports=jdk.naming.rmi/com.sun.jndi.url.rmi=ALL-UNNAMED --add-exports=java.security.sasl/com.sun.security.sasl=ALL-UNNAMED --add-exports=java.base/sun.security.x509=ALL-UNNAMED --add-exports=java.base/sun.security.rsa=ALL-UNNAMED --add-exports=java.base/sun.security.pkcs=ALL-UNNAMED -jar /opt/nexus-3.89.1-02/bin/sonatype-nexus-repository-3.89.1-02.jar
nexus      19711  200  0.1  22292  4684 pts/2    R+   14:53   0:00 ps aux
nexus      19712  0.0  0.0  17820  2372 pts/2    S+   14:53   0:00 grep --color=auto nexus

nexus@student:~$ netstat -lnpt
(Not all processes could be identified, non-owned process info
 will not be shown, you would have to be root to see it all.)
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
tcp        0      0 127.0.0.54:53           0.0.0.0:*               LISTEN      -                   
tcp        0      0 127.0.0.53:53           0.0.0.0:*               LISTEN      -                   
tcp        0      0 127.0.0.1:631           0.0.0.0:*               LISTEN      -                   
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      -                   
tcp        0      0 127.0.0.1:32775         0.0.0.0:*               LISTEN      -                   
tcp6       0      0 :::8081                 :::*                    LISTEN      19424/java          
tcp6       0      0 ::1:631                 :::*                    LISTEN      -                   
tcp6       0      0 :::22                   :::*                    LISTEN      -   

```

* connect to nexus
* default user ls /opt/sonatype-work/nexus3/ - admin.password
* default pass cat /opt/sonatype-work/nexus3/admin.password
* on web page, sign in - admin - password
* set up new password / enable anonymous access


## Publish Artifact to Repository
* create new user

Java Gradle App: https://gitlab.com/twn-devops-bootcamp/latest/06-nexus/java-app
Java Maven App: https://gitlab.com/twn-devops-bootcamp/latest/06-nexus/java-maven-app

* java-app - settings gradle - apply plugin: 'maven-publish'


* Gradle Project
```
C:\Users\Oscar\Downloads\IdeaProjects\java-app>gradle build
Starting a Gradle Daemon (subsequent builds will be faster)

[Incubating] Problems report is available at: file:///C:/Users/Oscar/Downloads/IdeaProjects/java-app/build/reports/problems/problems-report.html

Deprecated Gradle features were used in this build, making it incompatible with Gradle 10.

You can use '--warning-mode all' to show the individual deprecation warnings and determine if they come from your own scripts or plugins.

For more on this, please refer to https://docs.gradle.org/9.3.1/userguide/command_line_interface.html#sec:command_line_warnings in the Gradle documentation.

BUILD SUCCESSFUL in 1m 42s
5 actionable tasks: 5 executed
Consider enabling configuration cache to speed up this build: https://docs.gradle.org/9.3.1/userguide/configuration_cache_enabling.html
C:\Users\Oscar\Downloads\IdeaProjects\java-app>gradle publish

[Incubating] Problems report is available at: file:///C:/Users/Oscar/Downloads/IdeaProjects/java-app/build/reports/problems/problems-report.html

Deprecated Gradle features were used in this build, making it incompatible with Gradle 10.

You can use '--warning-mode all' to show the individual deprecation warnings and determine if they come from your own scripts or plugins.

For more on this, please refer to https://docs.gradle.org/9.3.1/userguide/command_line_interface.html#sec:command_line_warnings in the Gradle documentation.

BUILD SUCCESSFUL in 16s
2 actionable tasks: 2 executed
C:\Users\Oscar\Downloads\IdeaProjects\java-app>
```

* Maven Project
I excecuted all steps, I configures settings.xml on .m2 folder, I checked role permissions on nexus repository, but I couldn't upload the files to nexus.
I was working with intelliJ installes on Windows, Gradle Project finished ok.

```
[INFO] BUILD FAILURE
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  10.200 s
[INFO] Finished at: 2026-02-28T18:20:17-05:00
[INFO] ------------------------------------------------------------------------
[ERROR] Failed to execute goal org.apache.maven.plugins:maven-deploy-plugin:3.1.4:deploy (default-deploy) on project java-maven-app: Failed to deploy artifacts: Could not transfer artifact com.example:java-maven-app:pom:1.1.0-20260228.232007-1 from/to nexus-snapshots (http://192.168.100.90:8081/repository/maven-snapshots/): status code: 401, reason phrase: Unauthorized (401) -> [Help 1]                                           
[ERROR] 
[ERROR] To see the full stack trace of the errors, re-run Maven with the -e switch.
[ERROR] Re-run Maven using the -X switch to enable full debug logging.
[ERROR] 
[ERROR] For more information about the errors and possible solutions, please read the following articles:
[ERROR] [Help 1] http://cwiki.apache.org/confluence/display/MAVEN/MojoExecutionException
```




