---
id: 367
title: 'Notebook of Caffe Development'
date: '2016-08-03T17:49:16+00:00'
author: weiwen.web
layout: revision
guid: 'http://www.pittnuts.com/2016/08/137-revision-v1/'
permalink: '/?p=367'
---

Caffe cuDNN error: CUDNN\_STATUS\_SUCCESS (4 vs. 0) CUDNN\_STATUS\_INTERNAL\_ERROR  
Solution: Use CUDA 7.0 instead of 6.5

- - - - - -

Parsing mean and caffemodel from google binary proto  
Parsing mean:

```
<pre class="EnlighterJSRAW" data-enlighter-language="python">    f = open('data/ilsvrc12/imagenet_mean.binaryproto','rb')
    blob = caffe_pb2.BlobProto()#mean file is a BlobProto
    blob.ParseFromString(f.read())
    f.close()
    print blob.ByteSize()
    print blob.height, blob.width,blob.channels, blob.num
    f = open(dstmean,'wb')    
    f.write(blob.SerializeToString())
    f.close()
    print 'Done.'
```

Parsing caffemodel:

```
<pre class="EnlighterJSRAW" data-enlighter-language="python">    f = open('models/bvlc_reference_caffenet/bvlc_reference_caffenet.caffemodel','rb')
    blob = caffe_pb2.NetParameter()#caffemodel is a NetParameter proto
    blob.ParseFromString(f.read())
    f.close()
    print blob.ByteSize()
    print blob.ListFields()
    f = open(dstmodel,'wb')    
    f.write(blob.SerializeToString())
    f.close()
    print 'Done.'
```

- - - - - -

Error: ImportError: No module named lmdb

Solution: conda install -c https://conda.binstar.org/dougal lmdb

- - - - - -

Error: PIL Image, scipy.misc and opencv cv2 all fail to enlarge/resize small images

Environment: Anaconda + Ubuntu  
Solution: bug!!! Debug model fails but release version works well.

- - - - - -

Validating accuracy of Caffe reference model:

- \[50000\] Accuracy (Top 1): 56.7% (Top 5): 79.9%  
    – Used Caffe’s create\_imagenet.sh (images are directly stretched to 256×256. Ratio is not preserved, using python/caffe/imagenet/ilsvrc\_2012\_mean.npy) (Only center crop)
- \[50000\] Accuracy (Top 1): 56.9% (Top 5): 80.0%  
    – Used Caffe’s create\_imagenet.sh (images are directly stretched to 256×256. Ratio is not preserved, using data/ilsvrc12/imagenet\_mean.binaryproto) (Only center crop)
- \[50000\] Accuracy (Top 1): 53.3% (Top 5): 77.4%  
    – Cropped and resized the center to 256×256 using img\_center.resize((w\_target,h\_target),Image.ANTIALIA) (Only center crop) (using python/caffe/imagenet/ilsvrc\_2012\_mean.npy)
- \[50000\] Accuracy (Top 1): 50.8% (Top 5): 75.1%  
    – Cropped and resized the center to 256×256 using img\_center.resize((w\_target,h\_target),Image.NEAREST) (Only center crop) (using python/caffe/imagenet/ilsvrc\_2012\_mean.npy)
- \[50000\] Accuracy (Top 1): 45.8% (Top 5): 70.5%  
    – Naive w recover (using python/caffe/imagenet/ilsvrc\_2012\_mean.npy)
- \[29375\] Accuracy (Top 1): 79.2% (Top 5): 94.0% (Training error) (using python/caffe/imagenet/ilsvrc\_2012\_mean.npy)
- \[50000\] Accuracy (Top 1): 56.7% (Top 5): 79.9% (Equivalent SCNN) (using python/caffe/imagenet/ilsvrc\_2012\_mean.npy)

Validating accuracy of VGG model:

- \[50000\] Accuracy (Top 1): 68.4% (Top 5): 88.5%  
    – Used Caffe’s create\_imagenet.sh (images are directly stretched to 256×256. Ratio is not preserved) (Only center crop) (using python/caffe/imagenet/ilsvrc\_2012\_mean.npy)

- - - - - -

return {out: self.blobs\[out\].data for out in outputs}  
KeyError: ‘relu3’  
HowTo: change your prototxt so that it have relu3 top blobs

Draw caffe net (Warning: group can NOT be shown properly)

- - - - - -

```
<pre class="EnlighterJSRAW" data-enlighter-language="shell">conda install pydot #if you use anaconda python
sudo apt-get install graphviz
cd caffe/
python/draw_net.py \
models/bvlc_reference_caffenet/deploy.prototxt \
models/bvlc_reference_caffenet/deploy.png

export MKL_NUM_THREADS=1 #config maximum threads the mkl uses
```

- - - - - -

**Bug**

In Caffe CPU mode, float may have some precision problem. For example, sum of parameters won’t be accurate. Especially when you use MKL!!!

The test accuracy of the same caffemodel even varies among multiple runs when using MKL?!

When make runtest, use atlas and export following variables:

```
<pre class="EnlighterJSRAW" data-enlighter-language="shell"># bug of runtest
export CUDA_VISIBLE_DEVICES=0 # use one GPU
export MKL_CBWR=AUTO # to avoid precision problem if use mkl
make runtest -j24
```

- - - - - -

If $LD\_LIBRARY\_PATH has anaconda/lib/, make caffe may has error like:  
/usr/local/lib/libopencv\_highgui.so: undefined reference to `TIFFIsTiled@LIBTIFF\_4.0′, this is because the version of libtiff in anaconda may not be the one required by opencv  
Possible solutions:

1. remove anaconda/lib/ in $LD\_LIBRARY\_PATH (You may lose some libraries installed in anaconda/lib)
2. or, locate the system library of tiff ( located in /usr/lib64 in my case), and `export LD_LIBRARY_PATH="/usr/lib64/:$LD_LIBRARY_PATH:/home/wwen/anaconda2/lib";bash`

- - - - - -

```
<pre class="EnlighterJSRAW" data-enlighter-language="shell"># RESULTS OF RUN make runtest when using ATLAS
[ FAILED ] 9 tests, listed below:
[ FAILED ] RMSPropSolverTest/0.TestRMSPropLeastSquaresUpdateWithEverything, where TypeParam = caffe::CPUDevice<float>
[ FAILED ] RMSPropSolverTest/0.TestRMSPropLeastSquaresUpdateWithEverythingShare, where TypeParam = caffe::CPUDevice<float>
[ FAILED ] RMSPropSolverTest/1.TestRMSPropLeastSquaresUpdateWithEverythingShare, where TypeParam = caffe::CPUDevice<double>
[ FAILED ] RMSPropSolverTest/1.TestRMSPropLeastSquaresUpdateWithEverything, where TypeParam = caffe::CPUDevice<double>
[ FAILED ] RMSPropSolverTest/2.TestRMSPropLeastSquaresUpdateWithEverythingShare, where TypeParam = caffe::GPUDevice<float>
[ FAILED ] RMSPropSolverTest/2.TestRMSPropLeastSquaresUpdateWithEverything, where TypeParam = caffe::GPUDevice<float>
[ FAILED ] RMSPropSolverTest/3.TestRMSPropLeastSquaresUpdateWithEverything, where TypeParam = caffe::GPUDevice<double>
[ FAILED ] RMSPropSolverTest/3.TestRMSPropLeastSquaresUpdateWithEverythingShare, where TypeParam = caffe::GPUDevice<double>
[ FAILED ] Program crash when run 'TYPED_TEST(ConvolutionLayerTest, Test0DConvolution)'

# RESULTS OF RUN make runtest when using MKL
[  PASSED  ] 1929 tests.
[  FAILED  ] 19 tests, listed below:
[  FAILED  ] ConvolutionLayerTest/1.TestNDAgainst2D, where TypeParam = caffe::CPUDevice<double>
[  FAILED  ] DeconvolutionLayerTest/1.TestNDAgainst2D, where TypeParam = caffe::CPUDevice<double>
[  FAILED  ] NetTest/1.TestFromTo, where TypeParam = caffe::CPUDevice<double>
[  FAILED  ] SGDSolverTest/0.TestSnapshotShare, where TypeParam = caffe::CPUDevice<float>
[  FAILED  ] AdaGradSolverTest/0.TestSnapshotShare, where TypeParam = caffe::CPUDevice<float>
[  FAILED  ] NesterovSolverTest/0.TestSnapshotShare, where TypeParam = caffe::CPUDevice<float>
[  FAILED  ] AdaDeltaSolverTest/0.TestSnapshotShare, where TypeParam = caffe::CPUDevice<float>
[  FAILED  ] AdamSolverTest/0.TestSnapshotShare, where TypeParam = caffe::CPUDevice<float>
[  FAILED  ] RMSPropSolverTest/0.TestRMSPropLeastSquaresUpdateWithEverything, where TypeParam = caffe::CPUDevice<float>
[  FAILED  ] RMSPropSolverTest/0.TestRMSPropLeastSquaresUpdateWithEverythingShare, where TypeParam = caffe::CPUDevice<float>
[  FAILED  ] RMSPropSolverTest/0.TestSnapshot, where TypeParam = caffe::CPUDevice<float>
[  FAILED  ] RMSPropSolverTest/0.TestSnapshotShare, where TypeParam = caffe::CPUDevice<float>
[  FAILED  ] RMSPropSolverTest/1.TestRMSPropLeastSquaresUpdateWithEverythingShare, where TypeParam = caffe::CPUDevice<double>
[  FAILED  ] RMSPropSolverTest/1.TestRMSPropLeastSquaresUpdateWithEverything, where TypeParam = caffe::CPUDevice<double>
[  FAILED  ] RMSPropSolverTest/2.TestRMSPropLeastSquaresUpdateWithEverything, where TypeParam = caffe::GPUDevice<float>
[  FAILED  ] RMSPropSolverTest/2.TestRMSPropLeastSquaresUpdateWithEverythingShare, where TypeParam = caffe::GPUDevice<float>
[  FAILED  ] RMSPropSolverTest/3.TestRMSPropLeastSquaresUpdateWithEverythingShare, where TypeParam = caffe::GPUDevice<double>
[  FAILED  ] RMSPropSolverTest/3.TestRMSPropLeastSquaresUpdateWithEverything, where TypeParam = caffe::GPUDevice<double>
[ FAILED ] Program crash when run 'TYPED_TEST(ConvolutionLayerTest, Test0DConvolution)'
```

**The reason is you might zerout weights with tiny values.**