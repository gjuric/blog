---
title: Set filenames with Nginx secure download module
author: Goran Jurić
layout: post
categories:
  - Linux
tags:
  - lighttpd
  - nginx
highlight: true
---
When we switched from [Lighttpd][1] to [Nginx][2] a couple of months ago we were faced with an annoying problem.

Paying subscribers to our site have an option of downloading PDF files of the magazine. With Lighttpd we were using [mod_secdownload][3] to provide this functionality without exposing the files to the public. We compiled Nginx with the [secure\_download\_module][4] and it kinda worked.

Files were downloading as expected but the file names where all messed up. Download links where generated for each user and they looked something like this: */pdf/645.pdf/097ac16cb19ff6c163d6f813fdd44b4d/4b283bfa* and the browser saved the file to the users hard drive with the file name of *4b283bfa*. Since the file name didn&#8217;t have an extension it was impossible for the OS to know that is has to use a PDF reader to open the file.

Finally we managed to force the file name to the browser (download client) with a configuration directive that looks something like this:

~~~nginx
# PDF download
location /pdf {
    secure_download on;
    secure_download_secret $request_addr;
    secure_download_path_mode file;
    root /path/to/dir/with/pdfs

    # Extract the name of the PDF
    if ($uri ~ "^/pdf/(.+\.pdf)$") {
        set $filename $1;
    }

    # Set appropriate headers
    add_header Content-Disposition "attachment; filename=$filename";
}
~~~

And that&#8217;s all there is to it.

 [1]: http://www.lighttpd.net/
 [2]: http://wiki.nginx.org/
 [3]: http://redmine.lighttpd.net/wiki/lighttpd/docs:modsecdownload
 [4]: http://wiki.nginx.org/NginxHttpSecureDownload