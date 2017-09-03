---
layout: post
title: How to generate SSH key and connect to hosting via SSH
date: 2017-09-03
---

## Generate SSH key ##

1. Ggenerate the SSH key in the your hosting's control panel.

2. Download `private` SSH key.

3. Copy the key into `~\.ssh` folder.

4. Run the following command to add this identity to you rmachine:
	
	```ssh-add -K <key-file-name>```
		
	You will be prompted to type the password that you have set during the key generation in cPanel.

5. Use the cPanel user name to connect to you host by SSH:

{% highlight shell %}
ssh -p 1033 onemapmd@onemap.md
{% endhighlight %}

