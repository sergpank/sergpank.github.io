---
layout: post
title: VirtualBox network modes
date: 2017-10-17
---

## VirtualBox network modes ##

Sometimes, looking neccessary network mode by restarting ant testing VM becomes really annoing.

Here is a table that describes all network modes in VirtualBox:

|             | VM ↔ Host | VM1 ↔ VM2 | VM → Internet | VM ← Internet   |
|-------------|:---------:|:---------:|:-------------:|:---------------:|
| Host-only   | +         | +         | –             | –               |
| Internal    | –         | +         | –             | –               |
| Bridged     | +         | +         | +             | +               |
| NAT         | –         | –         | +             | Port forwarding |
| NAT Network | –         | +         | +             | Port forwarding |

Source: VirtualBox documentation - https://www.virtualbox.org/manual/ch06.html#networkingmodes
