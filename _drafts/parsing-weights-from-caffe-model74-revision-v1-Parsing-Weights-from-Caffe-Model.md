---
id: 106
title: 'Parsing Weights from Caffe Model'
date: '2015-07-08T23:53:38+00:00'
author: weiwen.web
layout: revision
guid: 'http://www.pittnuts.com/2015/07/74-revision-v1/'
permalink: '/?p=106'
---

You may also refer to:  
<span style="color: #0000ff;">[http://nbviewer.ipython.org/github/BVLC/caffe/blob/master/examples/net\_surgery.ipynb](http://nbviewer.ipython.org/github/BVLC/caffe/blob/master/examples/net_surgery.ipynb)</span>  
[https://github.com/BVLC/caffe/blob/master/examples/cpp\_classification/classification.cpp](https://github.com/BVLC/caffe/blob/master/examples/cpp_classification/classification.cpp)  
<http://mochajl.readthedocs.org/en/latest/user-guide/tools/import-caffe-model.html>

Comments for errors in the first link  
**1. ImportError: No module named Image:** Replace “import Image” with “from PIL import Image”  
**2. matplotlib does not show any image** import matplotlib.pyplot as plt  
plt.imshow(im) #doesn’t show any image  
plt.show() # Add this in the last line.

Dig into C++:  
Prerequisites: Google Protobuf.  
Key function to read weights from caffemodel file  
::google::protobuf::Message::ParseFromCodedStream  
Key related class implementation in Caffe  
class NetParameter : public ::google::protobuf::Message {  
…..  
}