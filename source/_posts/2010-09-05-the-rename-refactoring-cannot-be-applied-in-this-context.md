---
title: The Rename refactoring cannot be applied in this context
author: Goran Jurić
layout: post
categories:
  - PHP
tags:
  - ide
  - netbeans
  - refactoring
---
I have been using NetBeans for a couple of months now, and I must admit that except for a quirk here and there the overall experience of switching from Zend Studio was a pleasant one.

Since switching to 6.9 I had some trouble using the integrated Refactoring utility. There error I got was: `The Rename refactoring cannot be applied in this context.`

The solution is a simple one, use the built in shortcut &#8220;Ctrl + R&#8221; (or &#8220;Cmd + R&#8221; on a Mac).

The other thing that bothered me is that when you select a method or class and use the context menu to navigate to the declaration of the method/class Netbeans doesn&#8217;t do a thing. To make this work the method/class name **must not** be selected (or you can use the + click shortcut).