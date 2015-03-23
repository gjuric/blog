---
title: UTF-8 encoding and the TinyMCE SpellCheck plugin
author: Goran Jurić
layout: post
categories:
  - PHP
tags:
  - spellcheck
  - tinymce
  - utf8
---
If you have tried using the the [TinyMCE spell check plugin][1] on UTF-8 encoded page with the PSpellShell adapter you probably noticed that the implementation is broken. The spelling suggestions do not contain properly encoded UTF-8 characters.

Since the creators of TinyMCE moved to [Github][2] it was a great opportunity to try and push these changes to the master branch. If you need this you can look at the [changes that need to be made to the PSpellShell adapter][3] or just clone my [fork of the spellcheck plugin][4].

p.s. Have I mentioned how awesome Git and Github are?

 [1]: http://wiki.moxiecode.com/index.php/TinyMCE:Plugins/spellchecker
 [2]: http://github.com/
 [3]: http://github.com/gjuric/tinymce_spellchecker_php/commit/5d8430ccb116ae83218a48c0dc36088ecc250596
 [4]: http://github.com/gjuric/tinymce_spellchecker_php