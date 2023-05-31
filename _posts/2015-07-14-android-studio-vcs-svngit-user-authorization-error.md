---
id: 142
title: 'Android Studio VCS (SVN) User Authorization Error'
date: '2015-07-14T02:55:42+00:00'
author: weiwen.web
layout: post
guid: 'http://www.pittnuts.com/?p=142'
permalink: /2015/07/android-studio-vcs-svngit-user-authorization-error/
categories:
    - Programming
---

[This tutorial](http://www.cs.dartmouth.edu/~campbell/cs65/svn/androidstudio.html) is also most perfect except that the user authorization fails and android studio keeps asking username and password.

To fix this problem:

Mac OS :  
1\) go to Android Studio -&gt; Preferences -&gt; Version Control -&gt; Subversion;  
2\) uncheck “Use command line client” and “Use system default Subversion configuration directory”.

Windows:  
Corresponding checkboxs are under File -&gt; Settings