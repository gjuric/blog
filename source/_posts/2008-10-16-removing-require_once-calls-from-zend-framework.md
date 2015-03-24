---
title: 'Removing require_once() calls from Zend Framework'
author: Goran Jurić
layout: post
highlight: true
---
If you are using Zend\_Loader to load your classes in Zend Framework, there is no need for all the require\_once() calls that are littered all over ZF files.

[Some acctually report big improvements][1] in responsiveness of their applications and increase in number of transactions per second their hardware can handle because require_once() is quite an expensive operation, especially if the required file is already included prior to the call.<!--more-->

If you are wondering how to comment out all the require_once() calls from your <span><span>Zend</span></span> Framework without doing it by hand here is the solution. Navigate to the folder where your installation of ZF resides on the server and just enter this into your shell:

<pre><span style="text-decoration: line-through;">find ./ -type f -exec <span><span>sed</span></span> -i 's/require_once/\/\/require_once/' {} \;</span></pre>

My box is running PHP 5.2.6 with APC 3.0.19 and Zend Framework 1.7 PR and my include path is optimized for use with ZF. Stripping require_once() calls didn&#8217;t make a drastic impact on my setup and average performance gain was around 3 requests per second (from 63 to 66).

### Update (May 16, 2009)

Since the release of Zend Framework 1.8 there is a script to strip requre_once calls from ZF in the [ZF Performance guide][2]. This script does not remove require\_once calls to the new Zend\_Loader_Autoloader component.

<pre>% cd path/to/ZendFramework/library
% <span style="text-decoration: line-through;">find . -name '*.php' -not -wholename '*/Loader/Autoloader.php' -print0 | \
xargs -0 sed --regexp-extended --in-place 's/(require_once)/\/\/ \1/g'</span></pre>

### Update 2 (October 3, 2010)

Thanks to Tom Anderson for noticing that this does not work since ZF 1.10.8 , the new script for stripping require_once calls is:

~~~bash
% cd path/to/ZendFramework/library
% find . -name '*.php' -not -wholename '*/Loader/Autoloader.php' \
-not -wholename '*/Application.php' -print0 | \
xargs -0 sed --regexp-extended --in-place 's/(require_once)/\/\/ \1/g'
~~~

Note that you must use autoloading if you strip the require_once from the ZF project.

 [1]: http://www.whitewashing.de/blog/articles/73
 [2]: http://framework.zend.com/manual/en/performance.classloading.html#performance.classloading.striprequires