---
title: Dual display setup with Nvidia on Ubuntu 8.04
author: Goran Jurić
layout: post
categories:
  - Linux
  - Ubuntu
tags:
  - display
  - Linux
  - monitor
  - Ubuntu
---
Setting up multiple monitors under Linux can sometimes be quite a daunting task. TwinView, Xinerama, restricted drivers, editing configuration files, etc. doesn&#8217;t sound user friendly at all.

After you install Ubuntu and boot it the first time, you will get a notification in the tray asking you if you would like to install nVidia restricted drivers. They don&#8217;t get installed by default because the source code for this drivers isn&#8217;t open source. Since we don&#8217;t care about that, and just want the damn thing to work install the drivers.<!--more-->

Next, install the **NVIDIA X Server Settings** using the Add/Remove application or type **sudo apt-get install nvidia-settings** into your terminal.

<div id="attachment_32" class="wp-caption alignnone" style="width: 510px">
  <a href="http://gogs.info/wp-content/uploads/2008/09/nvidia-settings.png"><img class="size-full wp-image-32" title="nvidia-settings" src="http://gogs.info/wp-content/uploads/2008/09/nvidia-settings.png" alt="Add/Remove dialog" width="500" height="337" /></a><p class="wp-caption-text">
    Add/Remove dialog
  </p>
</div>

You can now run the NVIDIA X Server Setting from System -> Administration menu. All the changes you select there will be applied, but they will not be saved because saving them requires root privileges. The file that need to be changed (/etc/X11/xorg.conf) for the changes to become permanent can be changed only by the root user.

Fire up your terminal and first save a backup copy of your **xorg.conf** file:

<p style="padding-left: 30px;">
  <strong>sudo cp /etc/X11/xorg.conf /etc/X11/xorg.conf.backup</strong>
</p>

Now we can run the settings utility as root, enter:

<p style="padding-left: 30px;">
  <strong>sudo nvidia-settings</strong>
</p>

Select the X Server Display Configuration from the menu on the left and play around with the options, most of them are self explanatory except maybe for the almost all of the are self explanatory except maybe for the Configuration button.

<div id="attachment_34" class="wp-caption alignnone" style="width: 510px">
  <a href="http://gogs.info/wp-content/uploads/2008/09/nvidia_x_server_settings.png"><img class="size-full wp-image-34" title="nvidia_x_server_settings" src="http://gogs.info/wp-content/uploads/2008/09/nvidia_x_server_settings.png" alt="NVIDIA X Server Settings" width="500" height="424" /></a><p class="wp-caption-text">
    NVIDIA X Server Settings
  </p>
</div>

You can run the displays in two basic configurations: TwinView and Separate X screen. We will select TwinView because using separate X screens doesn&#8217;t allow you to drag application windows from one display to another, which kinda defeats the purpose of having a dual display setup.

When you are done configuring the displays and are satisfied with the output you get after hitting Apply it&#8217;s tame to save this changes to the X configuration files, so hit that button and you are ready, well, almost&#8230;

All of the dialogs where being placed in the center, showing one half on the left monitor and the other half on the other monitor. Maximizing windows would make them maximize across both of the screens, and the taskbar spanned across both of the monitors.

To fix this &#8220;little annoyance&#8221; I had to open up my terminal once again and enter:

<p style="padding-left: 30px;">
  <strong>sudo dpkg-reconfigure xorg</strong>
</p>

I restarted my X session (Ctrl + Alt + Backspace) and that was it, everything works as I expect. Application windows are located on my primary display and I can move them to my other display and when I hit maximise it only fills one of the displays. My tray and taskbar are also only on my primary display.

If you have an idea how to create additional taskbars for each of the monitors, and make each taskbar only show tasks from the monitor it is on, do tell. I really miss the functionality I get from using dual display setup under Windows using [Ultramon][1].

 [1]: http://realtimesoft.com/ultramon/overview/