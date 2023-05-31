---
id: 954
title: 'Remote Debugging in PyCharm Pro (Mac OS)'
date: '2018-09-21T02:21:40+00:00'
author: weiwen.web
layout: revision
guid: 'http://www.pittnuts.com/2018/09/946-revision-v1/'
permalink: '/?p=954'
---

(Only PyCharm Pro supports this. Free for students)

## Create project and use ssh interpreter

Create a &lt;LOCAL PATH&gt;

-&gt;

Create New Project

-&gt;

Browse &lt;LOCAL PATH&gt;

-&gt;

Select Existing interpreter and browse

-&gt;

[![](http://www.pittnuts.com/wp-content/uploads/2018/09/pycharm-ssh1.jpg)](http://www.pittnuts.com/wp-content/uploads/2018/09/pycharm-ssh1.jpg)

Next, configure all settings and finish.

-&gt;

git clone code to &lt;REMOTE PATH&gt;

-&gt;

set “Remote project location” as &lt;REMOTE PATH&gt;

-&gt;

Create

-&gt;right click project -&gt; “Deployment” -&gt; Download from remote server.

## Supporting multi-hop forwarding

```
<pre class="EnlighterJSRAW" data-enlighter-language="shell"># In your local machine
ssh -L 6000:<a final server ip>:22 username@<ip of a server forwarding ssh login>

# In you pycharm, configure ssh being equivalent to
ssh -p 6000 <username of the final server>@localhost
```

<audio controls="controls" style="display: none;"></audio>

<audio controls="controls" style="display: none;"></audio>

<audio controls="controls" style="display: none;"></audio>

<audio controls="controls" style="display: none;"></audio>