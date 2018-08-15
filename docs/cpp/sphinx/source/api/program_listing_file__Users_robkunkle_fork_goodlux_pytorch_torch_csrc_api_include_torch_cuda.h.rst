
.. _program_listing_file__Users_robkunkle_fork_goodlux_pytorch_torch_csrc_api_include_torch_cuda.h:

Program Listing for File cuda.h
===============================

- Return to documentation for :ref:`file__Users_robkunkle_fork_goodlux_pytorch_torch_csrc_api_include_torch_cuda.h`

.. code-block:: cpp

   #pragma once
   
   #include <cstddef>
   
   namespace torch {
   namespace cuda {
   size_t device_count();
   
   bool is_available();
   
   bool cudnn_is_available();
   } // namespace cuda
   } // namespace torch
