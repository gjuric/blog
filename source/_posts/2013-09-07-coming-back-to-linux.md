---
title: Coming back to Linux
author: Goran Jurić
layout: post
categories:
  - Uncategorized
---
Five years ago I was using Ubuntu Linux at my job exclusively. After my new Macbook Pro arrived I have switched to OS X exclusively but I still had an old Windows 7 machine at home that was being used more or less as a NAS.

The time finally came to move from from proprietary OS to something free. As I am used to running Debian on the servers, the choice of Linux distribution was simple, Debian Wheezy.

Here are some of the things I had to do to make things work the way I wanted them to.

## Graphics

Installation guide for ATI cards is available on [Debians Wiki page][1]. For making fonts look nicer check the section on font smoothing that is also available on [Debians Wiki page on fonts][2].

## Mounting Windows drives

Since I am still not sure if this switch is going to be a permanent decision I have decided to keep my storage drives on the NTFS file system. Because of this I was forced to enter the root users password every time I wanted to mount them, this was a no go since I wanted to share those drives via NFS to my Mede8er media player.

For this I had to install ntfs-config and ntfs-3g packages.

<pre>apt-get install ntfs-config ntfs-3g disk-manager</pre>

Running ntfs-config gives you an option to Enable write support for external and internal devices. After checking this options all the drives where available for reading and writing in the /media folder.

## Exporting shares via NFS

Since my media player (Mede8er) supports NFS shares I opted to use it instead of Samba. The setup is simple once you get the export options right. To install NFS on the server

<pre>apt-get install nfs-kernel-server nfs-common portmap</pre>

Instead of portmap, rpcbind will be installed but that doesn&#8217;t matter.

Edit /etc/exports and add:

<pre>/media     10.29.10.0/255.255.255.0(rw,sync,root_squash)</pre>

Replace the IP address and netmask with the ones in your network.

Restart NFS with

<pre>/etc/init.d/nfs-kernel-server restart</pre>

Or if NFS is already running you can just run

`exportfs -a`

On the Mede8er, go to NFS, Add and enter the IP address of your host and &#8220;/media&#8221; as the folder.

## OSX Time Machine backup

Since I still have two Apple machines, the idea is to use this Debian NAS as a Time Machine backup server so I do not have to rely on plugging in the USB hard drive. As far as I can see there are lots on guides on how to accomplish this. I will update this section once I get the time to implement this.

 [1]: https://wiki.debian.org/ATIProprietary
 [2]: https://wiki.debian.org/Fonts#Subpixel-hinting_and_Font-smoothing