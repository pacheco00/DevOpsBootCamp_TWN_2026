# Installation Help for Windows
* Install IntelliJ Community Edition
https://www.jetbrains.com/idea/download/other/

* IntelliJ, clone repository
https://gitlab.com/twn-devops-bootcamp/latest/04-build-tools/java-maven-app

* Install Java
File/Project structure/Project-sdk - Java 17

* Add java to  "Path" and set "JAVA_HOME" environment variable
File/Settings/Tools/Terminal - shell path - windows/system32
Environment Variables - System Variables
- Path - add - C:\Users\pache\.jdks\ms-17.0.18\bin
- New - Variable name: JAVA_HOME - Variable value: C:\Users\pache\.jdks\ms-17.0.18

* Install Maven
Binary zip archive - unzip
Environment variables - System variables Path - Edit - C:\Users\pache\Downloads\apache-maven-3.9.13-bin\apache-maven-3.9.13\bin
mvn package

* Setup Java Gradle Project in IntelliJ
Download Gradle Binary only - https://docs.gradle.org/current/userguide/installation.html#ex-installing-manually
Unzip, copy in C Gradle\gradle-9.4.0
Environment Variaples - Path - Edit - new - C:\Gradle\gradle-9.4.0\bin

* Configure IntelliJ location
Open IntelliJ - Customize - All settings - search gradle - Build tools - gradle - distributions - local installation - check previous location configured

* Apps
Java-app: https://gitlab.com/twn-devops-bootcamp/latest/04-build-tools/java-app
IntelliJ - Terminal - gradle build

* Setup Node project in IntelliJ
React-nodejs-app: https://github.com/techworld-with-nana/react-nodejs-example
Install Node - Environment automatically configured
Move to api folder -C:\Users\pache\Downloads\BC_NANA\04\react-nodejs-example\api
npm install - node server.js












 