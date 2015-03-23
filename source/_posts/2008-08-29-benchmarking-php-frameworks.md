---
title: '&#8220;Benchmarking&#8221; PHP frameworks'
author: Goran Jurić
layout: post
categories:
  - PHP
  - Programming
tags:
  - Frameworks
  - PHP
  - Zend Framework
---
Although I am not a [Drupal][1] user, the chance to visit [Drupalcon in Szeged][2] (Hungary) appeared and I couldn&#8217;t pass on that one. We got there a little bit late, but just in time to hear [Rasmus Lerdorfs][3] keynote speech [Simple is Hard][4]. There are some really good ideas for optimizing your applications performance and I strongly recommend it for every PHP developer.

There were also some things I really don&#8217;t agree that much. He showed a small PHP frameworks &#8220;benchmark&#8221; measuring the speed (response time and transactions per second) for each of this frameworks to output the simple HTML page printing out the &#8220;Hello world&#8221; string. [Zend Framework][5] (the framework of my choice) didn&#8217;t perform all that bad. [Symfony][6] was around 30% slower, and [Solar][7] was about 2 times faster. If you are really interested in the numbers have a look at the [slides from the session][4].

<!--more-->

The methodology was a little bit questionable. I believe that frameworks offering a caching solution out of the boxes should have been tested with the caching enabled.

Another thing he talked about was removing unnecessary require\_once() and include\_once() calls from the applications, and their impact on the frameworks speed, and how those calls are handled in PHP internally and how to set up your include path to minimize the impact).

Zend Framework didn&#8217;t do that well, as you can see on the graphs plotted out for every framework (dashed lines represent require_once() calls). The main reason for this is I guess that ZF is a loosely coupled framework. The other reason is probably that the ZF is still not mature enough to focus on the optimization issues while there is still so much other work to be done.

 [1]: http://drupal.org
 [2]: http://szeged2008.drupalcon.org/
 [3]: http://lerdorf.com/bio.php
 [4]: http://talks.php.net/show/drupal08
 [5]: http://framework.zend.com/
 [6]: http://www.symfony-project.org/
 [7]: http://solarphp.com/