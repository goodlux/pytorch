
.. _program_listing_file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_cuda_ATenCUDAGeneral.h:

Program Listing for File ATenCUDAGeneral.h
==========================================

- Return to documentation for :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_cuda_ATenCUDAGeneral.h`

.. code-block:: cpp

   #pragma once
   
   #ifdef _WIN32
   # if defined(ATen_cuda_EXPORTS) || defined(caffe2_gpu_EXPORTS)
   #  define AT_CUDA_API __declspec(dllexport)
   # else
   #  define AT_CUDA_API __declspec(dllimport)
   # endif
   #else
   # define AT_CUDA_API
   #endif
