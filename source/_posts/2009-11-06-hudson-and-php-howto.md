---
title: Hudson and PHP Howto
author: Goran Jurić
layout: post
categories:
  - PHP
tags:
  - ci
  - continuous Integration
  - Debian
  - hudson
  - PHP
  - phpundercontrol
  - Ubuntu
  - unit testing
---
After a couple of weeks of playing with [phpUnderControl][1] and reading some nice reviews of [Hudson][2] I decided to give Hudson a try. And boy am I glad I did.

Hudson definitively won me over with its clean interface and easy to setup tasks. Even plugin installation is a breeze and you can even do it right out of the comfort of your browser.

I found a nice tutorial about setting up Hudson with PHP [here][3] but I had some question marks hovering over my head during the installation so I have decided to write a more detailed installation tutorial.

If you are running a Debian flavored distribution (yes I mean Ubuntu) take a look at the [tutorial][4], you should have [Hudson working with your PHP projects][4] in no time.

Bonus points if you integrate your Hudson installation to act as your staging server.

 [1]: http://www.phpundercontrol.org/about.html
 [2]: http://hudson-ci.org/
 [3]: http://toptopic.wordpress.com/2009/02/26/php-and-hudson/
 [4]: http://gogs.info/wiki/debian/hudson