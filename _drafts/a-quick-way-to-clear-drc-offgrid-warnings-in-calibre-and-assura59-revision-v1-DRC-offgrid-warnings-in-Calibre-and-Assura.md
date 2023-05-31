---
id: 61
title: 'DRC offgrid warnings in Calibre and Assura'
date: '2015-07-01T01:59:48+00:00'
author: 'Yandan Wang'
layout: revision
guid: 'http://www.pittnuts.com/2015/07/59-revision-v1/'
permalink: '/?p=61'
---

A quick way to clear DRC offgrid warnings in Calibre and Assura of Cadence virtuoso:

Step1: If the grid value of Calibre or Assura rule is 0.01, just like IBM 130nm cmrf8sf, however, we may wrongly set it to some other value, for example 0.001.  
[![Debug Caffe in Eclipse Nsight](http://www.pittnuts.com/pub/img/gridsetting.png)](http://www.pittnuts.com/pub/img/gridsetting.png)

Then there will come offgrid warnings.  
[![Debug Caffe in Eclipse Nsight](http://www.pittnuts.com/pub/img/Assura_offGrid.png)](http://www.pittnuts.com/pub/img/Assura_offGrid.png)  
Step 2: