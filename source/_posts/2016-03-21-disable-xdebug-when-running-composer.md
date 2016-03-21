---
title: Disable Xdebug when running Composer
author: Goran Jurić
layout: post
highlight: true
categories:
  - PHP
---
If you have Xdebug enabled when running CLI applications but do not want it to be loaded when running composer here is what you need to do.

Let us imagine that you have composer installed in `/usr/local/bin/composer` and PHP in `/usr/local/bin/php`

1. Move /usr/local/bin/composer to /usr/local/bin/composer.phar
2. Create /usr/local/bin/composer
3. Make it executable by running "chmod +x /usr/local/bin/composer"
4. Fill it with the following content:

~~~php
#!/usr/bin/env bash

/usr/local/bin/php -n -d extension=/usr/local/Cellar/php55-mcrypt/5.5.17/mcrypt.so /usr/local/bin/composer.phar "$@"
~~~

The `-n` flag tells PHP not to load a php.ini file and `-d extension=...` specifies the only INI entry we want to use so we can load the mcrypt extension that we need in order to be able to use Composer.

Note that the location of your `mcrypt.so` depends on your platform and the way how you installed PHP. You can find it by running:

~~~bash
find / -name mcrypt.so
~~~

Now, you will not be seeing the message "You are running composer with xdebug enabled. This has a major impact on runtime performance. See https://getcomposer.org/xdebug" anymore and composer will be faster.