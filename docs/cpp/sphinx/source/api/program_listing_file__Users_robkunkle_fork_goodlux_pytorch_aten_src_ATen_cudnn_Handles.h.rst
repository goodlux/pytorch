
.. _program_listing_file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_cudnn_Handles.h:

Program Listing for File Handles.h
==================================

- Return to documentation for :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_cudnn_Handles.h`

.. code-block:: cpp

   #pragma once
   
   #include "cudnn-wrapper.h"
   #include "ATen/cuda/ATenCUDAGeneral.h"
   
   namespace at { namespace native {
   
   AT_CUDA_API cudnnHandle_t getCudnnHandle();
   
   }} // namespace
