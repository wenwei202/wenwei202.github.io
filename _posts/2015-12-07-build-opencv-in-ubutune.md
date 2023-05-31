---
id: 246
title: 'Build and Install OpenCV 3.0 to Anaconda Python in Ubuntu'
date: '2015-12-07T18:29:53+00:00'
author: weiwen.web
layout: post
guid: 'http://www.pittnuts.com/?p=246'
permalink: /2015/12/build-opencv-in-ubutune/
image: /wp-content/uploads/2015/12/optical_flow-652x288.png
categories:
    - 'Computer Vision'
    - Programming
---

Good references:  
<http://www.pyimagesearch.com/2015/06/22/install-opencv-3-0-and-python-2-7-on-ubuntu/>  
[http://docs.opencv.org/master/d5/de5/tutorial\_py\_setup\_in\_windows.html#gsc.tab=0](http://docs.opencv.org/master/d5/de5/tutorial_py_setup_in_windows.html#gsc.tab=0) [http://docs.opencv.org/master/d7/d9f/tutorial\_linux\_install.html#gsc.tab=0](http://docs.opencv.org/master/d7/d9f/tutorial_linux_install.html#gsc.tab=0)  
<https://scivision.co/anaconda-python-opencv3/>

```
<pre class="EnlighterJSRAW" data-enlighter-language="shell">$ sudo apt-get update
$ sudo apt-get upgrade
$ sudo apt-get install build-essential cmake git pkg-config
$ sudo apt-get install libjpeg8-dev libtiff4-dev libjasper-dev libpng12-dev
$ sudo apt-get install libgtk2.0-dev
$ sudo apt-get install libavcodec-dev libavformat-dev libswscale-dev libv4l-dev
$ sudo apt-get install libatlas-base-dev gfortran
$ sudo apt-get install python2.7-dev

# clone source
$ cd ~
$ git clone https://github.com/Itseez/opencv.git
$ cd opencv
$ git checkout 3.0.0

# SIFT SURF are in opencv_contrib.git
$ cd ~
$ git clone https://github.com/Itseez/opencv_contrib.git
$ cd opencv_contrib
$ git checkout 3.0.0

# 
$ cd ~/opencv
$ mkdir build
$ cd build
$ cmake -DCMAKE_BUILD_TYPE=RELEASE \
 -DOPENCV_EXTRA_MODULES_PATH=~/github/opencv_contrib/modules \
 -DWITH_TBB=ON \
 -DBUILD_NEW_PYTHON_SUPPORT=ON \
 -DINSTALL_C_EXAMPLES=ON \
 -DINSTALL_PYTHON_EXAMPLES=ON \
 -DBUILD_EXAMPLES=ON \
 -DWITH_CUDA=OFF \
 -DBUILD_TIFF=ON \
 -DCMAKE_INSTALL_PREFIX=$(python -c "import sys; print(sys.prefix)") \
 -DPYTHON_EXECUTABLE=$(which python) \
 -DPYTHON_INCLUDE_DIR=$(python -c "from distutils.sysconfig import get_python_inc; print(get_python_inc())") \
 -DPYTHON_PACKAGES_PATH=$(python3 -c "from distutils.sysconfig import get_python_lib; print(get_python_lib())") \
..

# -DCMAKE_INSTALL_PREFIX=/usr/local \ #specify any location you want to install

$ make -j4
$ sudo make install
$ sudo ldconfig

# check if .so are installed in your specified location
# ex. cv2.so should be installed into
# /home/wew57/anaconda/lib/python2.7/site-packages/cv2.so

$ python
>>> import cv2
>>> cv2.__version__
'3.0.0' # is it the version you installed

# taste demo 
opencv/samples/python2/demo.py
```

<figure aria-describedby="caption-attachment-265" class="wp-caption aligncenter" id="attachment_265" style="width: 287px">[![kmeans](http://www.pittnuts.com/wp-content/uploads/2015/12/kmeans-287x300.png)](http://www.pittnuts.com/wp-content/uploads/2015/12/kmeans.png)<figcaption class="wp-caption-text" id="caption-attachment-265">kmeans</figcaption></figure><figure aria-describedby="caption-attachment-266" class="wp-caption aligncenter" id="attachment_266" style="width: 300px">[![optical_flow](http://www.pittnuts.com/wp-content/uploads/2015/12/optical_flow-300x236.png)](http://www.pittnuts.com/wp-content/uploads/2015/12/optical_flow.png)<figcaption class="wp-caption-text" id="caption-attachment-266">optical flow</figcaption></figure>