
.. _program_listing_file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_cuda_CUDAConfig.h:

Program Listing for File CUDAConfig.h
=====================================

- Return to documentation for :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_cuda_CUDAConfig.h`

.. code-block:: cpp

   #pragma once
   
   // Test these using #if AT_CUDNN_ENABLED(), not #ifdef, so that it's
   // obvious if you forgot to include Config.h
   //    c.f. https://stackoverflow.com/questions/33759787/generating-an-error-if-checked-boolean-macro-is-not-defined
   
   #define AT_CUDNN_ENABLED() 0
