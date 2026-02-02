---
layout: post
title:  "Where are CodeDeploy logs?"
date:   2025-05-10 14:47:45 +0300
categories: aws codedeploy amazon linux 
---

# Where are CodeDeploy logs?
Default location for CodeDeploy logs on `Amazon Linux` is:  
``` bash
/opt/codedeploy-agent/deployment-root/...
# or maybe :
/var/log/aws/codedeploy-agent/...
# depends on what are you looking for
```
* I repitedly google it tooo many times

# Tailing agent logs is very useful for debugging codedeploy-agent issues
```bash
tail /var/log/aws/codedeploy-agent/codedeploy-agent.log -f
```
* Sometimes it is the only way to find out why deployment fails

# Cleanup codedeploy-agent cache
```bash

cd /opt/codedeploy-agent/deployment-root/
rm -rf *
watch -n 1 "ls -R /opt/codedeploy-agent/deployment-root/"
```
* Remove all agent cache, start new deployment, and watch filestructure update