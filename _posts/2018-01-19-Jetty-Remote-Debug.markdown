---
layout: post
title: Jetty Remote Debug JIdea
date: 2018-01-19
---  

## I have found 2 easy ways to debug jetty app server remotely:

---  

### 1. When you need to debug application on **local** machine:

   1. GoTo `Maven Project` -> `Plugins` -> `jetty`
   2. Right click on `jetty:run` -> `Debug`

---  

### 2. When you need to debug application on **remote** machine:

   1. Start jetty server in debug mode:  
   `mvnDebug jetty:run`
   2. Create and start remote debug configuration:  
   `-agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=8000`


---  


Source: https://stackoverflow.com/questions/15111366/debug-web-app-in-intellij-webapp-built-by-maven-run-by-jetty