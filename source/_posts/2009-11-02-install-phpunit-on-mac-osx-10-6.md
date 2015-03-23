---
title: Install phpUnit on Mac OSX 10.6
author: Goran Jurić
layout: post
categories:
  - PHP
tags:
  - PHP
  - phpUnit
  - unit testing
  - Zend Studio
highlight: true
---
Here is another quick How-to so I don&#8217;t have to look it up next time.

Open up Terminal.app and enter &#8220;sudo su&#8221; and enter your password when asked (I guess you must be using an account with administrator privileges).

~~~
pear upgrade PEAR
pear channel-discover pear.phpunit.de
pear install --alldeps phpunit/PHPUnit
~~~

That&#8217;s it. phpunit binary should be in your path now.

p.s.

For some reason phpUnit that is being used by Zend Studios (6.1) does not have mbstring extension compiled and that makes it unusable for me. I guess there is a way to force PHP interpreter that is being used in the Zend Studio to run with the mbstring extension, but this is a faster approach. Hudson or phpUnderControl (whichever you like) will take care of the code coverage reports.