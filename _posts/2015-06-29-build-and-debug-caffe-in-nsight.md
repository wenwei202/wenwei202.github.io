---
id: 24
title: 'Build and Debug Caffe in Eclipse Nsight'
date: '2015-06-29T19:36:55+00:00'
author: weiwen.web
layout: post
guid: 'http://www.pittnuts.com/?p=24'
permalink: /2015/06/build-and-debug-caffe-in-nsight/
categories:
    - 'Computer Vision'
    - 'Machine Learning'
    - Programming
---

<span style="color: #000000;">For those who want to debug and dig into the source code of Caffe, it may be better to build and debug it in a graphic IDE. This post records my success of building and debugging Caffe in Nvidia’s Eclipse Nsight.</span>

<span style="color: #000000;">I detoured far far away before figuring out this extremely simple solution:</span>

<span style="color: #000000;">Environment: Ubuntu 14.04</span>

<span style="color: #000000;">To build:</span>

<span style="color: #000000;">1. Follow <span style="color: #0000ff;">[Caffe Installation tutorial](http://caffe.berkeleyvision.org/installation.html)</span> to install all prerequisites;</span>

<span style="color: #000000;">2. git clone Caffe to $CAFFE\_ROOT;</span>

<span style="color: #000000;">git clone https://github.com/BVLC/caffe.git $CAFFE\_ROOT</span>

<span style="color: #000000;">3. cp Makefile.config.example Makefile.config and configure your parameters (ex. turn debug mode by uncommenting DEBUG <span class="s2">:= 1</span>);</span>

<span style="color: #000000;">4. run “nsight” in the terimal to launch Nsignt IDE;</span>

<span style="color: #000000;">5. in Nsight, File -&gt; New -&gt; Makefile Project with Existing Code, and browse $CAFFE\_ROOT</span>

<span style="color: #000000;">6. Just build and all is done!!!</span>

<span style="color: #000000;">To debug</span>

<span style="color: #000000;">1. make sure you turned on DEBUG <span class="s2">:= 1 when you built Caffe;</span></span>

<span style="color: #000000;">2. open the source file of the executive binary you want to debug (generally, there is a main function);</span>

<span style="color: #000000;">3. click “Debug”!! (You must add a **Debug Configuration** for every executive program).</span>  
[![Debug Caffe in Eclipse Nsight](http://www.pittnuts.com/pub/img/caffedebug.jpg)](http://www.pittnuts.com/pub/img/caffedebug.jpg)

Fix includes and symbols unfound problem:

1\. include all header folders in Properties -&gt; General -&gt; Paths and Symbols. Use following command to find the default header search paths and add them into the Includes tab  
cpp -x c++ -Wp,-v # for cpp  
cpp -x c -Wp,-v # for c

- - - - - -

Archived (just for backup)

- - - - - -

1\. Follow [Caffe Installation tutorial](http://caffe.berkeleyvision.org/installation.html) to install all prerequisites;

2\. run “nsight” in the terimal to launch Nsignt IDE

3\. new a Cuda c/c++ project in Nsight

4\. git clone Caffe under Nsight project ($CAFFE\_ROOT), and refresh the project folder in Nsight

git clone https://github.com/BVLC/caffe.git

5\. configure “include path” of the compiler (During building, if any header file is unfound, just use “locate/find” command to check where it is and include its location in the build configuration)

6.generate caffe.pb.h manually

protoc src/caffe/proto/caffe.proto –cpp\_out=.  
mkdir include/caffe/proto  
\#mv src/caffe/proto/caffe.pb.h include/caffe/proto (unnecessary if you included $CAFFE\_ROOT/src )

7\. For following error, check if and where you install python, and include /usr/include/python2.7, in my case

../caffe/python/caffe/\_caffe.cpp:1:52: fatal error: Python.h: No such file or directory

8\. fatal error: src/caffe/proto/caffe.pb.h: No such file or directory (Fix it by including $CAFFE\_ROOT)

9\. ../caffe/matlab/+caffe/private/caffe\_.cpp:16:17: fatal error: mex.h: No such file or directory

ensure you installed matlab, “locate” and include the path of mex.h (in my case, /usr/local/MATLAB/R2014b/extern/include)

(Just for backup)…