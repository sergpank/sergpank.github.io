---
layout: post
title:  "Java memory leak detection"
date:   2025-05-10 12:48:45 +0300
categories: java bash linux 
---

# Java Memory leak detection

## run GC and dump only reacheable objects
sudo -u tomcat jmap -dump:live,file=heapdump_live.hprof <PID>

## don't run GC and dump everything
sudo -u tomcat jmap -dump:format=b,file=heapdump_all.hprof <PID>

## if nothing is fond or dump size is significantly smaller than memory amount consumed by app, restart app with this flag
-XX:NativeMemoryTracking=detail

## tool for heap dump analysis - Eclipse MAT
https://eclipse.dev/mat/