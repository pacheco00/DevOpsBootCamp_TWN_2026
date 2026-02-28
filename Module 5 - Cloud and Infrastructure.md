# DevOpsBootCamp_TWN_2026
Preparation for DIgital Badge Certificate


# Setup server on Azure

## Create Ubuntu Server
* Select create virtual machines
* Select Subscription, Resource Group
* Set VM name, region, availability options, security type, image (Ubuntu Server 24.04), VM Architecture, size
* Account administrator, SSH public key, username, SSH public key source, type, key pair name, Inbound ports (HTTP, HTTPS, SSH)
* Create VM

## Connect to VM
* Download SSH key

```
azuser@linuxe00:~$ pwd
/home/azuser
```

## Install packages
* apt update
* java 17
* gradle

```
azuser@linuxe00:~$ java --version
openjdk 17.0.18 2026-01-20
OpenJDK Runtime Environment (build 17.0.18+8-Ubuntu-124.04.1)
OpenJDK 64-Bit Server VM (build 17.0.18+8-Ubuntu-124.04.1, mixed mode, sharing)
azuser@linuxe00:~$ gradle --version
openjdk version "17.0.18" 2026-01-20
OpenJDK Runtime Environment (build 17.0.18+8-Ubuntu-124.04.1)
OpenJDK 64-Bit Server VM (build 17.0.18+8-Ubuntu-124.04.1, mixed mode, sharing)

------------------------------------------------------------
Gradle 4.4.1
------------------------------------------------------------

Build time:   2012-12-21 00:00:00 UTC
Revision:     none

Groovy:       2.4.21
Ant:          Apache Ant(TM) version 1.10.14 compiled on September 25 2023
JVM:          17.0.18 (Ubuntu 17.0.18+8-Ubuntu-124.04.1)
OS:           Linux 6.14.0-1017-azure amd64
```

## Deploy and run application artifact
- in local computer
* https://gitlab.com/twn-devops-bootcamp/latest/05-cloud/java-react-example
* cd java-react-example
* gradle build

- connecto to cloud server
* ssh root@ip

- copy jarfile from local computer to linux on cloud
```
$ scp -i linuxe00_key.pem java-react-example.jar azuser@20.102.80.25:/home/azuser
java-react-example.jar   
```

* java -jar java-react-example.jar (application started)
```
azuser@linuxe00:~$ java -jar java-react-example.jar 

  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/

 :: Spring Boot ::                (v3.5.5)

2026-02-28T00:41:56.360Z  INFO 4770 --- [           main] com.coditorium.sandbox.Application       : Starting Application using Java 17.0.18 with PID 4770 (/home/azuser/java-react-example.jar started by azuser in /home/azuser)
2026-02-28T00:41:56.365Z  INFO 4770 --- [           main] com.coditorium.sandbox.Application       : No active profile set, falling back to 1 default profile: "default"
2026-02-28T00:41:56.480Z  INFO 4770 --- [           main] .e.DevToolsPropertyDefaultsPostProcessor : For additional web related logging consider setting the 'logging.level.web' property to 'DEBUG'
2026-02-28T00:41:58.435Z  INFO 4770 --- [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat initialized with port 7071 (http)
2026-02-28T00:41:58.466Z  INFO 4770 --- [           main] o.apache.catalina.core.StandardService   : Starting service [Tomcat]
2026-02-28T00:41:58.468Z  INFO 4770 --- [           main] o.apache.catalina.core.StandardEngine    : Starting Servlet engine: [Apache Tomcat/10.1.44]
2026-02-28T00:41:58.776Z  INFO 4770 --- [           main] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring embedded WebApplicationContext
2026-02-28T00:41:58.779Z  INFO 4770 --- [           main] w.s.c.ServletWebServerApplicationContext : Root WebApplicationContext: initialization completed in 2294 ms
2026-02-28T00:41:59.367Z  INFO 4770 --- [           main] o.s.b.a.w.s.WelcomePageHandlerMapping    : Adding welcome page: class path resource [static/index.html]
2026-02-28T00:41:59.986Z  INFO 4770 --- [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat started on port 7071 (http) with context path '/'
2026-02-28T00:42:00.040Z  INFO 4770 --- [           main] com.coditorium.sandbox.Application       : Started Application in 4.892 seconds (process running for 5.965)

* open port 7071
* open app publicip:7071
* java -jar java-react-example.jar & (detached mode)
* ps aux | grep java
azuser@linuxe00:~$ ps aux | grep java
azuser      4804  2.3 14.6 2330484 135056 pts/0  Sl   00:43   0:06 java -jar java-react-example.jar

* apt install net-tools
* netstat -lpnt
azuser@linuxe00:~$ netstat -lpnt
(Not all processes could be identified, non-owned process info
 will not be shown, you would have to be root to see it all.)
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
tcp        0      0 127.0.0.53:53           0.0.0.0:*               LISTEN      -
tcp        0      0 127.0.0.54:53           0.0.0.0:*               LISTEN      -
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      -
tcp6       0      0 :::7071                 :::*                    LISTEN      4804/java
tcp6       0      0 :::22                   :::*                    LISTEN      -

azuser@linuxe00:~$ curl http://20.102.80.25:7071/
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>Countries</title>
</head>
<body>
  <div id="root"></div>
  <script src="//cdnjs.cloudflare.com/ajax/libs/systemjs/0.19.34/system.js"></script>
  <script>
    System.config({
      transpiler: 'babel',
      packages: {
        './app': { defaultExtension: 'js' }
      },
      map: {
        'react-dom': '//cdnjs.cloudflare.com/ajax/libs/react/15.3.1/react-dom.min.js',
        'react': '//cdnjs.cloudflare.com/ajax/libs/react/15.3.1/react.min.js',
        'babel': '//cdnjs.cloudflare.com/ajax/libs/babel-core/5.8.38/browser.min.js',
        'redux': '//cdnjs.cloudflare.com/ajax/libs/redux/3.6.0/redux.min.js',
        'react-redux': '//cdnjs.cloudflare.com/ajax/libs/react-redux/5.0.1/react-redux.min.js',
        'redux-thunk': '//cdnjs.cloudflare.com/ajax/libs/redux-thunk/2.1.0/redux-thunk.min.js'
      }
    });
    System
      .import('./script.js')
      .catch(err => console.error('Global error:', err));
  </script>
</body>
</html>
```

## Create linux user
* adduser student
* set password
* add user to sudo group
* usermod -aG sudo student

## Switch user
* su - student
* pwd
* exit

```
student@linuxe00:~$ pwd
/home/student
```

## Connecto to server with student
* ssh student@ip (doesn't work)
* ssh root@ip -> su - student
* on  local cat ~/.ssh/id_rsa.public
* on cloud server -> home/student mkdir .ssh
* sudo vim .ssh/authorized_keys -> copy public key from cat -> exit
* ssh student@ip

```
$ ssh -i linuxe00_key.pem student@20.102.80.25
Welcome to Ubuntu 24.04.3 LTS (GNU/Linux 6.14.0-1017-azure x86_64)
```