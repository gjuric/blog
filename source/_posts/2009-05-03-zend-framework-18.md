---
title: Zend Framework 1.8
author: Goran Jurić
layout: post
categories:
  - Zend Framework
tags:
  - Zend Framework
highlight: true
---
Yesterday Zend Framework 1.8 was released and since I was ill and didn&#8217;t have much else to do I decided to have a look and make necessary changes to port our company CMS to the new version.

The new autoloader Zend\_Loader\_Autoloader is just what I was looking for to easily group the forms and models with my application modules. I definitively recommend to read a great article about the new autoloader at [Zend Developer Zone][1].

The whole process of migrating from ZF 1.7.7 was quite painless I just had to replace:

~~~php
Zend_Loader::registerAutoload();
~~~

with

~~~php
$loader = Zend_Loader_Autoloader::getInstance();
$loader->registerNamespace('MyApp_');
~~~

Zend\_Log\_Writer_Mail is finally in the stable distribution and It took only couple of lines of code to add this to our application logger so now when stuff go terribly wrong I at least know I will be getting an email about it.

You can view the full list of changes in the 1.8 release [here][2].

I look forward to playing with Zend_Navigation and see if we how could we use it to cleanup some of the mumbo-jumbo we are currently doing with ACLs and navigational elements.

Zend\_Validate\_Db_RecordExists doesn&#8217;t look like much, but it makes me happy to remove some custom code because it is now supported in the framework.

 [1]: http://devzone.zend.com/article/4525-Developing-a-Comprehensive-Autoloader
 [2]: http://devzone.zend.com/article/4524-Zend-Framework-1.8.0-Released