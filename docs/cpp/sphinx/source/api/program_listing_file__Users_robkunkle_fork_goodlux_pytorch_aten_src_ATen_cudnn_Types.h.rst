
.. _program_listing_file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_cudnn_Types.h:

Program Listing for File Types.h
================================

- Return to documentation for :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_cudnn_Types.h`

.. code-block:: cpp

   #pragma once
   
   #include "cudnn-wrapper.h"
   #include <ATen/Tensor.h>
   
   namespace at { namespace native {
   
   cudnnDataType_t getCudnnDataType(const at::Tensor& tensor);
   
   int64_t cudnn_version();
   
   }}  // namespace at::cudnn
