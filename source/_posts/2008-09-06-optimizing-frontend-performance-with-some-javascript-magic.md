---
title: Optimizing frontend performance with some Javascript magic
author: Goran Jurić
layout: post
categories:
  - Optimization
tags:
  - frontend
  - javascript
  - jQuery
  - js
  - openx
  - optimize
  - performance
highlight: true
---
So, you know Yahoo!&#8217;s [Best Practices for Speeding Up Your Web Site][1] by hart and you have already implemented most of the tips on your web site but you are still not satisfied with the performance bottlenecks caused by your banner serving services ([OpenX][2], formerly known as OpenAds and PhpAdsNew in my case) and other stuff you need to use but have no control over it.

Fear no more!

<!--more-->

The idea is not actually mine, so I have to give credit to [Nivas][3] and the post about how they solved the issue. There solution was to load the banner in a hidden div at the end of the page, and use jQuery&#8217;s appendTo() method to move the content to the place the banner is supposed to show. You can read more about their method [here][4].

The problem (and the reason I&#8217;m writing this post) is that their method has a dependency on jQuery 1.2 (which is the last official build of the jQuery which doesn&#8217;t have problems using this method. Patching jQuery and potentially breaking some other stuff in it wasn&#8217;t really an option.

Since I&#8217;m dependant on the latest jQuery (1.2.6 at the time of the writing) I had to look for some other solution.

Chris Coyier&#8217;s article on [How To Fix Video Slowing Down Your Page Load Time][5] gave me an idea. Why don&#8217;t I move the banners invocation code to a separate file and try to work around the appendTo() issues.

So, here it goes:

1.  If you are using OpenX for serving your banners, first you have to get the invocation code for your banner (go for iframe version, worked for me).
2.  Put the invocation code into a separate file, let&#8217;s call it **bannerInvocation.html**
3.  Create an empty div with an ID (ID&#8217;s need to be unique on the page, but you already knew that) in your HTML that is going to be used as a banner placeholder until we fetch the banner. You can style it, and use a preloader GIF as a background if you like.
4.  Just before the closing </body> tag insert something like this: 

~~~html
<script type="text/javascript">
//<![CDATA[
$(window).bind("load", function() {
    $('#banner').load('bannerInvocation.html');
});
//]]>
</script>
~~~


## Pros for using this method

1.  Banners start loading only after all other element on the page finished loading, and that includes images as well!
2.  Page loads are faster because important element&#8217;s of the page (images!) don&#8217;t have to compete for bandwidth with ads.
3.  Users can use the site even before the ads load.

## Cons

1.  You are actually making two extra requests. One to make an AJAX call to the bannerInvocation.html and another one to load the content of the `<iframe>` that gets loaded from the bannerInvocation.html.
2.  This method requires you to use the jQuery library (or some other JS library for fetching an external file and inserting it into the DOM) and that is a quite an overkill if you don&#8217;t already use the library on your page.
3.  If you are using XHTML strict DTD you are inserting a deprecated element (iframe) into your page. You probably could change the invocation code to use <object> element instead of an `<iframe>` but AFAIK there are some issues accross different browsers and browser versions in implementing the <object> element.  
    If it&#8217;s all about the &#8220;*look ma, my sites validates using W3C validator, and I can put this cool badge on my site*&#8221; don&#8217;t worry. Your page will still show as valid because the iframe gets injected into the DOM via an AJAX call.

Of course, you could (and should) set an [Expires header][6] for the bannerInvocation.html file so after the first page, the file will be cached into the visitors browser.

If you can think of a better solution, do share. I would really like to see something like this working with the pure JS invocation code for the ads. I really don&#8217;t like iframes, and somethimes they are not an option you can use.

 [1]: http://developer.yahoo.com/performance/rules.html
 [2]: http://www.openx.org/
 [3]: http://www.nivas.hr/blog/
 [4]: http://www.nivas.hr/blog/2008/04/02/trackers-banners-analyzers-and-other-shit-that-slows-down-your-site-but-helps-you-earn-money
 [5]: http://css-tricks.com/how-to-fix-video-slowing-down-page-load-time/
 [6]: http://developer.yahoo.com/performance/rules.html#expires