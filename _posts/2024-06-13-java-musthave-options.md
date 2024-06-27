---
layout: post
title: Three must have options for any java backend app
date: 2024-06-13
---  

For historical reasons we need to add these three parameters to any java server application (Tomcat, SpringBoot, etc...)

```bash
# set to headless, just in case
-Djava.awt.headless=true

# ensure UTF-8 encoding by default (e.g. filenames)
-Dfile.encoding=UTF-8

# Entropy source for randomness
-Djava.security.egd=file:/dev/urandom
```

Explication of each of these flags deserves a separate articale, which can be easily found ;)
