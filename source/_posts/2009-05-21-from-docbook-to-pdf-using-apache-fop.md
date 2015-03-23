---
title: From DocBook to PDF using Apache FOP
author: Goran Jurić
layout: post
categories:
  - Documentation
tags:
  - apache fop
  - docbook
  - Documentation
  - encoding
  - fonts
  - fop
  - utf-8
  - xsl
highlight: true
---
Creating user manuals for the software you are building is an important task. Sometimes it is a project requirement but more often than that it is just more efficient having a document to which you can refer users to and stop waisting you precious time explaining the fundamentals of content management systems to novice users instead of actually doing what you are payed for.

Since I do not like to repeat myself I wanted a system that is capable of generating documentation in variety of formats, PDF being the most important one.

[DocBook][1] is the first thing that came to mind, but as it is usually the case the things are not so simple as they should be. After playing fore the most part of the day with DocBook and various utilities I decided to write it down for future reference.<!--more-->

I will not contemplate on the DocBook syntax as there are various sources on the internet that will teach you how to use the DocBook syntax for writing. The place that got me started was a [blog post by Pádraic Brady][2]. I am using the directory structure he created as well as sample files, with little modifications. Take a look ad DocBook samples he provides.

For starters I do not use any PHP code in my documentation so I do not have the need for Phing tasks that take care of syntax highlighting program listings (yet). Other modifications I made include dropping the DocBook DTD from the manual.xml. That is the line that in his sample starts with &#8220;<!DOCTYPE book PUBLIC &#8220;-//OASIS//DTD DocBook XML V5.0//EN&#8221; &#8230;&#8221;.

Another thing I needed to add was language information for my <book> so for Croatian language it now looks like this:

~~~
<book lang="hr" version="5.0" xmlns="http://docbook.org/ns/docbook"
    xmlns:xlink="http://www.w3.org/1999/xlink"
    xmlns:xi="http://www.w3.org/2001/XInclude"
    xmlns:svg="http://www.w3.org/2000/svg"
    xmlns:mml="http://www.w3.org/1998/Math/MathML"
    xmlns:html="http://www.w3.org/1999/xhtml"
    xmlns:db="http://docbook.org/ns/docbook">
~~~

"hr" is a two-letter country code for Croatia. I will explain later why the language attribute is important.

To create a PDF from your DocBook XML files you will need to download:

*   [Apache FOP][3]
*   [FOP XML Hyphenation Patterns][4]
*   [DocBook XSL stylesheets][5]

If you also want to validate the syntax of your XML files you will need the [DocBook 5.0 DTD][6]. I will not go into details on validating DocBoox syntax, but it is recommended that you validate your files because I guess it is faster than invoking Apache FOP to generate you PDF and realising that somewhere at the end of your documentation there is a syntax error.

## DocBook XSL

After you extract the archive of DocBoox XSL stylesheets you will get a myriad of files and folders. In the &#8220;fo&#8221; folders you will find XSL documents used for transformation of your XML documents, and the file you will have to pass to Apache FOP is located in &#8220;fo/docbook.xsl&#8221;.

Files located in the &#8220;common&#8221; folder contain common terms used in creation of a book based on the language attribute of your book. As a specified that my book is written in Croatian Apache FOP will use &#8220;common/hr.xml&#8221; to look for terms such as &#8220;Table of Content&#8221;, &#8220;Chapter&#8221;, etc. This XML file is the one you want to edit if you want to change the output strings used for creation of the PDF document.

## Apache FOP

Since the Apache FOP will probably complain about hyphenation, [grab the hyphenation patterns][7] and copy the jar file fop-hyph.jar into the lib directory of the your FOP installation (the place where you have extracted Apache FOP).

## Encoding (UTF-8) problems with Apache FOP

There are 14 most used fonts that you can use in your PDF files that do not require you to embedd the font file itself into the PDF. The problems is that these fonts do not have support for all the characters you are probably using if your are converting a document that is not written in English. Accented characters are replaced with hashes if you do not use a font that supports multi-byte characters.

There is an explanation about [embedding fonts in the PDF using Apache FOP at their site][8] but this documentation is written for Apache FOP 0.94 and since 0.95 (the current stable release) it is not necessary to create a font metrics file and trying to create one will probably result in an error.

To get the font file embedded into our PDF we need to tell Apache FOP to embed the fonts. Open up the &#8220;conf/fop.xconf&#8221; file with a text editor and find the part of the configuration file that is enclosed in <renderer mime=&#8221;application/pdf&#8221;><fonts> &#8230; </font></renderer> and insert

~~~
<font kerning="yes" embed-url="/Users/gog/Desktop/fop-0.95/times-roman.ttf">
     <font-triplet name="Times-Roman" style="" weight="normal" />
     <font-triplet name="Times-Roman" style="" weight="bold" />
</font>
~~~

Of course you will have to replace the embed-url with the path to Times New Roman.ttf file on your system.

## Wrap it up

And finally to generate the PDF navigate to the folder where you extracted the Apache FOP archive and run:

~~~
./fop -c conf/fop.xconf -xml manual.xml -xsl docbook.xsl -param body.font.family Times-Roman -param title.font.family Times-Roman -pdf manual.pdf
~~~

In case you are using Windows you probably just need to replace &#8220;./fop&#8221; at the end of the line with &#8220;fop.bat&#8221;. Ofcourse you will have to change manual.xml with the full path to your DocBook document and docbook.xsl with the full path to that file located in the DocBook XSL stylesheets inside the &#8220;fo&#8221; folder.

The important part to notice is that we needed to tell the XML processor to use Times-Roman font for body and title text and that we configured Apache FOP to embed the &#8220;Times-Roman&#8221; font into the PDF document.

I guess there is a better way to customize the output of your PDF files without using &#8220;-param&#8221; during the invocation of the FOP. Maybe I&#8217;ll come back to it later, for now this works for me.

 [1]: http://www.docbook.org/
 [2]: http://blog.astrumfutura.com/archives/369-Writing-Professional-Looking-Documentation-With-Docbook,-PHP,-Phing-and-Apache-FOP-Part-1-Getting-Started.html
 [3]: http://xmlgraphics.apache.org/fop/download.html
 [4]: http://offo.sourceforge.net/hyphenation/
 [5]: http://sourceforge.net/project/showfiles.php?group_id=21935&package_id=16608
 [6]: http://www.docbook.org/xml/5.0/docbook-5.0.zip
 [7]: http://offo.sourceforge.net/hyphenation/fop-stable/installation.html
 [8]: http://xmlgraphics.apache.org/fop/0.94/fonts.html