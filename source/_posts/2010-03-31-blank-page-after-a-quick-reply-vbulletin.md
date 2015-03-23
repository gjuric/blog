---
title: Blank page after a quick reply (vBulletin)
author: Goran Jurić
layout: post
categories:
  - Uncategorized
tags:
  - adsense
  - vbulletin
highlight: true
---
While launching the Google Adsense on [Gameplay][1] and its forum (vBulletin 3.8) we ran into a strange issue while posting quick reply messages to the forum with Firefox. After hitting submit the page turned blank although the comment was apparently posted.

We have integrated our Adsense code into the postbit vBulletin template after the last post on the page using this syntax:

~~~
<if condition="$post['islastshown']">
     Adsense code here.
</if>
~~~

The problem happend because Quick Reply uses asynchronous javascript to submit the reply and render the new reply at the and of the page. Since we are embedding the Adsense code if the post is &#8220;last shown&#8221; this renderd the Google Adsense code twice and FF &#8220;broke&#8221;.

The fix is quite simple, you just have to check if the post is being sent as a response to the asynchronous request, so the new and working code looks like this:

~~~
<if condition="$post['islastshown'] AND !$GLOBALS['vbulletin']->GPC['ajax']">
    Adsense code here.
</if>
~~~

Of course this is not the only case which causes vBulletin forum to render a blank page. For a list of other possible reasons take a look at [this chapter in the  official documentation][2].

If you are interested you can also take a look at this exhaustive [list of conditionals][3] which you can use in your vBulletin templates.

 [1]: http://www.gameplay.hr/
 [2]: http://www.vbulletin.com/docs/html/blank_pages
 [3]: http://forum.vbulletinsetup.com/f18/vbulletin-template-conditionals-list-2185.html