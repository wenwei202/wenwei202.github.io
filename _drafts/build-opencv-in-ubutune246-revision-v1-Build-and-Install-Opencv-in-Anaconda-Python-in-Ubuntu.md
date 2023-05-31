---
id: 262
title: 'Build and Install Opencv in Anaconda Python in Ubuntu'
date: '2015-12-07T21:01:53+00:00'
author: weiwen.web
layout: revision
guid: 'http://www.pittnuts.com/2015/12/246-revision-v1/'
permalink: '/?p=262'
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
$ cmake -D CMAKE_BUILD_TYPE=RELEASE \
 -D OPENCV_EXTRA_MODULES_PATH=~/github/opencv_contrib/modules \
 -D WITH_TBB=ON \
 -D BUILD_NEW_PYTHON_SUPPORT=ON \
 -D INSTALL_C_EXAMPLES=ON \
 -D INSTALL_PYTHON_EXAMPLES=ON \
 -D BUILD_EXAMPLES=ON \
 -D WITH_CUDA=OFF \
 -D CMAKE_INSTALL_PREFIX=$(python -c "import sys; print(sys.prefix)") \
 -D PYTHON_EXECUTABLE=$(which python) \
 -D PYTHON_INCLUDE_DIR=$(python -c "from distutils.sysconfig import get_python_inc; print(get_python_inc())") \
 -D PYTHON_PACKAGES_PATH=$(python3 -c "from distutils.sysconfig import get_python_lib; print(get_python_lib())") \
 ..
# -D CMAKE_INSTALL_PREFIX=/usr/local \ #specify any location you want to install

$ make -j4
$ sudo make install
$ sudo ldconfig

# check if so is installed in your specified location
# ex. cv2.so should be updated
# /home/wew57/anaconda/lib/python2.7/site-packages/cv2.so

$ python
>>> import cv2
>>> cv2.__version__
'3.0.0' # is it the version you installed

# taste demo 
opencv/samples/python2/demo.py
```