---
id: 349
title: 'GeForce GTX 1080 + CUDA 8.0 + Ubuntu 16.04 + Caffe/TensorFlow'
date: '2016-07-24T18:18:58+00:00'
author: weiwen.web
layout: post
guid: 'http://www.pittnuts.com/?p=349'
permalink: /2016/07/geforce-gtx-1080-cuda-8-0-ubuntu-16-04-caffe/
categories:
    - 'Computer Vision'
    - 'Machine Learning'
    - Programming
    - Tools
---

# Install cuda 8.0 referring [here](http://docs.nvidia.com/cuda/cuda-getting-started-guide-for-linux/#axzz4FHQEeR00).

As the default gcc in Ubuntu 16.04 is very new, if you have compiling error similar to `#error -- unsupported GNU version! gcc versions later than 5.3 are not supported!`

Try to uncomment the #error line in file `/usr/local/cuda/include/host_config.h`

```
<pre class="EnlighterJSRAW" data-enlighter-language="cpp">#if __GNUC__ > 5 || (__GNUC__ == 5 && __GNUC_MINOR__ > 3)

//#error -- unsupported GNU version! gcc versions later than 5.3 are not supported!

#endif /* __GNUC__ > 5 || (__GNUC__ == 5 && __GNUC_MINOR__ > 1) */
```

# Update nvidia driver

You may need to remove the old driver version installed with cuda-8.0 and install a newer version of driver, if you have error similar to `modprobe: ERROR: could not insert 'nvidia_361_uvm': Invalid argument`

Please refer [here](https://codeyarns.com/2013/02/07/how-to-fix-nvidia-driver-failure-on-ubuntu/) to remove old driver, then install a [new one](http://www.nvidia.com/Download/index.aspx?lang=en-us):

```
<pre class="EnlighterJSRAW" data-enlighter-language="shell">sudo apt-get purge nvidia-*
dkms status
sudo dkms remove bbswitch/0.8 -k 4.4.0-31-generic #do this based on your `dkms status`
sudo ./NVIDIA-Linux-x86_64-367.35.run # download the compatible driver (GTX 1080 in my case) from nvidia website and install it
sudo reboot # reboot if the driver was not updated
./cuda8-0-samples/bin/x86_64/linux/release/deviceQuery # compile the sample code and check if it works
```

# Install cudnn V5

cudnn v4 is NOT supported in 1080. You may compile and run, but will not converge.

# Install prerequisities of Caffe

Because the gcc version is Ubuntu 16.04 is very new, If any prerequisity installed from apt-get does not work, uninstall it, compile and install it by the default gcc (5.4) from the source code. The prerequisities that may have problems include: [protobuf](https://github.com/BVLC/caffe/issues/3046) and opencv. E.g. if you have protobuf error similar to

```
<pre class="EnlighterJSRAW" data-enlighter-language="null">.build_release/lib/libcaffe.so: undefined reference to `google::protobuf::io::CodedOutputStream::WriteVarint64ToArray(unsigned long long, unsigned char*)'
```

Try to uninstall the protobuf installed by apt-get, the one installed by apt-get might have been compiled by a older gcc version, so its shared libraries may not be compatible with your default gcc:

```
<pre class="EnlighterJSRAW" data-enlighter-language="shell">sudo apt-get purge libprotobuf-dev protobuf-compiler
```

then compile the [protobuf-2.5.0](https://github.com/google/protobuf/tree/v2.5.0) from src and install it. Please config the default gcc (5.4 in my case) when you compile protobuf:

```
<pre class="EnlighterJSRAW" data-enlighter-language="shell">./configure --prefix=/your/path/ CC=/usr/bin/gcc
make
make check
make install
```

#  Compile and test Caffe [here](http://caffe.berkeleyvision.org/installation.html).

Please also refer [here](https://github.com/BVLC/caffe/wiki/Ubuntu-16.04-or-15.10-Installation-Guide).

If you use anaconda, and get error like `awk: symbol lookup error: $HOME/anaconda2/lib/libreadline.so.6: undefined symbol: PC`

try to remove readline in anaconda lib so as to use the default system one.

```
<pre class="EnlighterJSRAW" data-enlighter-language="shell">conda remove --force readline
```

# Compile and test TensorFlow [here](https://www.tensorflow.org/versions/master/get_started/os_setup.html#installing-from-sources).

If there is error like `ERROR: /mnt/tmp/tensorflow/tensorflow/core/BUILD:87:1: //tensorflow/core:protos_all_py: no such attribute 'imports' in 'py_library' `, newer version (e.g. [0.2.3](https://github.com/bazelbuild/bazel/tree/0.2.3)) of bazel can [solve](https://github.com/tensorflow/tensorflow/issues/1452) this problem.

```
<pre class="EnlighterJSRAW" data-enlighter-language="shell">git clone https://github.com/bazelbuild/bazel.git
cd bazel/
git tag -l
git checkout tags/0.2.3 # or 0.3.0 etc.
./compile.sh
```

“This will create a bazel binary in bazel-bin/src/bazel. This binary is self-contained, so it can be copied to a directory on the PATH (e.g., /usr/local/bin) or used in-place. ”

Use `which bazel` to make sure your bazel is updated.

Then [build and install](https://github.com/tensorflow/tensorflow/blob/master/tensorflow/g3doc/get_started/os_setup.md#installing-from-sources):

```
<pre class="EnlighterJSRAW" data-enlighter-language="shell">./configure
bazel build -c opt --config=cuda //tensorflow/cc:tutorials_example_trainer
# test
bazel-bin/tensorflow/cc/tutorials_example_trainer --use_gpu

# build pip package with gpu support
bazel build -c opt --config=cuda //tensorflow/tools/pip_package:build_pip_package

bazel-bin/tensorflow/tools/pip_package/build_pip_package ~/tmp/tensorflow_pkg

# The name of the .whl file will depend on your platform.
sudo pip install ~/tmp/tensorflow_pkg/tensorflow-0.9.0-py2-none-any.whl

mkdir _python_build
cd _python_build
ln -s ../bazel-bin/tensorflow/tools/pip_package/build_pip_package.runfiles/* .
ln -s ../tensorflow/tools/pip_package/* .
python setup.py develop

cd tensorflow/models/image/mnist
python convolutional.py
```

## Known issues

- TensorFlow compiling @ RHEL

```
<pre class="EnlighterJSRAW" data-enlighter-language="shell">ERROR: /home/wwen/github/tensorflow/tensorflow/core/kernels/BUILD:1529:1: undeclared inclusion(s) in rule '//tensorflow/core/kernels:depth_space_ops_gpu':
this rule is missing dependency declarations for the following files included by 'tensorflow/core/kernels/depthtospace_op_gpu.cu.cc':
  '/fdata/opt/cuda-7.5/include/cuda_runtime.h'
  '/fdata/opt/cuda-7.5/include/host_config.h'
  '/fdata/opt/cuda-7.5/include/builtin_types.h'
  '/fdata/opt/cuda-7.5/include/device_types.h'
  '/fdata/opt/cuda-7.5/include/host_defines.h'
  '/fdata/opt/cuda-7.5/include/driver_types.h'
  '/fdata/opt/cuda-7.5/include/surface_types.h'
  '/fdata/opt/cuda-7.5/include/texture_types.h'
  '/fdata/opt/cuda-7.5/include/vector_types.h'
```

Solution: add cuda include path to `third_party/gpus/crosstool/CROSSTOO` by adding a line similar to `cxx_builtin_include_directory: "/usr/local/cuda-7.5/include"`

- bazel compiling @ RHEL

If no JAVA\_HOME is found when installing bazel from source code, install java sdk and make sure the default one is the one you want

```
<pre class="EnlighterJSRAW" data-enlighter-language="shell">sudo yum install java-1.8.0-openjdk-devel
/usr/sbin/alternatives --config java
```

Add JAVA\_HOME variables in ~/.bashrc:

```
<pre class="EnlighterJSRAW" data-enlighter-language="shell">export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk
export PATH=$PATH:$JAVA_HOME/bin
```

- TensorFlow WORK\_DIR issue

```
<pre class="EnlighterJSRAW" data-enlighter-language="shell">bazel-bin/inception/download_and_preprocess_imagenet "/fdata/imagenet-data"
```

And get error: `bazel-bin/inception/download_and_preprocess_imagenet: line 66: bazel-bin/inception/download_and_preprocess_imagenet.runfiles/inception/data/download_imagenet.sh: No such file or directory`, change WORK\_DIR in `./inception/data/download_and_preprocess_imagenet.sh` to `WORK_DIR="$0.runfiles/__main__/inception"`

.