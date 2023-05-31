---
id: 244
title: Matlab
date: '2015-12-07T03:42:54+00:00'
author: weiwen.web
layout: revision
guid: 'http://www.pittnuts.com/2015/12/242-autosave-v1/'
permalink: '/?p=244'
---

```
<pre class="EnlighterJSRAW" data-enlighter-language="generic">set(0, 'DefaultAxesFontSize', 20); % set default figure font size
set(0, 'DefaultAxesFontName', 'Times New Roman'); % set default figure font

% draw arrows
drawArrow = @(x,y) quiver( x(1),x(2),y(1)-x(1),y(2)-x(2),0,'-r','LineWidth',2,'MaxHeadSize',0.5);
 drawArrow(cur_point,min_point); hold on
```