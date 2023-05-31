---
id: 272
title: 'Image Segmentation by OpenCV'
date: '2015-12-09T06:13:39+00:00'
author: weiwen.web
layout: revision
guid: 'http://www.pittnuts.com/2015/12/269-autosave-v1/'
permalink: '/?p=272'
---

• Watershed • Graphcut • Gabor wavelet • Adaptive threshold and contour method are explored to do vessel segmentation, the best method is adaptive threshold and contour.

<figure aria-describedby="caption-attachment-270" class="wp-caption aligncenter" id="attachment_270" style="width: 300px">[![Retina](http://www.pittnuts.com/wp-content/uploads/2015/12/RetinaFD-R6-1024-300x300.png)](http://www.pittnuts.com/wp-content/uploads/2015/12/RetinaFD-R6-1024.png)<figcaption class="wp-caption-text" id="caption-attachment-270">Original Retina Image</figcaption></figure>

Python source code:

```
<pre class="EnlighterJSRAW" data-enlighter-language="python">__author__ = 'pittnuts'
import cv2
from numpy import *
from matplotlib import pyplot as plt
import numpy as np

def build_filters():
    filters = []
    ksize = 31
    for theta in np.arange(0, np.pi, np.pi / 16):
        kern = cv2.getGaborKernel((ksize, ksize), 4.0, theta, 10.0, 0.5, 0, ktype=cv2.CV_32F)
        kern /= 1.5*kern.sum()
        filters.append(kern)
    return filters

def gabor_segment(img, filters):
    accum = np.zeros_like(img)
    for kern in filters:
        fimg = cv2.filter2D(img, cv2.CV_8UC1, kern)
        #cv2.imshow('filtered retina {}'.format(kern),fimg[::2,::2])
        np.maximum(accum, fimg, accum)
    return accum

if __name__ == "__main__":
    #imagefile = "../data/RetinaFD-L12-1024.jpg"
    imagefile = "../data/RetinaFD-R6-1024.jpg"
    img = cv2.imread(imagefile)
    orig_dimen = img.shape
    cv2.imshow('original retina',img[::2,::2])
    img = img[:,:,1]
    cv2.imshow('green retina',img[::2,::2])

    hist = cv2.calcHist([img],[0],None,[256],[0,256])
    plt.plot(hist,color='g')

    img_segmented = gabor_segment(img,build_filters())
    cv2.imshow('gabor segmentation',img_segmented[::2,::2])

    #cv2.threshold(img,100,255,cv2.THRESH_BINARY,img_segmented)
    cv2.adaptiveThreshold(img,255,cv2.ADAPTIVE_THRESH_GAUSSIAN_C,cv2.THRESH_BINARY,45,1,img_segmented)
    cv2.imshow('threshold segmentation',img_segmented[::2,::2])
    im2, contours, hierarchy = cv2.findContours(img_segmented,cv2.RETR_TREE,cv2.CHAIN_APPROX_SIMPLE)
    # remove small contours
    for idx in range(len(contours)-1,-1,-1):
        if cv2.contourArea(contours[idx])
```

[![segmentation](http://www.pittnuts.com/wp-content/uploads/2015/12/segmentation.png)](http://www.pittnuts.com/wp-content/uploads/2015/12/segmentation.png)