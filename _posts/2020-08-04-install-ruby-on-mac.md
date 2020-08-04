---
layout: post
title: How to install Ruby on Mac
date: 2020-08-04
category: mac
---  

## That's funny but that's tricky:
```bash
# Instructions on off site don't help. I couldn't get a security cert: https://rvm.io/rvm/security
# And I had to install it manually:
curl -sSL https://rvm.io/mpapis.asc | gpg --import -
curl -sSL https://rvm.io/pkuczynski.asc | gpg --import -

# Then I've installed RVM:
# 1. get rvm
\curl -sSL https://get.rvm.io | bash
# 2. install openssl to rvm
rvm pkg install openssl
# 3. install ruby
# rvm install 2.3.1 --with-openssl-dir=$HOME/.rvm/usr
# instruction from above did not work for me, so I had to add rubygems version, and absolute path to .rvm dir:
rvm install 2.3.3 --rubygems 2.7.10 --with-openssl-dir=/Users/spanko/.rvm/usr
```

All that steps were necessary because of openssl issues, I dunno why but otherwise I coldn't install any gem and was getting an error like:
```bash
$ gem install bundler
ERROR:  While executing gem ... (Gem::Exception)
    Unable to require openssl, install OpenSSL and rebuild Ruby (preferred) or use non-HTTPS sources
```

You might also need to reinstall RVM to update openssl-dir:
```bash
rvm implode
# this command should remove RVM completely, but anyway check that ~/.rvm dir was removed
```

Some people say that Ruby is broken on MacOS.  
I am partially agree with such conclusion.  
And its installation process is purely documented. (there are some articales on ruby-lang off site, and on rubymine site, but the did not help me). Have a look a them: 
1. Installing Ruby - https://www.ruby-lang.org/en/documentation/installation/
2. Set up Ruby dev env - https://www.jetbrains.com/help/ruby/set-up-a-ruby-development-environment.html
