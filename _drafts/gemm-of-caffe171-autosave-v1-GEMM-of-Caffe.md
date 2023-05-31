---
id: 173
title: 'GEMM of Caffe'
date: '2015-08-31T20:21:47+00:00'
author: weiwen.web
layout: revision
guid: 'http://www.pittnuts.com/2015/08/171-autosave-v1/'
permalink: '/?p=173'
---

```
<pre class="EnlighterJSRAW" data-enlighter-language="cpp">void BaseConvolutionLayer<Dtype>::forward_gpu_gemm(const Dtype* input, const Dtype* weights, Dtype* output, bool skip_im2col) {
  const Dtype* col_buff = input;
  if (!is_1x1_) {
    if (!skip_im2col) {
      conv_im2col_gpu(input, col_buffer_.mutable_gpu_data());
    }
    col_buff = col_buffer_.gpu_data();
  }
  for (int g = 0; g < group_; ++g) {
    caffe_gpu_gemm<Dtype>(CblasNoTrans, CblasNoTrans, conv_out_channels_ /
        group_, conv_out_spatial_dim_, kernel_dim_ / group_,
        (Dtype)1., weights + weight_offset_ * g, col_buff + col_offset_ * g,
        (Dtype)0., output + output_offset_ * g);
  }
}
```

Firstly, expand input feature maps by conv\_im2col\_gpu to matrix **B**. The format is similar to [here](http://petewarden.com/2015/04/20/why-gemm-is-at-the-heart-of-deep-learning/), but each ***column*** stores a cube of input features convoluted by any filter (kernel/weights), so totally output\_width x output\_height columns.

Secondly, caffe\_gpu\_gemm multiplies **B** by weight matrix (**A**) , getting output feature maps **C**, that’s **C**=**A**\***B**. **C** has the same format to **B**, and in weight matrix **A**, each row is a filter (kernel).

Forwarding for different groups and batches are handled by loop. (Slow speed for memory)

Function explanation:

```
<pre class="EnlighterJSRAW" data-enlighter-language="cpp">/*
Remark: ALL data are stored as 1D array. 
Remark: 2D, 3D and 4D can only be imagined by your brain
Function: Input feature maps are expanded to a 2D matrix, imagining expanded data as a 3D array (BLOB_X) makes it easier to understand. 
For this 3D array (noted as 'BLOB_X'): 
  The first and second dimension are the width and height of output feature maps;
  The third dimension is the number of parameters in a 3D kernel
  One executing instance of im2col_gpu_kernel deals with input feature slice connected with a 2D kernel slice (SLICE_Y)
*/
template <typename Dtype>
__global__ void im2col_gpu_kernel(const int n, const Dtype* data_im,
    const int height, const int width, //dimemsion of input feature maps
    const int kernel_h, const int kernel_w,
    const int pad_h, const int pad_w,
    const int stride_h, const int stride_w,
    const int height_col, const int width_col,//dimension of output feature maps
    Dtype* data_col) {
  CUDA_KERNEL_LOOP(index, n) {//index of a SLICE_Y
    int w_out = index % width_col; //offset of the 1st dimension of BLOB_X
    int h_index = index / width_col;
    int h_out = h_index % height_col;//offset of the 2nd dimension of BLOB_X
    int channel_in = h_index / height_col;//offset of the 3rd dimen of SLICE_Y
    int channel_out = channel_in * kernel_h * kernel_w;//offset of the 3rd dimension of BLOB_X
    int h_in = h_out * stride_h - pad_h;//offset of the 1st dimen of SLICE_Y
    int w_in = w_out * stride_w - pad_w;//offset of the 2nd dimen of SLICE_Y
    Dtype* data_col_ptr = data_col;
    data_col_ptr += (channel_out * height_col + h_out) * width_col + w_out;
    const Dtype* data_im_ptr = data_im;
    data_im_ptr += (channel_in * height + h_in) * width + w_in;
    //store values connected with SLICE_Y
    for (int i = 0; i < kernel_h; ++i) {
      for (int j = 0; j < kernel_w; ++j) {
        int h = h_in + i;
        int w = w_in + j;
        *data_col_ptr = (h >= 0 && w >= 0 && h < height && w < width) ?
            data_im_ptr[i * width + j] : 0;
        data_col_ptr += height_col * width_col;//increment 3rd dimen of BLOB_X
      }
    }
  }
}
```

```
<pre class="EnlighterJSRAW" data-enlighter-language="cpp">/*
C=alpha*AB+beta*C
As cublas follows fortran order(colunm first order), this function adapts it to C/C++ (row first order) 
*/
template <>
void caffe_gpu_gemm<double>(const CBLAS_TRANSPOSE TransA,
    const CBLAS_TRANSPOSE TransB, const int M, const int N, const int K,
    const double alpha, const double* A, const double* B, const double beta,
    double* C) {
  // Note that cublas follows fortran order.
  int lda = (TransA == CblasNoTrans) ? K : M;
  int ldb = (TransB == CblasNoTrans) ? N : K;
  cublasOperation_t cuTransA =
      (TransA == CblasNoTrans) ? CUBLAS_OP_N : CUBLAS_OP_T;
  cublasOperation_t cuTransB =
      (TransB == CblasNoTrans) ? CUBLAS_OP_N : CUBLAS_OP_T;
  //C=AB <==> C'=B'A'
  //As fortran order and C/C++ order are transposingly related, transposing is done intrinsically
  CUBLAS_CHECK(cublasDgemm(Caffe::cublas_handle(), cuTransB, cuTransA,
      N, M, K, &alpha, B, ldb, A, lda, &beta, C, N));
}
```

Reference:

> [Why GEMM is at the heart of deep learning](https://petewarden.com/2015/04/20/why-gemm-is-at-the-heart-of-deep-learning/)

<iframe class="wp-embedded-content" data-secret="mVZvoRHvXF" frameborder="0" height="338" marginheight="0" marginwidth="0" sandbox="allow-scripts" scrolling="no" security="restricted" src="https://petewarden.com/2015/04/20/why-gemm-is-at-the-heart-of-deep-learning/embed/#?secret=1n1lfQ9Img#?secret=mVZvoRHvXF" style="position: absolute; clip: rect(1px, 1px, 1px, 1px);" title="“Why GEMM is at the heart of deep learning” — Pete Warden's blog" width="600"></iframe>