---
id: 64
title: 'A Quick Way to Clear DRC Offgrid Warnings in Calibre and Assura'
date: '2015-07-01T02:22:46+00:00'
author: 'Yandan Wang'
layout: revision
guid: 'http://www.pittnuts.com/2015/07/59-autosave-v1/'
permalink: '/?p=64'
---

A quick way to clear DRC offgrid warnings in Calibre and Assura of Cadence virtuoso:

Step 1: If the grid value of Calibre or Assura rule is 0.01, just like IBM 130nm cmrf8sf, however, we may wrongly set it to some other value, for example 0.001.  
![Offgrid warning in Assura and Calibre of Cadence Virtuosu](http://www.pittnuts.com/pub/img/gridsetting.png)

Then there will come offgrid warnings. The first figure is under Assura and the second is under Calibre.  
![Debug Caffe in Eclipse Nsight](http://www.pittnuts.com/pub/img/Assura_offGrid.png)  
![Debug Caffe in Eclipse Nsight](http://www.pittnuts.com/pub/img/Calibre_offGrid_1.png)

Step 2: Find the place where there is offgrid warning.  
![Debug Caffe in Eclipse Nsight](http://www.pittnuts.com/pub/img/Assura_offGrid2.png)

Step 3: Change the coordinate to the right one.  
![Debug Caffe in Eclipse Nsight](http://www.pittnuts.com/pub/img/Assura_offGrid_3.jpg)  
![Debug Caffe in Eclipse Nsight](http://www.pittnuts.com/pub/img/Assura_offGrid_4.jpg)

Step 4: Finally we clear the offgrid warnings. The first figure is under Assura and the second is under Calibre.  
![Debug Caffe in Eclipse Nsight](http://www.pittnuts.com/pub/img/Assura_offGrid_5.jpg)  
![Debug Caffe in Eclipse Nsight](http://www.pittnuts.com/pub/img/Calibre_offGrid_2.png)