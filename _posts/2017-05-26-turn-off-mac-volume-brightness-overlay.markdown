---
layout: post
title: Turn off the Mac's volume/brightness screen overlays/HUDs
date: 2017-05-26
---

Whenever you adjust the screen brightness or audio volume on a Mac you’ll see a small grey box overlay appear showing the current setting. This is sometimes known as a head-up display, or HUD.

Some people find it annoying, especially if they’re watching a movie because it can obscure the screen for several seconds before it disappears.

Here’s how to turn it off.

## Turning off temporarily ##

To turn off the on-screen volume and brightness displays, open a Terminal window (you’ll find it in the Utilites folder of the Applications list) and paste in the following, which should be copied and pasted as a single line:

``launchctl unload -F /System/Library/LaunchAgents/com.apple.BezelUI.plist``

Restoring the overlay/HUD is easy: you’ll just need to log out and back in again, or simply restart your Mac.

**For macOS 10.12 Sierra:** [Disable System Integrity protection](https://developer.apple.com/library/content/documentation/Security/Conceptual/System_Integrity_Protection_Guide/ConfiguringSystemIntegrityProtection/ConfiguringSystemIntegrityProtection.html), then:

``launchctl unload -F /System/Library/LaunchAgents/com.apple.OSDUIHelper.plist``

Don't forget to enable System Integrity protection when you're done.

## Turning off permanently ##

To turn off the on-screen volume and brightness displays permanently, again open a Terminal window (as described above) and paste-in the following, which again should be copied and pasted as a single line:

``launchctl unload -wF /System/Library/LaunchAgents/com.apple.BezelUI.plist``

To turn them back on again, open the Terminal window and paste-in the following, which again should be copied and pasted as a single line, before logging out and back in again, or restarting your Mac,:

``launchctl load -wF /System/Library/LaunchAgents/com.apple.BezelUI.plist``

**For macOS 10.12 Sierra:** [Disable System Integrity protection](https://developer.apple.com/library/content/documentation/Security/Conceptual/System_Integrity_Protection_Guide/ConfiguringSystemIntegrityProtection/ConfiguringSystemIntegrityProtection.html), then:

``launchctl unload -wF /System/Library/LaunchAgents/com.apple.OSDUIHelper.plist``

You can now enable System Integrity protection again -- your settings will persist.

* * *

Source: 

[http://www.mackungfu.org/](http://www.mackungfu.org/get-rid-of-the-volume-brightness-screen-overlays-mac "Turn off the Mac's volume/brightness screen overlays/HUDs")

[https://www.stackexchange.com](https://apple.stackexchange.com/questions/16849/how-do-i-disable-the-volume-control-overlay "How do I disable the volume control overlay?")
