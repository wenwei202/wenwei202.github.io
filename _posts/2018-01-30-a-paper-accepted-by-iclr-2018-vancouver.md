---
id: 827
title: 'Sparse RNNs work accepted by ICLR 2018'
date: '2018-01-30T03:20:11+00:00'
author: weiwen.web
layout: post
guid: 'http://www.pittnuts.com/?p=827'
permalink: /2018/01/a-paper-accepted-by-iclr-2018-vancouver/
image: /wp-content/uploads/2018/01/6F72E760-CCDA-4AB6-91A3-49499E66FA38-2122-000001201751965D_tmp.jpg
categories:
    - News
---

[![](http://www.pittnuts.com/wp-content/uploads/2018/01/ssl-1.jpg)](http://www.pittnuts.com/wp-content/uploads/2018/01/ssl-1.jpg)

My work ([Learning Intrinsic Sparse Structures within Long Short-Term Memory](https://openreview.net/forum?id=rk6cfpRjZ)) on structurally sparse Recurrent Neural Networks (RNNs) is accepted by ICLR 2018. This is a work collaborated with Microsoft Research and it’s advanced from my [NIPS 2016](http://papers.nips.cc/paper/6504-learning-structured-sparsity-in-deep-neural-networks.pdf) paper on structurally sparse Convolutional Neural Networks (CNNs). Our [source code](https://github.com/wenwei202/iss-rnns) is available on my GitHub. Poster is [here](https://github.com/wenwei202/iss-rnns/blob/master/Poster_Wen_ICLR2018.pdf).

[![](http://www.pittnuts.com/wp-content/uploads/2018/01/MJ-speedup-vs-sparsity-crop.jpg)](http://www.pittnuts.com/wp-content/uploads/2018/01/MJ-speedup-vs-sparsity-crop.jpg)

[![](http://www.pittnuts.com/wp-content/uploads/2018/01/iss-by-group-lasso-crop.jpg)](http://www.pittnuts.com/wp-content/uploads/2018/01/iss-by-group-lasso-crop.jpg)

By learning structurally sparse RNNs, we can reduce the hidden size in RNNs so as to accelerate inference and save memory. The regularity of the sparse weights enables a higher utilization of the peak computing capacity in AI platforms, and gives higher speedup than random connection pruning as shown in the top figure. A later OpenAI work of “[Block-Sparse GPU Kernels](https://blog.openai.com/block-sparse-gpu-kernels/)” proves both the training and inference efficiency of structurally sparse RNNs.

See you in Vancouver!!!

<audio controls="controls" style="display: none;"></audio>

<audio controls="controls" style="display: none;"></audio>

<audio controls="controls" style="display: none;"></audio>