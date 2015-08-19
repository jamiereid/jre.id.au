---
title: "Hidden FTP server in Mac OS X Lion"
description: ""
date: 2011-10-14
categories: ["Personal"]
draft: false
---

I'd been using the iOS5 betas for a few months before it was released to the public earlier this week.  While most of the Apps worked as you'd expect, there were a few little bugs here and there, or in some cases features that just didn't work.

In the [GoodReader][1] App for example, the WiFi webdav feature stopped working completely.  This meant that if I wanted to upload entire directories I'd have to do it file by file through iTunes; not a good solution.

Luckly, GoodReader has built in FTP support (and SFTP etc.) which meant that all I needed now was an FTP server for Lion.  A quick google revealed that there are a few FTP server applications out there, but they all cost money.  I wasn't able to find a free or trial version.

Damn.  Back to the drawing board.

Wait.  Doesn't OS X have a Unix backend?  Then surely there is an FTP server somewhere in there.

There is!  Yay!  But it's not immediatly obvious how to turn it on.

To turn on the FTP server run the following command in Terminal.app

<code>sudo launchctl load -w /System/Library/LaunchDaemons/ftp.plist</code>

It will prompt you for your password and then Voila! Your filesystem is being served via FTP.  Use your OS X username and password to authenticate to the server from your client.

To turn off the server you can run the following command

<code>sudo launchctl unload -w /System/Library/LaunchDaemons/ftp.plist</code>

I didn't have any luck with that command (the FTP server was still running), so I had to kill the process manually with <code>kill -9</code>

Problem solved!  And a useful hidden feature of Lion discovered.

[1]:http://www.goodiware.com/goodreader.html
