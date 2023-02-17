---
title: Google confused with 301 redirects
author: Goran Jurić
layout: post
categories:
  - Uncategorized
tags:
  - "301"
  - Google
  - Googlebot
  - redirect
  - SEO
  - URI
---
It has been almost I year since we launched [nacional.hr][1]. During the migration from the old content management we thoroughly went through the whole system and made sure that all the old URIs are still working. Although [cool URIs don&#8217;t change][2] sometimes you have no choice and that was the case with this site. We had to enhance some of the URIs for SEO purpose and some of them were just clashing with our URI scheme it we did not want to maintain a large list of legacy URIs in our rewrite rules.

We hooked a legacy URI &#8220;plugin&#8221; in our error controller so that checking for old URIs does not interfere with the loading times of our system unless really necessary and created 301 (permanent) redirects to our new URI scheme patted ourselves on the back an forgot all about it until recently.

While checking the access logs for some unusual traffic spikes we where getting we noticed that Google stills checks our old category URIs and follows the redirects that we have created. The most unusual things was that the URIs where checked very often. Links to our news category pages where crawled by Google a couple of times a minute during the course of couple of days. To add to the confusion the categories that are most often crawled our the ones that list articles from our print issue which get updated once a week, the ones that are updated many times a day get crawled at a regular pace.

Any ideas what could cause something like this?

 [1]: http://www.nacional.hr/
 [2]: http://www.w3.org/Provider/Style/URI