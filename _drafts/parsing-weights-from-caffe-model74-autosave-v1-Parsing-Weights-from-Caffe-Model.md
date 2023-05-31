---
id: 77
title: 'Parsing Weights from Caffe Model'
date: '2015-07-02T02:35:48+00:00'
author: weiwen.web
layout: revision
guid: 'http://www.pittnuts.com/2015/07/74-autosave-v1/'
permalink: '/?p=77'
---

Prerequisites: Google Protobuf.  
Key function to read weights from caffemodel file  
::google::protobuf::Message::ParseFromCodedStream  
Key related class implementation in Caffe  
class NetParameter : public ::google::protobuf::Message {  
…..  
}

You may also refer to:  
[https://github.com/BVLC/caffe/blob/master/examples/cpp\_classification/classification.cpp](https://github.com/BVLC/caffe/blob/master/examples/cpp_classification/classification.cpp)

<http://mochajl.readthedocs.org/en/latest/user-guide/tools/import-caffe-model.html>

[http://nbviewer.ipython.org/github/BVLC/caffe/blob/master/examples/net\_surgery.ipynb](http://nbviewer.ipython.org/github/BVLC/caffe/blob/master/examples/net_surgery.ipynb)

Comments for the last link errors

1\. ImportError: No module named Image:  
Replace “import Image” with “from PIL import Image”

2\. matplotlib does not show any image  
import matplotlib.pyplot as plt  
plt.imshow(im) #doesn’t show any image  
Add “plt.show()” in the last line.