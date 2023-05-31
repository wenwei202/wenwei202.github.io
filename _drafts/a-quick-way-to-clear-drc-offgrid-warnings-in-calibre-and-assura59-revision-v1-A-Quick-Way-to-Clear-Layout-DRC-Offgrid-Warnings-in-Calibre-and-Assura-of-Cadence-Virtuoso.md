---
id: 68
title: 'A Quick Way to Clear Layout DRC Offgrid Warnings in Calibre and Assura of Cadence Virtuoso'
date: '2015-07-01T02:26:59+00:00'
author: 'Yandan Wang'
layout: revision
guid: 'http://www.pittnuts.com/2015/07/59-revision-v1/'
permalink: '/?p=68'
---

**A quick way to clear DRC offgrid warnings in Calibre and Assura of Cadence virtuoso:**

**Step 1:** If the grid value of Calibre or Assura rule is 0.01, just like IBM 130nm cmrf8sf, however, we may wrongly set it to some other value, for example 0.001.  
![Offgrid warning in Assura and Calibre of Cadence Virtuosu](http://www.pittnuts.com/pub/img/gridsetting.png)

Then there will come offgrid warnings. The first figure is under Assura and the second is under Calibre.  
![](http://www.pittnuts.com/pub/img/Assura_offGrid.png)  
![](http://www.pittnuts.com/pub/img/Calibre_offGrid_1.png)

**Step 2:** Find the place where there is offgrid warning.  
![](http://www.pittnuts.com/pub/img/Assura_offGrid2.png)

**Step 3:** Change the coordinate to the right one.  
![](http://www.pittnuts.com/pub/img/Assura_offGrid_3.jpg)  
![](http://www.pittnuts.com/pub/img/Assura_offGrid_4.jpg)

**Step 4:** Finally we clear the offgrid warnings. The first figure is under Assura and the second is under Calibre.  
![](http://www.pittnuts.com/pub/img/Assura_offGrid_5.jpg)  
![](http://www.pittnuts.com/pub/img/Calibre_offGrid_2.png)